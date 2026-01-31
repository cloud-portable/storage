# Tiers

| Metadata | Value  |
| :------- | :----- |
| Status   | Draft  |

A fully "S3-Compatible" storage service would have [208 operations](https://docs.aws.amazon.com/AmazonS3/latest/API/API_Operations.html). Only a tiny subset is needed to do useful work.

The idea here is to split the idea of an S3-Compatible storage api into well defined tiers to provide a simple way for implementors and users to think about what subset of the API their service needs.

## Tier 1: Core

The minimum requirements for a system to function as an object store. Enables basic CRUD operations. Most static site generators and simple backup tools only need this. 

### Operations

- [`CreateBucket`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CreateBucket.html)
- [`HeadBucket`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_HeadBucket.html)
- [`DeleteBucket`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_control_DeleteBucket.html)
- [`PutObject` ](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html)
- [`HeadObject`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_HeadObject.html)
- [`GetObject`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html)
- [`DeleteObject`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObject.html)
- [`ListObjectsV2`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjectsV2.html)

## Tier 2: Reliability

Essential for production workloads handling files larger than 100MB. Ensures reliability over unstable networks via multipart uploads.

### Operations

- [`CreateMultipartUpload`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CreateMultipartUpload.html)
- [`UploadPart`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_UploadPart.html)
- [`CompleteMultipartUpload`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CompleteMultipartUpload.html)
- [`AbortMultipartUpload`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_AbortMultipartUpload.html)
- [`ListParts`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListParts.html)

## Tier 3: Advanced

TBD - Features required for complex application architectures

### Operations

- [Presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html) - Support v4 signature generation and consumption. Mechanism for delegating permssion to act on a bucket to others.
- ???
