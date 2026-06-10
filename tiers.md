# Tiers

| Metadata | Value  |
| :------- | :----- |
| Status   | Draft  |

A fully "S3-Compatible" storage service would have [> 300 operations](https://docs.aws.amazon.com/AmazonS3/latest/API/API_Operations.html). Only a tiny subset is needed to do useful work.

The idea here is to split the S3-Compatible storage API into well-defined tiers to provide a simple way for implementors and users to understand what subset of the API their service needs.

## Tier 1: Core

The minimum requirements for a system to function as an object store. Enables basic CRUD operations and is sufficient for static site hosting, backups, and basic web application uploads/downloads.

### Operations

- [`CreateBucket`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CreateBucket.html)
- [`HeadBucket`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_HeadBucket.html)
- [`DeleteBucket`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucket.html)
- [`PutObject` ](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html)
- [`HeadObject`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_HeadObject.html)
- [`GetObject`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html)
- [`DeleteObject`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObject.html)
- [`ListObjectsV2`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjectsV2.html)
- [`ListBuckets`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListBuckets.html)
- [`CopyObject`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CopyObject.html)
- [`DeleteObjects`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObjects.html)

### Capabilities

- **Auth (Headers)**: AWS Signature Version 4 (SigV4) via the `Authorization` header.
- **Transport Security**: HTTPS (TLS 1.2 or higher).
- **Addressing Styles**: Path-Style addressing (`https://endpoint/bucket/key`).
- **CORS (Cross-Origin Resource Sharing)**: MUST support HTTP `OPTIONS` preflight requests and return correct CORS response headers (e.g. `Access-Control-Allow-Origin`) to enable browser-based direct uploads and downloads.


## Tier 2: Reliability

Essential for production workloads handling files larger than 100MB. Ensures reliability over unstable networks via multipart uploads.

### Operations

- [`CreateMultipartUpload`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CreateMultipartUpload.html)
- [`UploadPart`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_UploadPart.html)
- [`CompleteMultipartUpload`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CompleteMultipartUpload.html)
- [`AbortMultipartUpload`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_AbortMultipartUpload.html)
- [`ListParts`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListParts.html)
- [`ListMultipartUploads`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListMultipartUploads.html)

### Capabilities

- **Stateful Upload Tracking**: Ability to track and assemble multiple file parts reliably.


## Tier 3: Advanced

Features required for complex application architectures.

### Operations

- *TBD*

### Capabilities

- *TBD*


## Under Discussion

We are actively seeking feedback, use cases, and contributions on where the following features should be classified. If you have thoughts, please join the discussion on [GitHub issues](https://github.com/cloud-portable/storage/issues).

### Dynamic CORS Configuration (`PUT /bucket?cors`, `GET /bucket?cors`, `DELETE /bucket?cors`)
* **Description**: Set and retrieve cross-origin access rules programmatically on a bucket.
* **Discussion**: Basic CORS response header support is required for Tier 1 to allow frontend direct uploads. Is the ability to *dynamically configure* these rules via S3 APIs necessary for Tier 2 or 3, or should it be left as an out-of-band/provider console setting?

### Presigned URLs (SigV4 Query Authentication)
* **Description**: Allowing clients to perform operations using signature credentials passed directly in the query string.
* **Discussion**: Extremely useful for client-side direct uploads/downloads. Should this live in Tier 2 (alongside multipart uploads for file transfers) or Tier 3 (Advanced)?

### Merging Tiers 1 and 2 (CRUD + Multipart)
* **Description**: Combining Core CRUD and Multipart Uploads into a single baseline tier.
* **Discussion**: Many standard S3 client libraries and CLI tools assume multipart uploads are always available and fail immediately if they are missing. Merging them simplifies developer assumptions but increases the implementation complexity for lightweight or local-only storage backends.

### Out-of-band Bucket Lifecycle (CreateBucket, DeleteBucket)
* **Description**: Managing bucket existence outside of the S3 API interface.
* **Discussion**: For serverless and multi-tenant providers (like Cloudflare R2), bucket creation/deletion are usually done via custom web dashboards or infrastructure-as-code tools (Terraform, CloudFormation). Should bucket CRUD operations be removed from Tier 1 Core, or kept as a requirement?

### Object Versioning (`GET /bucket?versions`, Version IDs)
* **Description**: Storing and retrieving historical versions of objects.
* **Discussion**: Crucial for backup tools, but introduces significant state management complexity for lightweight S3 providers.

### Object Lifecycle Rules (`PUT /bucket?lifecycle`)
* **Description**: Policies to automatically transition objects to cheaper storage classes or delete them after a time limit.
* **Discussion**: Background worker implementation details vary heavily between providers. Is this a Tier 3 requirement or out of scope?
