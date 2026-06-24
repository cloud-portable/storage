# Cloud Portable Storage Tier 1

| Metadata | Value  |
| :------- | :----- |
| ID       | CP-001 |
| Title    | Cloud Portable Storage Tier 1 |
| Status   | Experimental  |
| Created  | 2025-11-21 |
| Updated  | 2026-06-18 |
| Authors  | @deaves, @olizilla |

## 1. Abstract

This document defines "Tier 1", the foundational compliance level for the Cloud Portable Storage Standard.

Tier 1 specifies the S3-Compatible API baseline required for basic object storage, sufficient for CRUD operations, static site hosting, and basic backup workflows.

The exact API structures, endpoints, parameters, and headers are defined in the machine-readable specification:
- [Smithy AST JSON](tier-1.smithy.json)

These specifications are reproducibly generated from `tier-1.yaml` using the `storage-spec` CLI. For details on how to regenerate them, see the [Generating Specs Guide](docs/generating-specs.md).

## 2. Tier 1 Operations (Core)

To achieve Tier 1 compatibility, a storage provider MUST support the following 15 operations divided into Core (Single-Part) and Multipart profiles:

### Core Operations (Object CRUD & Discovery)
- **`HeadBucket`** `HEAD /{bucket}` Check bucket existence and access rights.
- **`ListObjectsV2`** `GET /{bucket}?list-type=2` Paginated list of objects.
- **`HeadObject`** `HEAD /{bucket}/{key}` Retrieves object metadata.
- **`GetObject`** `GET /{bucket}/{key}` Retrieves object body and metadata.
- **`PutObject`** `PUT /{bucket}/{key}` Uploads/overwrites an object atomically.
- **`CopyObject`** `PUT /{bucket}/{key}` with `x-amz-copy-source` Copies an object server-side.
- **`DeleteObject`** `DELETE /{bucket}/{key}` Idempotently deletes an object.
- **`DeleteObjects`** `POST /{bucket}?delete` Performs a bulk delete in a single HTTP request.

### Multipart Operations
- **`CreateMultipartUpload`** `POST /{bucket}/{key}?uploads` Initiates a multipart upload session.
- **`UploadPart`** `PUT /{bucket}/{key}?uploadId={id}&partNumber={n}` Uploads an individual chunk.
- **`UploadPartCopy`** `PUT /{bucket}/{key}?uploadId={id}&partNumber={n}` with `x-amz-copy-source` Copies a chunk server-side.
- **`CompleteMultipartUpload`** `POST /{bucket}/{key}?uploadId={id}` Assembles all parts into a finished object.
- **`AbortMultipartUpload`** `DELETE /{bucket}/{key}?uploadId={id}` Cancels the session and deletes uploaded chunks.
- **`ListParts`** `GET /{bucket}/{key}?uploadId={id}` Lists uploaded pa for an active session.
- **`ListMultipartUploads`** `GET /{bucket}?uploads` Lists all active multipart uploads for a bucket.

## 3. Protocol & Authentication Requirements

- **Authentication**: Providers MUST support AWS Signature Version 4 (SigV4) authentication via the `Authorization` header.
- **Addressing Styles**: Providers SHOULD support Path-Style addressing (`https://endpoint/{bucket}/{key}`).
