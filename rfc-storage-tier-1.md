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

It specifies the minimal subset of an S3-Compatible API that a storage service MUST implement to be considered "Cloud Portable Storage Compatible." This tier focuses on fundamental Create, Read, Update, and Delete (CRUD) operations for Buckets and Objects, sufficient for basic file storage, static site hosting, and simple backup workflows.

## 2. Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

* Provider: The storage service implementing this standard.  
* Client: The software agent issuing HTTP requests to the Provider.  
* Bucket: A container for objects.  
* Object: A file and any optional metadata.

## 3. Motivation

The S3 API contains over 100 distinct operations, many of which are proprietary, legacy, or rarely used (e.g., SelectObjectContent, RestoreObject). This complexity creates fragmentation where "S3 Compatible" tools fail unpredictably across different providers. Tier 1 establishes a strict, verifiable baseline that guarantees basic interoperability for the 80% use case: storing and retrieving files.

## 4. Authentication & Connectivity

### 4.1. Transport Security

Providers MUST support HTTPS (TLS 1.2 or higher).

### 4.2. Authentication Signature

Providers MUST support AWS Signature Version 4 (SigV4) authentication.

* Requests MUST be accepted via the Authorization header.  
* Providers MAY support Presigned URL authentication (query parameters), but it is not required for Tier 1 compliance.

### 4.3. Addressing Models

Providers SHOULD support Path-Style addressing (https://endpoint/bucket/key).  
Providers MAY support Virtual-Hosted-Style addressing (https://bucket.endpoint/key).

## 5. Specification

### 5.1. CreateBucket (`PUT /bucket`)

Creates a new bucket.

* Request:  
  * MUST accept a PUT request to the bucket resource.  
  * MUST handle bucket naming conflicts (return 409 Conflict or 403 Forbidden if owned by another).  
* Response:  
  * MUST return 200 OK on success.  
  * MUST return a Location header.

### 5.2. DeleteBucket (`DELETE /bucket`)

Deletes an existing empty bucket.

* Request: 
  * MUST accept a DELETE request.  
* Behavior: 
  * MUST succeed if the bucket is empty.  
  * MUST return 409 Conflict (BucketNotEmpty) if the bucket contains objects.  
* Response: 
  * MUST return 204 No Content on success.

### 5.3. HeadBucket (`HEAD /bucket`)

Determines if a bucket exists and if the user has permission to access it.

* Response:  
  * MUST return 200 OK if the bucket exists and is accessible.
  * MUST return 404 Not Found if the bucket does not exist.
  * MUST return 403 Forbidden if the bucket exists but access is denied.

### 5.4. PutObject (`PUT /bucket/key`)

Uploads an object.

* Request:  
  * MUST support the Content-Type header.
  * MUST support Content-Length.
* Behavior: 
  * MUST overwrite existing objects with the same key.
  * MUST atomically persist the data.
* Response:  
  * MUST return 200 OK.
  * MUST return an ETag header containing the MD5 hash (hex-encoded) of the object data.

### 5.5. GetObject (`GET /bucket/key`)

Retrieves an object.

* Response:
  * MUST return 200 OK.
  * MUST return the object body.
  * MUST return Content-Type, Content-Length, Last-Modified, and ETag headers matching the stored object.
  * MUST return 404 Not Found if the key does not exist.

### 5.6. DeleteObject (`DELETE /bucket/key`)

Removes the an object.

TBD: Versioning

* Response:  
  * MUST return 204 No Content on success.  
  * MUST return 204 No Content even if the object did not exist (idempotency), unless a specific version ID was requested (not covered in Tier 1).

### 5.7. HeadObject (`HEAD /bucket/key`)

Retrieves metadata from an object without returning the object itself.

* Response:  
  * MUST return the same HTTP headers as GetObject (Content-Type, Content-Length, ETag, Last-Modified).
  * MUST return 404 Not Found if the object does not exist.

### 5.8. ListObjectsV2 (`GET /bucket?list-type=2`)

Returns some or all (up to 1,000) of the objects in a bucket.

* Request Parameters:
  * MUST support prefix: Limits the response to keys that begin with the specified prefix.
  * MUST support continuation-token: Pagination token.
  * MUST support max-keys: Sets the maximum number of keys returned in the response (default 1000).
* Response Body (XML):
  * MUST return valid XML conforming to the S3 ListBucketResult schema.
  * MUST include \<Contents\> elements for objects.
  * Inside \<Contents\>, MUST include: \<Key\>, \<LastModified\>, \<ETag\>, \<Size\>.
  * MUST include \<IsTruncated\> boolean to indicate if more results exist.
  * MUST include \<NextContinuationToken\> if IsTruncated is true.

## 6. Error Handling

Providers MUST return errors in XML format (unless the request was HEAD).

* Structure:
```xml
<Error\>  
  <Code\>ErrorCodeString\</Code\>  
  <Message\>Descriptive message\</Message\>  
  <Resource\>/mybucket/myfoto.jpg\</Resource\>  
  <RequestId\>...\</RequestId\>  
</Error\>
```

* Required Error Codes:  
  * NoSuchBucket  
  * NoSuchKey  
  * AccessDenied  
  * SignatureDoesNotMatch

## 7. Compliance Verification

TBD
