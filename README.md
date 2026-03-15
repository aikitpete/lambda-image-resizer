# serverless-image-resizer

A lightweight **AWS Lambda utility** for resizing images in a serverless workflow.

This function is intended for simple pipelines where images are uploaded, processed, and returned or stored in a resized format without running a dedicated image-processing server.

Typical use cases include:

- resizing user-uploaded images
- generating thumbnails
- preprocessing images before storage or CDN delivery
- integrating image processing into event-driven architectures (e.g. S3 → Lambda)

---

## Architecture

A common deployment pattern:

```
Client upload
      ↓
   Amazon S3
      ↓
   Lambda trigger
      ↓
lambda-image-resizer
      ↓
Resized image written to S3 or returned to caller
```

Because the function runs inside **AWS Lambda**, it automatically scales with incoming requests and does not require server management.

---

## Repository structure

```
function/
    handler.js
    package.json
    node_modules/
```

The `function` directory contains the actual Lambda handler and dependencies.

---

## Build

AWS Lambda requires the function to be uploaded as a **ZIP archive**.

Run the following command to create the deployment package:

```bash
cd function
zip -r -X "../function.zip" *
```

This produces:

```
function.zip
```

Upload `function.zip` to AWS Lambda as the function code.

---

## Deployment

1. Create a Lambda function in AWS.
2. Upload `function.zip`.
3. Configure the handler (for example `handler.handler` depending on the implementation).
4. Attach the appropriate trigger (e.g. **S3 upload event** or **API Gateway**).
5. Configure permissions if writing resized images back to S3.
