# Cloudinary File Storage Service Guide

This guide **explains every concept and code step by step**. It covers:

* What a **signature** is and why it’s needed.
* How the **Spring Boot backend** generates it.
* How the **React/Angular frontend** uses it to upload files securely.
* How to save file URLs in the database.

---

## 🔑 What is a Signature in Cloudinary?

A **signature** is a cryptographic string generated using your **API Secret**. It ensures that:

1. The upload request is authentic (comes from your app).
2. The request has not been tampered with.
3. Cloudinary can trust the upload parameters.

Without a signature, anyone could upload unlimited files to your Cloudinary account → potential abuse & costs. That’s why **signatures must be generated on the backend**, never in frontend code.

---

## 📦 Backend

### 1. Add Dependency

This library allows Java to communicate with Cloudinary.

```xml
<dependency>
  <groupId>com.cloudinary</groupId>
  <artifactId>cloudinary-http44</artifactId>
  <version>1.33.0</version>
</dependency>
```

### 2. Cloudinary Configuration

We configure Cloudinary once so it can be injected everywhere.

```java
@Configuration
public class CloudinaryConfig {
    @Bean
    public Cloudinary cloudinary() {
        Map<String, String> config = ObjectUtils.asMap(
                "cloud_name", System.getenv("CLOUDINARY_CLOUD_NAME"),
                "api_key", System.getenv("CLOUDINARY_API_KEY"),
                "api_secret", System.getenv("CLOUDINARY_API_SECRET")
        );
        return new Cloudinary(config);
    }
}
```

* Uses environment variables (secure).
* Registers a `Cloudinary` bean for reuse.

### 3. Signature Endpoint

This endpoint gives the frontend a valid **signature** + metadata.

```java
@RestController
public class SignatureController {

    private final Cloudinary cloudinary;

    public SignatureController(Cloudinary cloudinary) {
        this.cloudinary = cloudinary;
    }

    @GetMapping("/api/signature")
    public Map<String, Object> getSignature() {
        long timestamp = System.currentTimeMillis() / 1000; // valid for a short time
        Map<String, Object> paramsToSign = new HashMap<>();
        paramsToSign.put("timestamp", timestamp);

        // Create a signed hash using API secret
        String signature = cloudinary.apiSignRequest(paramsToSign, System.getenv("CLOUDINARY_API_SECRET"));

        // Return signature and upload info to frontend
        Map<String, Object> response = new HashMap<>();
        response.put("timestamp", timestamp);
        response.put("signature", signature);
        response.put("cloudName", System.getenv("CLOUDINARY_CLOUD_NAME"));
        response.put("apiKey", System.getenv("CLOUDINARY_API_KEY"));

        return response;
    }
}
```

* **timestamp**: prevents reusing old signatures.
* **signature**: generated hash, proves request is valid.
* **cloudName & apiKey**: needed by frontend to connect.

### 4. Database Storage

We save only the **URL** of the uploaded file, not the file itself.

```java
@Entity
public class FileRecord {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String url;
    // getters and setters
}

public interface FileRecordRepository extends JpaRepository<FileRecord, Long> {}

@RestController
@RequestMapping("/api/files")
public class FileController {
    private final FileRecordRepository repository;
    public FileController(FileRecordRepository repository) { this.repository = repository; }

    @PostMapping
    public FileRecord saveFile(@RequestBody FileRecord fileRecord) {
        return repository.save(fileRecord);
    }
}
```

* Only the URL is persisted → lightweight DB.
* File itself stays on Cloudinary.

---

## 🎨 Frontend (React/Next.js)

```javascript
const handleUpload = async (event) => {
  const file = event.target.files[0];

  // 1️⃣ Get signature from backend
  const sigRes = await fetch("http://localhost:8080/api/signature");
  const { cloudName, apiKey, timestamp, signature } = await sigRes.json();

  // 2️⃣ Upload file to Cloudinary
  const formData = new FormData();
  formData.append("file", file);
  formData.append("api_key", apiKey);
  formData.append("timestamp", timestamp);
  formData.append("signature", signature);

  const uploadRes = await fetch(`https://api.cloudinary.com/v1_1/${cloudName}/image/upload`, {
    method: "POST",
    body: formData,
  });

  const data = await uploadRes.json();

  // 3️⃣ Save URL in backend DB
  await fetch("http://localhost:8080/api/files", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ url: data.secure_url })
  });

  console.log("Uploaded file URL:", data.secure_url);
};
```

* Step 1 → ask backend for signature.
* Step 2 → upload directly to Cloudinary.
* Step 3 → tell backend to store the returned URL.

---

## 🎨 Frontend (Angular)

```typescript
uploadFile(file: File) {
  return this.http.get('http://localhost:8080/api/signature').pipe(
    switchMap((sigRes: any) => {
      // 1️⃣ Use backend signature
      const formData = new FormData();
      formData.append('file', file);
      formData.append('api_key', sigRes.apiKey);
      formData.append('timestamp', sigRes.timestamp);
      formData.append('signature', sigRes.signature);

      // 2️⃣ Upload file to Cloudinary
      return this.http.post(`https://api.cloudinary.com/v1_1/${sigRes.cloudName}/image/upload`, formData).pipe(
        switchMap((res: any) => {
          // 3️⃣ Save URL to backend DB
          return this.http.post('http://localhost:8080/api/files', { url: res.secure_url });
        })
      );
    })
  );
}
```

* Step 1 → Fetch signature from backend.
* Step 2 → Upload file using signature.
* Step 3 → Save URL in backend DB.

---

## ✅ Flow Summary

1. **Frontend** asks backend for a signature.
2. **Backend** creates a signature with API Secret.
3. **Frontend** uploads the file → Cloudinary (using signature).
4. **Cloudinary** validates and stores the file, returns a URL.
5. **Frontend** sends the URL → backend.
6. **Backend** saves the URL in the DB.

---

## 📚 Best Practices

* 🔒 Never expose **API Secret** in frontend.
* 💾 Store only **URLs** in your DB.
* 📂 Use folders for organization.
* ⚡ Optimize images using Cloudinary transformations.

---
