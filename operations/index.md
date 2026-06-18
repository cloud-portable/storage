# S3 Storage API
Shared schema definitions for S3 operations.

## Overview

Welcome to the S3 Storage API specification index. This index defines the shared structures, data types, enumerations, and error schemas used by S3 operations. The operations themselves are documented in detail in their respective files.

## Operations

| Operation | Description |
| :--- | :--- |
| [`HeadBucket`](HeadBucket.md) | You can use this operation to determine if a bucket exists and if you have permission to access it. |
| [`PutObject`](PutObject.md) | Adds an object to a bucket. |
| [`HeadObject`](HeadObject.md) | The `HEAD` operation retrieves metadata from an object without returning the object itself. |
| [`GetObject`](GetObject.md) | Retrieves an object from Amazon S3. |
| [`DeleteObject`](DeleteObject.md) | Removes an object from a bucket. |
| [`ListObjectsV2`](ListObjectsV2.md) | Returns some or all (up to 1,000) of the objects in a bucket with each request. |
| [`CopyObject`](CopyObject.md) | Creates a copy of an object that is already stored in Amazon S3. |
| [`DeleteObjects`](DeleteObjects.md) | This operation enables you to delete multiple objects from a bucket using a single HTTP request. |
| [`CreateMultipartUpload`](CreateMultipartUpload.md) | This action initiates a multipart upload and returns an upload ID. |
| [`UploadPart`](UploadPart.md) | Uploads a part in a multipart upload. |
| [`UploadPartCopy`](UploadPartCopy.md) | Uploads a part by copying data from an existing object as data source. |
| [`CompleteMultipartUpload`](CompleteMultipartUpload.md) | Completes a multipart upload by assembling previously uploaded parts. |
| [`AbortMultipartUpload`](AbortMultipartUpload.md) | This operation aborts a multipart upload. |
| [`ListParts`](ListParts.md) | Lists the parts that have been uploaded for a specific multipart upload. |
| [`ListMultipartUploads`](ListMultipartUploads.md) | This operation lists in-progress multipart uploads in a bucket. |

