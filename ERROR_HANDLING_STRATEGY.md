# Error Handling Strategy - E-Assurance Project

## Overview

This document outlines the comprehensive error handling strategy implemented in the E-Assurance Spring Boot application. The system provides consistent, structured error responses across all API endpoints using a centralized approach.

## Architecture Components

### 1. Core Error Handling Classes

#### `ApiError` Enum (`com.assurix.e_assurance.Exceptions.ApiError`)
- **Purpose**: Centralized error definitions with consistent structure
- **Components**: Each error contains:
  - `HttpStatus` - HTTP status code (404, 400, 500, etc.)
  - `message` - User-friendly error message
  - `errorCode` - Unique identifier for the error type
  - `details` - Technical description for developers

#### `ApiException` Class (`com.assurix.e_assurance.Exceptions.ApiException`)
- **Purpose**: Custom runtime exception that wraps ApiError
- **Usage**: Thrown throughout services and controllers
- **Structure**: 
  ```java
  public class ApiException extends RuntimeException {
      private final ApiError apiError;
      // Constructors and getters
  }
  ```

#### `ApiResponse<T>` DTO (`com.assurix.e_assurance.dto.ApiResponse`)
- **Purpose**: Standardized response wrapper for all API endpoints
- **Structure**:
  ```java
  public class ApiResponse<T> {
      private int statusCode;
      private String message;
      private String errorCode;  // null for success responses
      private String details;    // null for success responses
      private T data;           // null for error responses
  }
  ```

### 2. Service Layer Components

#### `ResponseBuilderService` (`com.assurix.e_assurance.services.ResponseBuilderService`)
- **Purpose**: Utility service for building consistent API responses
- **Methods**:
  - `success(T data, String message, HttpStatus status)` - Build success responses
  - `error(ApiError error, Exception exception)` - Build error responses with exception details
  - `error(ApiError error)` - Build error responses without exception details

#### `GlobalExceptionHandler` (`com.assurix.e_assurance.Exceptions.GlobalExceptionHandler`)
- **Purpose**: Centralized exception handling using Spring's `@ControllerAdvice`
- **Handles**:
  - `ApiException` - Custom business logic exceptions
  - `NoSuchElementException` - Entity not found scenarios
  - `AccessDeniedException` - Authorization failures
  - `MethodArgumentNotValidException` - Validation errors
  - `Exception` - Generic fallback for unexpected errors

## Error Categories

### 1. General Errors (GENERAL_xxx)
```java
INTERNAL_ERROR(HttpStatus.INTERNAL_SERVER_ERROR, "An unexpected error occurred.", "GENERAL_001", "Unexpected server error")
BAD_REQUEST(HttpStatus.BAD_REQUEST, "Invalid request.", "GENERAL_002", "Malformed or missing parameters")
METHOD_NOT_ALLOWED(HttpStatus.METHOD_NOT_ALLOWED, "Method not allowed.", "GENERAL_003", "HTTP method is not supported for this endpoint")
```

### 2. User Management (USER_xxx)
```java
USER_NOT_FOUND(HttpStatus.NOT_FOUND, "User not found.", "USER_001", "No user exists with the given identifier")
USER_UNAUTHORIZED(HttpStatus.UNAUTHORIZED, "Unauthorized access.", "USER_002", "User is not authenticated or lacks permission")
USER_ALREADY_EXISTS(HttpStatus.CONFLICT, "User already exists.", "USER_003", "A user with this email or identifier already exists")
```

### 3. Authentication (AUTH_xxx)
```java
AUTH_INVALID_CREDENTIALS(HttpStatus.UNAUTHORIZED, "Invalid credentials.", "AUTH_001", "Email or password is incorrect")
AUTH_2FA_REQUIRED(HttpStatus.FORBIDDEN, "2FA required.", "AUTH_002", "Two-factor authentication is required for this account")
AUTH_TOKEN_INVALID(HttpStatus.UNAUTHORIZED, "Invalid or expired token.", "AUTH_004", "The provided authentication token is invalid or expired")
```

### 4. Vehicle Management (VEHICLE_xxx)
```java
VEHICLE_NOT_FOUND(HttpStatus.NOT_FOUND, "Vehicle not found.", "VEHICLE_001", "No vehicle exists with the given identifier")
VEHICLE_FORBIDDEN(HttpStatus.FORBIDDEN, "Access denied to vehicle.", "VEHICLE_002", "User does not have access to this vehicle")
```

### 5. Claims Management (CLAIM_xxx)
```java
CLAIM_NOT_FOUND(HttpStatus.NOT_FOUND, "Claim not found.", "CLAIM_001", "No claim exists with the given identifier")
CLAIM_FORBIDDEN(HttpStatus.FORBIDDEN, "Access denied to claim.", "CLAIM_002", "User does not have access to this claim")
CLAIM_INVALID(HttpStatus.BAD_REQUEST, "Invalid claim data.", "CLAIM_003", "Provided claim data is invalid or incomplete")
```

### 6. Registration & OTP (OTP_xxx, REGISTER_xxx)
```java
OTP_SEND_FAILED(HttpStatus.INTERNAL_SERVER_ERROR, "Failed to send OTP.", "OTP_001", "Error sending OTP email")
OTP_INVALID(HttpStatus.BAD_REQUEST, "Invalid OTP code.", "OTP_002", "The provided OTP code is invalid or expired")
EMAIL_ALREADY_EXISTS(HttpStatus.CONFLICT, "Email is already registered", "EMAIL_ALREADY_EXISTS", "Email is already registered")
```

### 7. Identity Verification (IDENTITY_xxx, OCR_xxx)
```java
IDENTITY_VERIFICATION_FAILED(HttpStatus.BAD_REQUEST, "Identity verification failed.", "REGISTER_002", "Failed to verify identity document")
OCR_NAME_MISMATCH(HttpStatus.UNPROCESSABLE_ENTITY, "Extracted name does not match registered name", "OCR_NAME_MISMATCH", "Extracted name does not match registered name")
```

## Implementation Patterns

### 1. Service Layer Error Handling

#### Pattern: Check-and-Throw
```java
public void register(RegisterRequest dto) {
    // Check for existing user
    if (userRepo.findByEmail(dto.getEmail()).isPresent()) {
        throw new ApiException(ApiError.EMAIL_ALREADY_EXISTS);
    }
    
    // Business logic here
    User user = new User();
    // ... setup user
    
    try {
        otpService.generateAndSendOtp(user.getEmail(), user.getName());
    } catch (Exception e) {
        throw new ApiException(ApiError.EMAIL_SEND_FAILED, e.getMessage());
    }
}
```

#### Pattern: Entity-Not-Found
```java
public User getUserById(Long id) {
    return userRepo.findById(id)
        .orElseThrow(() -> new ApiException(ApiError.USER_NOT_FOUND, "User ID: " + id));
}
```

#### Pattern: Validation with Context
```java
public void verifyOtp(String email, String rawCode) {
    try {
        otpService.validateOtp(email, rawCode);
    } catch (ApiException e) {
        throw new ApiException(ApiError.OTP_INVALID, e.getMessage());
    } catch (RuntimeException e) {
        throw new ApiException(ApiError.OTP_EXPIRED, e.getMessage());
    }
    
    User user = userRepo.findByEmail(email)
        .orElseThrow(() -> new ApiException(ApiError.USER_NOT_FOUND, "Email: " + email));
    
    user.setEmailVerified(true);
    userRepo.save(user);
}
```

### 2. Controller Layer Patterns

#### Pattern: Success Response
```java
@PostMapping("/create")
public ResponseEntity<ApiResponse<Object>> createClaim(@RequestBody ClaimRequestDto claimRequest) {
    claimService.createClaim(claimRequest);
    return responseBuilder.success(null, "Claim created successfully", HttpStatus.CREATED);
}
```

#### Pattern: Data Response
```java
@GetMapping("/calculate-premium")
public ResponseEntity<ApiResponse<PremiumCalculationDTO>> calculatePremium(@RequestParam String policyType) {
    PremiumCalculationDTO dto = insurancePolicyService.calculatePremium(policyType);
    return responseBuilder.success(dto, "Premium calculated", HttpStatus.OK);
}
```

### 3. Global Exception Handling

#### Pattern: Specific Exception Mapping
```java
@ExceptionHandler(ApiException.class)
public ResponseEntity<ApiResponse<Object>> handleApiException(ApiException ex) {
    return responseBuilder.error(ex.getApiError(), ex);
}

@ExceptionHandler(NoSuchElementException.class)
public ResponseEntity<ApiResponse<Object>> handleNotFound(NoSuchElementException ex) {
    return responseBuilder.error(ApiError.USER_NOT_FOUND, ex);
}
```

#### Pattern: Validation Error Handling
```java
@Override
protected ResponseEntity<Object> handleMethodArgumentNotValid(
        MethodArgumentNotValidException ex,
        HttpHeaders headers,
        HttpStatusCode status,
        WebRequest request) {
        
    String details = ex.getBindingResult().getFieldErrors().stream()
            .map(e -> e.getField() + ": " + e.getDefaultMessage())
            .reduce("", (a, b) -> a + "; " + b);

    ApiResponse<?> response = ApiResponse.builder()
            .statusCode(ApiError.VALIDATION_ERROR.getHttpStatus().value())
            .message(ApiError.VALIDATION_ERROR.getMessage())
            .errorCode(ApiError.VALIDATION_ERROR.getErrorCode())
            .details(details)
            .data(null)
            .build();

    return new ResponseEntity<>(response, headers, ApiError.VALIDATION_ERROR.getHttpStatus());
}
```

## Response Format Examples

### Success Response
```json
{
  "statusCode": 200,
  "message": "Claim created successfully",
  "errorCode": null,
  "details": null,
  "data": null
}
```

### Error Response
```json
{
  "statusCode": 404,
  "message": "User not found.",
  "errorCode": "USER_001",
  "details": "No user exists with the given identifier",
  "data": null
}
```

### Validation Error Response
```json
{
  "statusCode": 400,
  "message": "Validation failed.",
  "errorCode": "VALIDATION_001",
  "details": "email: must be a well-formed email address; password: size must be between 8 and 20",
  "data": null
}
```

## Best Practices for Implementation

### 1. Adding New Error Types
```java
// In ApiError enum
NEW_ERROR_TYPE(HttpStatus.BAD_REQUEST, "User-friendly message", "CATEGORY_XXX", "Technical details for developers")
```

### 2. Service Layer Guidelines
- Always throw `ApiException` with appropriate `ApiError`
- Include contextual information in exception messages
- Use try-catch blocks for external service calls
- Validate input parameters before processing

### 3. Controller Layer Guidelines
- Let `GlobalExceptionHandler` handle all exceptions
- Use `ResponseBuilderService` for all responses
- Don't catch exceptions in controllers unless specific handling is needed
- Always return consistent `ApiResponse<T>` format

### 4. Error Code Naming Convention
- Use category prefix (USER_, VEHICLE_, CLAIM_, etc.)
- Use sequential numbering (001, 002, 003...)
- Keep codes unique across the entire application

### 5. HTTP Status Code Guidelines
- `200 OK` - Successful operations
- `201 CREATED` - Resource creation
- `400 BAD_REQUEST` - Client input errors
- `401 UNAUTHORIZED` - Authentication required
- `403 FORBIDDEN` - Access denied
- `404 NOT_FOUND` - Resource not found
- `409 CONFLICT` - Resource conflicts (duplicate email)
- `422 UNPROCESSABLE_ENTITY` - Business logic validation failures
- `500 INTERNAL_SERVER_ERROR` - Server errors

## Testing Error Handling

### Unit Testing Services
```java
@Test
public void testRegister_EmailAlreadyExists_ThrowsException() {
    // Given
    when(userRepo.findByEmail("test@example.com")).thenReturn(Optional.of(new User()));
    
    // When & Then
    ApiException exception = assertThrows(ApiException.class, () -> {
        registerService.register(new RegisterRequest("test@example.com", "password", "Test User"));
    });
    
    assertEquals(ApiError.EMAIL_ALREADY_EXISTS, exception.getApiError());
}
```

### Integration Testing Controllers
```java
@Test
public void testCreateUser_InvalidInput_ReturnsValidationError() throws Exception {
    mockMvc.perform(post("/api/auth/register")
            .contentType(MediaType.APPLICATION_JSON)
            .content("{\"email\":\"invalid-email\",\"password\":\"123\"}"))
            .andExpect(status().isBadRequest())
            .andExpect(jsonPath("$.errorCode").value("VALIDATION_001"))
            .andExpect(jsonPath("$.statusCode").value(400));
}
```

## Monitoring and Logging

### Recommended Logging Pattern
```java
@ExceptionHandler(ApiException.class)
public ResponseEntity<ApiResponse<Object>> handleApiException(ApiException ex) {
    log.warn("API Exception occurred: {} - {}", ex.getApiError().getErrorCode(), ex.getMessage());
    return responseBuilder.error(ex.getApiError(), ex);
}

@ExceptionHandler(Exception.class)
public ResponseEntity<ApiResponse<Object>> handleGeneral(Exception ex) {
    log.error("Unexpected error occurred", ex);
    return responseBuilder.error(ApiError.INTERNAL_ERROR, ex);
}
```

## Future Enhancements

1. **Internationalization**: Add support for multiple languages in error messages
2. **Error Analytics**: Implement error tracking and metrics collection
3. **Client-Specific Errors**: Add different error detail levels for different client types
4. **Retry Mechanisms**: Implement automatic retry for transient errors
5. **Circuit Breaker**: Add circuit breaker pattern for external service failures

This error handling strategy ensures consistent, maintainable, and user-friendly error responses across the entire E-Assurance application.
