# Cloud Portable Storage Tier 1

| Metadata | Value  |
| :------- | :----- |
| ID       | CP-001 |
| Title    | Cloud Portable Storage Tier 1 |
| Status   | Draft  |
| Created  | 2025-11-21 |
| Authors  | @deaves, @olizilla |

## 1. Abstract

This document defines "Tier 1", the foundational compliance level for the Cloud Portable Storage Standard.

Tier 1 specifies the S3-Compatible API baseline required for basic object storage, sufficient for CRUD operations, static site hosting, and basic backup workflows.

The exact API structures, endpoints, parameters, and headers are defined in the machine-readable specifications:
- [Smithy AST](rfc-storage-tier-1.smithy.json)
- [OpenAPI 3.1 YAML](rfc-storage-tier-1.openapi.yaml)

These specifications are reproducibly generated from `tiers.md` using the `storage-spec` CLI. For details on how to regenerate them, see the [Generating Specs Guide](docs/generating-specs.md).

## 2. Tier 1 Operations

To achieve Tier 1 compatibility, a storage provider MUST support the following 11 operations:

### Bucket Operations
- **`CreateBucket`** (`PUT /bucket`): Creates a new bucket.
- **`DeleteBucket`** (`DELETE /bucket`): Deletes an empty bucket. Must return 409 Conflict if not empty.
- **`HeadBucket`** (`HEAD /bucket`): Checks bucket existence and access rights.
- **`ListBuckets`** (`GET /`): Lists all buckets owned by the sender.
- **`ListObjectsV2`** (`GET /bucket?list-type=2`): Lists objects with modern pagination.

### Object Operations
- **`PutObject`** (`PUT /bucket/key`): Uploads/overwrites an object atomically.
- **`GetObject`** (`GET /bucket/key`): Retrieves object body and metadata.
- **`HeadObject`** (`HEAD /bucket/key`): Retrieves object metadata (headers/size) without body.
- **`DeleteObject`** (`DELETE /bucket/key`): Idempotently deletes an object.
- **`DeleteObjects`** (`POST /bucket?delete`): Performs a bulk delete in a single HTTP request.
- **`CopyObject`** (`PUT /bucket/key` with `x-amz-copy-source`): Copies an object server-side.

## 3. Protocol & Authentication Requirements

- **Transport Security**: Providers MUST support HTTPS (TLS 1.2 or higher).
- **Authentication**: Providers MUST support AWS Signature Version 4 (SigV4) authentication via the `Authorization` header.
- **Addressing Styles**: Providers SHOULD support Path-Style addressing (`https://endpoint/bucket/key`).


