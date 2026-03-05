# Cloud Portable Storage

S3-compatible APIs have become the de-facto standard for digital storage.

This repository establishes an open specification for a minimal S3-compatible API subset, enabling storage interoperability across providers.

## Defining S3 Compatibility

While there is no official standards body definition of "S3-compatible," the ceph/s3-tests conformance suite (500+ tests) provides a practical, testable definition. This specification uses ceph/s3-tests pass rates to define two compliance tiers, enabling government procurement officers and enterprise decision-makers to specify concrete compatibility requirements and prevent vendor lock-in.

## Why S3 is the De Facto Standard

- **Universal SDK Support**: Mature S3 client libraries exist for Python (boto3), JavaScript (AWS SDK), Java, Go, .NET, Ruby, PHP, and Rust
- **Cloud Provider Convergence**: Major cloud providers offer S3-compatible object storage, including Cloudflare R2, Backblaze B2, DigitalOcean Spaces, Oracle Cloud, IBM Cloud, Wasabi, and Scaleway
- **Tool Ecosystem**: Thousands of applications and tools work exclusively with S3 APIs—backup software, data pipeline tools, static site generators, media processing systems, and data analytics platforms
- **Open Source Implementation**: Multiple independent open-source implementations (MinIO, Ceph, Garage, SeaweedFS) validate that the API is well-specified enough to replicate

The S3 API is the only object storage interface with this level of ecosystem adoption.

## Why This Matters

Procurement requirements that do not specify API compatibility standards result in incompatible storage systems, reduced competition, and vendor lock-in.

By specifying S3 compatibility tiers in procurement requirements, agencies can:
- Enable genuine competition among vendors on price and service quality
- Prevent lock-in to any single provider
- Maintain data portability and migration options
- Support domestic providers while preserving access to international tools and ecosystems

## Compliance Tiers

S3 compatibility is sufficiently widespread that most providers support the same core feature set, including multipart uploads, presigned URLs, CORS, and lifecycle management. This specification defines two tiers: Production Ready, which covers the features common to nearly all providers, and Encryption Ready, which adds customer-controlled encryption for sensitive data.

### Tier 1: Production Ready
**Purpose:** Works with all major tools, SDKs, and large files
**Use Cases:** Production web applications, media storage, data lakes, backup systems
**Compliance Threshold:** ≥95% pass rate on ceph/s3-tests Production Operations subset

**Required Operations:**
- Bucket operations: CreateBucket, DeleteBucket, HeadBucket, ListBuckets, GetBucketLocation
- Object operations: PutObject, GetObject, HeadObject, DeleteObject, DeleteObjects, ListObjects/ListObjectsV2, CopyObject
- Complete multipart upload support (CreateMultipartUpload, UploadPart, CompleteMultipartUpload, AbortMultipartUpload, ListParts, ListMultipartUploads)
- Presigned URLs for GET and PUT operations
- Virtual-host style and path-style URLs
- Range requests and conditional operations (If-Match, If-None-Match, If-Modified-Since)
- CORS configuration
- Lifecycle management
- Custom metadata support (x-amz-meta-* headers)
- AWS Signature Version 4 authentication
- Full compatibility with AWS CLI, boto3, rclone, s3cmd, and major S3 SDKs

### Tier 2: Encryption Ready
**Purpose:** Tier 1 plus customer-controlled encryption for sensitive data
**Use Cases:** Government classified or sensitive data, healthcare, financial services, any environment where the storage provider must not be able to read stored data
**Compliance Threshold:** ≥98% pass rate on ceph/s3-tests Full Suite

**All Tier 1 Requirements PLUS:**
- Server-side encryption with customer-provided keys (SSE-C): the customer provides an encryption key with each request; the provider encrypts/decrypts data but never stores the key

## Recommended Procurement Language

> *"Storage services must demonstrate compliance with Tier 1: Production Ready of the S3 API Compatibility Specification v1.0, validated through successful completion of the ceph/s3-tests conformance suite with ≥95% pass rate."*

For environments handling sensitive data:

> *"Storage services must demonstrate compliance with Tier 2: Encryption Ready, including support for SSE-C (Server-Side Encryption with Customer-Provided Keys), validated through ≥98% pass rate on the full ceph/s3-tests suite."*

## Implementors

### Open Source
Ceph (RGW), Garage, MinIO, OpenStack Swift, SeaweedFS, Zenko (CloudServer)

### Cloud Services
AWS S3, Backblaze B2, Cloudflare R2, DigitalOcean Spaces, Google Cloud Storage, IBM Cloud Object Storage, IDrive e2, Linode, Oracle Cloud, Scaleway, Tigris, Wasabi

## Core Principles

- Recognize and build upon de facto standards
- Prioritize working suboptimal systems over idealized perfection
- Emphasize simplicity and interoperability over incremental additional value
- Enable sovereignty through portability rather than complete infrastructure ownership

This approach commodifies mature technologies and increases provider optionality, allowing domestic and international vendors to compete on service quality rather than lock-in.

## Engagement

This specification uses an open RFC process. Contributions occur through GitHub issues and pull requests.

Moderated by @deaves and @olizilla.

## Full Specification

See [S3 API Compatibility Specification v1.0](S3_API_Compatibility_Specification_v1.0_1.md) for detailed API requirements, conformance testing procedures, and implementation guidance.

## References

- [ceph/s3-tests](https://github.com/ceph/s3-tests) - Industry standard for S3 compatibility testing
- [AWS S3 API Reference](https://docs.aws.amazon.com/AmazonS3/latest/API/)
