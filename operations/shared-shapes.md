# Shared Schemas
Shared schema definitions for S3 operations.

## 📦 Core Resources

Core resource shapes representing the primary entities in object storage.

### Schema: CommonPrefix
Container for all (if there are any) keys between Prefix and the next occurrence of the string specified by a delimiter. CommonPrefixes lists keys that act like subdirectories in the directory specified by Prefix. For example, if the prefix is notes/ and the delimiter is a slash (/) as in notes/summer/july, the common prefix is notes/summer/.

#### CommonPrefix$Prefix
Container for the specified common prefix.

### Schema: DeletedObject
Information about the deleted object.

#### DeletedObject$Key
The name of the deleted object.

#### DeletedObject$VersionId
The version ID of the deleted object.

> [!NOTE]
> This functionality is not supported for directory buckets.

#### DeletedObject$DeleteMarker
Indicates whether the specified object version that was permanently deleted was (true) or was not (false) a delete marker before deletion. In a simple DELETE, this header indicates whether (true) or not (false) the current version of the object is a delete marker. To learn more about delete markers, see [Working with delete markers](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DeleteMarker.html).

> [!NOTE]
> This functionality is not supported for directory buckets.

#### DeletedObject$DeleteMarkerVersionId
The version ID of the delete marker created as a result of the DELETE operation. If you delete a specific object version, the value returned by this header is the version ID of the object version deleted.

> [!NOTE]
> This functionality is not supported for directory buckets.

### Schema: Object
An object consists of data and its descriptive metadata.

#### Object$Key
The name that you assign to an object. You use the object key to retrieve the object.

#### Object$LastModified
Creation date of the object.

#### Object$ETag
The entity tag is a hash of the object. The ETag reflects changes only to the contents of an object, not its metadata. The ETag may or may not be an MD5 digest of the object data. Whether or not it is depends on how the object was created and how it is encrypted as described below:

*   Objects created by the PUT Object, POST Object, or Copy operation, or through the Amazon Web Services Management Console, and are encrypted by SSE-S3 or plaintext, have ETags that are an MD5 digest of their object data.
*   Objects created by the PUT Object, POST Object, or Copy operation, or through the Amazon Web Services Management Console, and are encrypted by SSE-C or SSE-KMS, have ETags that are not an MD5 digest of their object data.
*   If an object is created by either the Multipart Upload or Part Copy operation, the ETag is not an MD5 digest, regardless of the method of encryption. If an object is larger than 16 MB, the Amazon Web Services Management Console will upload or copy that object as a Multipart Upload, and therefore the ETag will not be an MD5 digest.

> [!NOTE]
> **Directory buckets** - MD5 is not supported by directory buckets.

#### Object$ChecksumAlgorithm
The algorithm that was used to create a checksum of the object.

#### Object$ChecksumType
The checksum type that is used to calculate the object’s checksum value. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Object$Size
Size in bytes of the object

#### Object$StorageClass
The class of storage used to store the object.

> [!NOTE]
> **Directory buckets** - Directory buckets only support `EXPRESS_ONEZONE` (the S3 Express One Zone storage class) in Availability Zones and `ONEZONE_IA` (the S3 One Zone-Infrequent Access storage class) in Dedicated Local Zones.

#### Object$Owner
The owner of the object

> [!NOTE]
> **Directory buckets** - The bucket owner is returned as the object owner.

#### Object$RestoreStatus
Specifies the restoration status of an object. Objects in certain storage classes must be restored before they can be retrieved. For more information about these storage classes and how to work with archived objects, see [Working with archived objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/archived-objects.html) in the *Amazon S3 User Guide*.

> [!NOTE]
> This functionality is not supported for directory buckets. Directory buckets only support `EXPRESS_ONEZONE` (the S3 Express One Zone storage class) in Availability Zones and `ONEZONE_IA` (the S3 One Zone-Infrequent Access storage class) in Dedicated Local Zones.

### Schema: Owner
Container for the owner's display name and ID.

#### Owner$DisplayName


#### Owner$ID
Container for the ID of the owner.

## ⚙️ Configuration & Payloads

Structure shapes used for parameters, configuration options, and payload bodies.

### Schema: CompletedMultipartUpload
The container for the completed multipart upload details.

#### CompletedMultipartUpload$Parts
Array of CompletedPart data types.

If you do not supply a valid `Part` with your request, the service sends back an HTTP 400 response.

### Schema: CompletedPart
Details of the parts that were uploaded.

#### CompletedPart$ETag
Entity tag returned when the part was uploaded.

#### CompletedPart$ChecksumCRC32
The Base64 encoded, 32-bit `CRC32` checksum of the part. This checksum is present if the multipart upload request was created with the `CRC32` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$ChecksumCRC32C
The Base64 encoded, 32-bit `CRC32C` checksum of the part. This checksum is present if the multipart upload request was created with the `CRC32C` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$ChecksumCRC64NVME
The Base64 encoded, 64-bit `CRC64NVME` checksum of the part. This checksum is present if the multipart upload request was created with the `CRC64NVME` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$ChecksumSHA1
The Base64 encoded, 160-bit `SHA1` checksum of the part. This checksum is present if the multipart upload request was created with the `SHA1` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$ChecksumSHA256
The Base64 encoded, 256-bit `SHA256` checksum of the part. This checksum is present if the multipart upload request was created with the `SHA256` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$ChecksumSHA512
The Base64 encoded, 512-bit `SHA512` digest of the part. This checksum is present if the multipart upload request was created with the `SHA512` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$ChecksumMD5
The Base64 encoded, 128-bit `MD5` digest of the part. This checksum is present if the multipart upload request was created with the `MD5` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$ChecksumXXHASH64
The Base64 encoded, 64-bit `XXHASH64` checksum of the part. This checksum is present if the multipart upload request was created with the `XXHASH64` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$ChecksumXXHASH3
The Base64 encoded, 64-bit `XXHASH3` checksum of the part. This checksum is present if the multipart upload request was created with the `XXHASH3` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$ChecksumXXHASH128
The Base64 encoded, 128-bit `XXHASH128` checksum of the part. This checksum is present if the multipart upload request was created with the `XXHASH128` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CompletedPart$PartNumber
Part number that identifies the part. This is a positive integer between 1 and 10,000.

> [!NOTE]
> *   **General purpose buckets** - In `CompleteMultipartUpload`, when a additional checksum (including `x-amz-checksum-crc32`, `x-amz-checksum-crc32c`, `x-amz-checksum-sha1`, or `x-amz-checksum-sha256`) is applied to each part, the `PartNumber` must start at 1 and the part numbers must be consecutive. Otherwise, Amazon S3 generates an HTTP `400 Bad Request` status code and an `InvalidPartOrder` error code.
> *   **Directory buckets** - In `CompleteMultipartUpload`, the `PartNumber` must start at 1 and the part numbers must be consecutive.

### Schema: CopyObjectResult
Container for all response elements.

#### CopyObjectResult$ETag
Returns the ETag of the new object. The ETag reflects only changes to the contents of an object, not its metadata.

#### CopyObjectResult$LastModified
Creation date of the object.

#### CopyObjectResult$ChecksumType
The checksum type that is used to calculate the object’s checksum value. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumCRC32
The Base64 encoded, 32-bit `CRC32` checksum of the object. This checksum is only present if the object was uploaded with the object. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumCRC32C
The Base64 encoded, 32-bit `CRC32C` checksum of the object. This checksum is only present if the checksum was uploaded with the object. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumCRC64NVME
The Base64 encoded, 64-bit `CRC64NVME` checksum of the object. This checksum is present if the object being copied was uploaded with the `CRC64NVME` checksum algorithm, or if the object was uploaded without a checksum (and Amazon S3 added the default checksum, `CRC64NVME`, to the uploaded object). For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumSHA1
The Base64 encoded, 160-bit `SHA1` digest of the object. This checksum is only present if the checksum was uploaded with the object. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumSHA256
The Base64 encoded, 256-bit `SHA256` digest of the object. This checksum is only present if the checksum was uploaded with the object. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumSHA512
The Base64 encoded, 512-bit `SHA512` digest of the object. This checksum is only present if the object was uploaded with the `SHA512` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumMD5
The Base64 encoded, 128-bit `MD5` digest of the object. This checksum is only present if the object was uploaded with the `MD5` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumXXHASH64
The Base64 encoded, 64-bit `XXHASH64` checksum of the object. This checksum is only present if the object was uploaded with the `XXHASH64` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumXXHASH3
The Base64 encoded, 64-bit `XXHASH3` checksum of the object. This checksum is only present if the object was uploaded with the `XXHASH3` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyObjectResult$ChecksumXXHASH128
The Base64 encoded, 128-bit `XXHASH128` checksum of the object. This checksum is only present if the object was uploaded with the `XXHASH128` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

### Schema: CopyPartResult
Container for all response elements.

#### CopyPartResult$ETag
Entity tag of the object.

#### CopyPartResult$LastModified
Date and time at which the object was uploaded.

#### CopyPartResult$ChecksumCRC32
The Base64 encoded, 32-bit `CRC32` checksum of the part. This checksum is present if the multipart upload request was created with the `CRC32` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyPartResult$ChecksumCRC32C
The Base64 encoded, 32-bit `CRC32C` checksum of the part. This checksum is present if the multipart upload request was created with the `CRC32C` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyPartResult$ChecksumCRC64NVME
The Base64 encoded, 64-bit `CRC64NVME` checksum of the part. This checksum is present if the multipart upload request was created with the `CRC64NVME` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyPartResult$ChecksumSHA1
The Base64 encoded, 160-bit `SHA1` digest of the part. This checksum is present if the multipart upload request was created with the `SHA1` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyPartResult$ChecksumSHA256
The Base64 encoded, 256-bit `SHA256` digest of the part. This checksum is present if the multipart upload request was created with the `SHA256` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyPartResult$ChecksumSHA512
The Base64 encoded, 512-bit `SHA512` digest of the part. This checksum is present if the multipart upload request was created with the `SHA512` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyPartResult$ChecksumMD5
The Base64 encoded, 128-bit `MD5` digest of the part. This checksum is present if the multipart upload request was created with the `MD5` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyPartResult$ChecksumXXHASH64
The Base64 encoded, 64-bit `XXHASH64` checksum of the part. This checksum is present if the multipart upload request was created with the `XXHASH64` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyPartResult$ChecksumXXHASH3
The Base64 encoded, 64-bit `XXHASH3` checksum of the part. This checksum is present if the multipart upload request was created with the `XXHASH3` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### CopyPartResult$ChecksumXXHASH128
The Base64 encoded, 128-bit `XXHASH128` checksum of the part. This checksum is present if the multipart upload request was created with the `XXHASH128` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

### Schema: Delete
Container for the objects to delete.

#### Delete$Objects
The object to delete.

> [!NOTE]
> **Directory buckets** - For directory buckets, an object that's composed entirely of whitespace characters is not supported by the `DeleteObjects` API operation. The request will receive a `400 Bad Request` error and none of the objects in the request will be deleted.

#### Delete$Quiet
Element to enable quiet mode for the request. When you add this element, you must set its value to `true`.

### Schema: Initiator
Container element that identifies who initiated the multipart upload.

#### Initiator$ID
If the principal is an Amazon Web Services account, it provides the Canonical User ID. If the principal is an IAM User, it provides a user ARN value.

> [!NOTE]
> **Directory buckets** - If the principal is an Amazon Web Services account, it provides the Amazon Web Services account ID. If the principal is an IAM User, it provides a user ARN value.

#### Initiator$DisplayName
> [!NOTE]
> This functionality is not supported for directory buckets.

### Schema: MultipartUpload
Container for the `MultipartUpload` for the Amazon S3 object.

#### MultipartUpload$UploadId
Upload ID that identifies the multipart upload.

#### MultipartUpload$Key
Key of the object for which the multipart upload was initiated.

#### MultipartUpload$Initiated
Date and time at which the multipart upload was initiated.

#### MultipartUpload$StorageClass
The class of storage used to store the object.

> [!NOTE]
> **Directory buckets** - Directory buckets only support `EXPRESS_ONEZONE` (the S3 Express One Zone storage class) in Availability Zones and `ONEZONE_IA` (the S3 One Zone-Infrequent Access storage class) in Dedicated Local Zones.

#### MultipartUpload$Owner
Specifies the owner of the object that is part of the multipart upload.

> [!NOTE]
> **Directory buckets** - The bucket owner is returned as the object owner for all the objects.

#### MultipartUpload$Initiator
Identifies who initiated the multipart upload.

#### MultipartUpload$ChecksumAlgorithm
The algorithm that was used to create a checksum of the object.

#### MultipartUpload$ChecksumType
The checksum type that is used to calculate the object’s checksum value. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

### Schema: ObjectIdentifier
Object Identifier is unique value to identify objects.

#### ObjectIdentifier$Key
Key name of the object.

> [!IMPORTANT]
> Replacement must be made for object keys containing special characters (such as carriage returns) when using XML requests. For more information, see [XML related object key constraints](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html#object-key-xml-related-constraints).

#### ObjectIdentifier$VersionId
Version ID for the specific version of the object to delete.

> [!NOTE]
> This functionality is not supported for directory buckets.

#### ObjectIdentifier$ETag
An entity tag (ETag) is an identifier assigned by a web server to a specific version of a resource found at a URL. This header field makes the request method conditional on `ETags`.

> [!NOTE]
> Entity tags (ETags) for S3 Express One Zone are random alphanumeric strings unique to the object.

#### ObjectIdentifier$LastModifiedTime
If present, the objects are deleted only if its modification times matches the provided `Timestamp`.

> [!NOTE]
> This functionality is only supported for directory buckets.

#### ObjectIdentifier$Size
If present, the objects are deleted only if its size matches the provided size in bytes.

> [!NOTE]
> This functionality is only supported for directory buckets.

### Schema: Part
Container for elements related to a part.

#### Part$PartNumber
Part number identifying the part. This is a positive integer between 1 and 10,000.

#### Part$LastModified
Date and time at which the part was uploaded.

#### Part$ETag
Entity tag returned when the part was uploaded.

#### Part$Size
Size in bytes of the uploaded part data.

#### Part$ChecksumCRC32
The Base64 encoded, 32-bit `CRC32` checksum of the part. This checksum is present if the object was uploaded with the `CRC32` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Part$ChecksumCRC32C
The Base64 encoded, 32-bit `CRC32C` checksum of the part. This checksum is present if the object was uploaded with the `CRC32C` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Part$ChecksumCRC64NVME
The Base64 encoded, 64-bit `CRC64NVME` checksum of the part. This checksum is present if the multipart upload request was created with the `CRC64NVME` checksum algorithm, or if the object was uploaded without a checksum (and Amazon S3 added the default checksum, `CRC64NVME`, to the uploaded object). For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Part$ChecksumSHA1
The Base64 encoded, 160-bit `SHA1` checksum of the part. This checksum is present if the object was uploaded with the `SHA1` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Part$ChecksumSHA256
The Base64 encoded, 256-bit `SHA256` checksum of the part. This checksum is present if the object was uploaded with the `SHA256` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Part$ChecksumSHA512
The Base64 encoded, 512-bit `SHA512` digest of the part. This checksum is present if the multipart upload request was created with the `SHA512` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Part$ChecksumMD5
The Base64 encoded, 128-bit `MD5` digest of the part. This checksum is present if the multipart upload request was created with the `MD5` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Part$ChecksumXXHASH64
The Base64 encoded, 64-bit `XXHASH64` checksum of the part. This checksum is present if the multipart upload request was created with the `XXHASH64` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Part$ChecksumXXHASH3
The Base64 encoded, 64-bit `XXHASH3` checksum of the part. This checksum is present if the multipart upload request was created with the `XXHASH3` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

#### Part$ChecksumXXHASH128
The Base64 encoded, 128-bit `XXHASH128` checksum of the part. This checksum is present if the multipart upload request was created with the `XXHASH128` checksum algorithm. For more information, see [Checking object integrity](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html) in the *Amazon S3 User Guide*.

### Schema: RestoreStatus
Specifies the restoration status of an object. Objects in certain storage classes must be restored before they can be retrieved. For more information about these storage classes and how to work with archived objects, see [Working with archived objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/archived-objects.html) in the *Amazon S3 User Guide*.

> [!NOTE]
> This functionality is not supported for directory buckets. Directory buckets only support `EXPRESS_ONEZONE` (the S3 Express One Zone storage class) in Availability Zones and `ONEZONE_IA` (the S3 One Zone-Infrequent Access storage class) in Dedicated Local Zones.

#### RestoreStatus$IsRestoreInProgress
Specifies whether the object is currently being restored. If the object restoration is in progress, the header returns the value `TRUE`. For example:

`x-amz-optional-object-attributes: IsRestoreInProgress="true"`

If the object restoration has completed, the header returns the value `FALSE`. For example:

`x-amz-optional-object-attributes: IsRestoreInProgress="false", RestoreExpiryDate="2012-12-21T00:00:00.000Z"`

If the object hasn't been restored, there is no header response.

#### RestoreStatus$RestoreExpiryDate
Indicates when the restored copy will expire. This value is populated only if the object has already been restored. For example:

`x-amz-optional-object-attributes: IsRestoreInProgress="false", RestoreExpiryDate="2012-12-21T00:00:00.000Z"`

## 🏷️ Enums & Constants

Enumerations, options, and fixed string constants.

### Schema: AnnotationDirective


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `COPY` | `COPY` |  |
| `EXCLUDE` | `EXCLUDE` |  |

### Schema: ArchiveStatus


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `ARCHIVE_ACCESS` | `ARCHIVE_ACCESS` |  |
| `DEEP_ARCHIVE_ACCESS` | `DEEP_ARCHIVE_ACCESS` |  |

### Schema: ChecksumAlgorithm


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `CRC32` | `CRC32` |  |
| `CRC32C` | `CRC32C` |  |
| `SHA1` | `SHA1` |  |
| `SHA256` | `SHA256` |  |
| `CRC64NVME` | `CRC64NVME` |  |
| `SHA512` | `SHA512` |  |
| `MD5` | `MD5` |  |
| `XXHASH64` | `XXHASH64` |  |
| `XXHASH3` | `XXHASH3` |  |
| `XXHASH128` | `XXHASH128` |  |

### Schema: ChecksumMode


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `ENABLED` | `ENABLED` |  |

### Schema: ChecksumType


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `COMPOSITE` | `COMPOSITE` |  |
| `FULL_OBJECT` | `FULL_OBJECT` |  |

### Schema: EncodingType
Encoding type used by Amazon S3 to encode the [object keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html) in the response. Responses are encoded only in UTF-8. An object key can contain any Unicode character. However, the XML 1.0 parser can't parse certain characters, such as characters with an ASCII value from 0 to 10. For characters that aren't supported in XML 1.0, you can add this parameter to request that Amazon S3 encode the keys in the response. For more information about characters to avoid in object key names, see [Object key naming guidelines](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html#object-key-guidelines).

> [!NOTE]
> When using the URL encoding type, non-ASCII characters that are used in an object's key name will be percent-encoded according to UTF-8 code values. For example, the object `test_file(3).png` will appear as `test_file%283%29.png`.

#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `url` | `url` |  |

### Schema: IntelligentTieringAccessTier


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `ARCHIVE_ACCESS` | `ARCHIVE_ACCESS` |  |
| `DEEP_ARCHIVE_ACCESS` | `DEEP_ARCHIVE_ACCESS` |  |

### Schema: LocationType


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `AvailabilityZone` | `AvailabilityZone` |  |
| `LocalZone` | `LocalZone` |  |

### Schema: MetadataDirective


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `COPY` | `COPY` |  |
| `REPLACE` | `REPLACE` |  |

### Schema: ObjectCannedACL


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `private` | `private` |  |
| `public_read` | `public-read` |  |
| `public_read_write` | `public-read-write` |  |
| `authenticated_read` | `authenticated-read` |  |
| `aws_exec_read` | `aws-exec-read` |  |
| `bucket_owner_read` | `bucket-owner-read` |  |
| `bucket_owner_full_control` | `bucket-owner-full-control` |  |

### Schema: ObjectLockLegalHoldStatus


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `ON` | `ON` |  |
| `OFF` | `OFF` |  |

### Schema: ObjectLockMode


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `GOVERNANCE` | `GOVERNANCE` |  |
| `COMPLIANCE` | `COMPLIANCE` |  |

### Schema: ObjectStorageClass


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `STANDARD` | `STANDARD` |  |
| `REDUCED_REDUNDANCY` | `REDUCED_REDUNDANCY` |  |
| `GLACIER` | `GLACIER` |  |
| `STANDARD_IA` | `STANDARD_IA` |  |
| `ONEZONE_IA` | `ONEZONE_IA` |  |
| `INTELLIGENT_TIERING` | `INTELLIGENT_TIERING` |  |
| `DEEP_ARCHIVE` | `DEEP_ARCHIVE` |  |
| `OUTPOSTS` | `OUTPOSTS` |  |
| `GLACIER_IR` | `GLACIER_IR` |  |
| `SNOW` | `SNOW` |  |
| `EXPRESS_ONEZONE` | `EXPRESS_ONEZONE` |  |
| `FSX_OPENZFS` | `FSX_OPENZFS` |  |
| `FSX_ONTAP` | `FSX_ONTAP` |  |

### Schema: OptionalObjectAttributes


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `RESTORE_STATUS` | `RestoreStatus` |  |

### Schema: ReplicationStatus


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `COMPLETE` | `COMPLETE` |  |
| `PENDING` | `PENDING` |  |
| `FAILED` | `FAILED` |  |
| `REPLICA` | `REPLICA` |  |
| `COMPLETED` | `COMPLETED` |  |

### Schema: RequestCharged
If present, indicates that the requester was successfully charged for the request. For more information, see [Using Requester Pays buckets for storage transfers and usage](https://docs.aws.amazon.com/AmazonS3/latest/userguide/RequesterPaysBuckets.html) in the *Amazon Simple Storage Service user guide*.

> [!NOTE]
> This functionality is not supported for directory buckets.

#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `requester` | `requester` |  |

### Schema: RequestPayer
Confirms that the requester knows that they will be charged for the request. Bucket owners need not specify this parameter in their requests. If either the source or destination S3 bucket has Requester Pays enabled, the requester will pay for the corresponding charges. For information about downloading objects from Requester Pays buckets, see [Downloading Objects in Requester Pays Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/ObjectsinRequesterPaysBuckets.html) in the *Amazon S3 User Guide*.

> [!NOTE]
> This functionality is not supported for directory buckets.

#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `requester` | `requester` |  |

### Schema: ServerSideEncryption


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `AES256` | `AES256` |  |
| `aws_fsx` | `aws:fsx` |  |
| `aws_kms` | `aws:kms` |  |
| `aws_kms_dsse` | `aws:kms:dsse` |  |

### Schema: StorageClass


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `STANDARD` | `STANDARD` |  |
| `REDUCED_REDUNDANCY` | `REDUCED_REDUNDANCY` |  |
| `STANDARD_IA` | `STANDARD_IA` |  |
| `ONEZONE_IA` | `ONEZONE_IA` |  |
| `INTELLIGENT_TIERING` | `INTELLIGENT_TIERING` |  |
| `GLACIER` | `GLACIER` |  |
| `DEEP_ARCHIVE` | `DEEP_ARCHIVE` |  |
| `OUTPOSTS` | `OUTPOSTS` |  |
| `GLACIER_IR` | `GLACIER_IR` |  |
| `SNOW` | `SNOW` |  |
| `EXPRESS_ONEZONE` | `EXPRESS_ONEZONE` |  |
| `FSX_OPENZFS` | `FSX_OPENZFS` |  |
| `FSX_ONTAP` | `FSX_ONTAP` |  |

### Schema: TaggingDirective


#### Values

| Member | Value | Description |
| :--- | :--- | :--- |
| `COPY` | `COPY` |  |
| `REPLACE` | `REPLACE` |  |

## 🚨 Errors

Common error response schemas returned when operations fail.

### Schema: `400` `EncryptionTypeMismatch`
The existing object was created with a different encryption type. Subsequent write requests must include the appropriate encryption parameters in the request or while creating the session.

### Schema: `Error`
Container for all error elements.

#### Error$Key
The error key.

#### Error$VersionId
The version ID of the error.

> [!NOTE]
> This functionality is not supported for directory buckets.

#### Error$Code
The error code is a string that uniquely identifies an error condition. It is meant to be read and understood by programs that detect and handle errors by type. The following is a list of Amazon S3 error codes. For more information, see [Error responses](https://docs.aws.amazon.com/AmazonS3/latest/API/ErrorResponses.html).

*   *   *Code:* AccessDenied
    *   *Description:* Access Denied
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* AccountProblem
    *   *Description:* There is a problem with your Amazon Web Services account that prevents the action from completing successfully. Contact Amazon Web Services Support for further assistance.
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* AllAccessDisabled
    *   *Description:* All access to this Amazon S3 resource has been disabled. Contact Amazon Web Services Support for further assistance.
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* AmbiguousGrantByEmailAddress
    *   *Description:* The email address you provided is associated with more than one account.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* AuthorizationHeaderMalformed
    *   *Description:* The authorization header you provided is invalid.
    *   *HTTP Status Code:* 400 Bad Request
    *   *HTTP Status Code:* N/A
*   *   *Code:* BadDigest
    *   *Description:* The Content-MD5 you specified did not match what we received.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* BucketAlreadyExists
    *   *Description:* The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.
    *   *HTTP Status Code:* 409 Conflict
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* BucketAlreadyOwnedByYou
    *   *Description:* The bucket you tried to create already exists, and you own it. Amazon S3 returns this error in all Amazon Web Services Regions except in the North Virginia Region. For legacy compatibility, if you re-create an existing bucket that you already own in the North Virginia Region, Amazon S3 returns 200 OK and resets the bucket access control lists (ACLs).
    *   *Code:* 409 Conflict (in all Regions except the North Virginia Region)
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* BucketNotEmpty
    *   *Description:* The bucket you tried to delete is not empty.
    *   *HTTP Status Code:* 409 Conflict
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* CredentialsNotSupported
    *   *Description:* This request does not support credentials.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* CrossLocationLoggingProhibited
    *   *Description:* Cross-location logging not allowed. Buckets in one geographic location cannot log information to a bucket in another location.
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* EntityTooSmall
    *   *Description:* Your proposed upload is smaller than the minimum allowed object size.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* EntityTooLarge
    *   *Description:* Your proposed upload exceeds the maximum allowed object size.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* ExpiredToken
    *   *Description:* The provided token has expired.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* IllegalVersioningConfigurationException
    *   *Description:* Indicates that the versioning configuration specified in the request is invalid.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* IncompleteBody
    *   *Description:* You did not provide the number of bytes specified by the Content-Length HTTP header
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* IncorrectNumberOfFilesInPostRequest
    *   *Description:* POST requires exactly one file upload per request.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InlineDataTooLarge
    *   *Description:* Inline data exceeds the maximum allowed size.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InternalError
    *   *Description:* We encountered an internal error. Please try again.
    *   *HTTP Status Code:* 500 Internal Server Error
    *   *SOAP Fault Code Prefix:* Server
*   *   *Code:* InvalidAccessKeyId
    *   *Description:* The Amazon Web Services access key ID you provided does not exist in our records.
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidAddressingHeader
    *   *Description:* You must specify the Anonymous role.
    *   *HTTP Status Code:* N/A
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidArgument
    *   *Description:* Invalid Argument
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidBucketName
    *   *Description:* The specified bucket is not valid.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidBucketState
    *   *Description:* The request is not valid with the current state of the bucket.
    *   *HTTP Status Code:* 409 Conflict
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidDigest
    *   *Description:* The Content-MD5 you specified is not valid.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidEncryptionAlgorithmError
    *   *Description:* The encryption request you specified is not valid. The valid value is AES256.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidLocationConstraint
    *   *Description:* The specified location constraint is not valid. For more information about Regions, see [How to Select a Region for Your Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html#access-bucket-intro).
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidObjectState
    *   *Description:* The action is not valid for the current state of the object.
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidPart
    *   *Description:* One or more of the specified parts could not be found. The part might not have been uploaded, or the specified entity tag might not have matched the part's entity tag.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidPartOrder
    *   *Description:* The list of parts was not in ascending order. Parts list must be specified in order by part number.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidPayer
    *   *Description:* All access to this object has been disabled. Please contact Amazon Web Services Support for further assistance.
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidPolicyDocument
    *   *Description:* The content of the form does not meet the conditions specified in the policy document.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidRange
    *   *Description:* The requested range cannot be satisfied.
    *   *HTTP Status Code:* 416 Requested Range Not Satisfiable
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidRequest
    *   *Description:* Please use `AWS4-HMAC-SHA256`.
    *   *HTTP Status Code:* 400 Bad Request
    *   *Code:* N/A
*   *   *Code:* InvalidRequest
    *   *Description:* SOAP requests must be made over an HTTPS connection.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidRequest
    *   *Description:* Amazon S3 Transfer Acceleration is not supported for buckets with non-DNS compliant names.
    *   *HTTP Status Code:* 400 Bad Request
    *   *Code:* N/A
*   *   *Code:* InvalidRequest
    *   *Description:* Amazon S3 Transfer Acceleration is not supported for buckets with periods (.) in their names.
    *   *HTTP Status Code:* 400 Bad Request
    *   *Code:* N/A
*   *   *Code:* InvalidRequest
    *   *Description:* Amazon S3 Transfer Accelerate endpoint only supports virtual style requests.
    *   *HTTP Status Code:* 400 Bad Request
    *   *Code:* N/A
*   *   *Code:* InvalidRequest
    *   *Description:* Amazon S3 Transfer Accelerate is not configured on this bucket.
    *   *HTTP Status Code:* 400 Bad Request
    *   *Code:* N/A
*   *   *Code:* InvalidRequest
    *   *Description:* Amazon S3 Transfer Accelerate is disabled on this bucket.
    *   *HTTP Status Code:* 400 Bad Request
    *   *Code:* N/A
*   *   *Code:* InvalidRequest
    *   *Description:* Amazon S3 Transfer Acceleration is not supported on this bucket. Contact Amazon Web Services Support for more information.
    *   *HTTP Status Code:* 400 Bad Request
    *   *Code:* N/A
*   *   *Code:* InvalidRequest
    *   *Description:* Amazon S3 Transfer Acceleration cannot be enabled on this bucket. Contact Amazon Web Services Support for more information.
    *   *HTTP Status Code:* 400 Bad Request
    *   *Code:* N/A
*   *   *Code:* InvalidSecurity
    *   *Description:* The provided security credentials are not valid.
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidSOAPRequest
    *   *Description:* The SOAP request body is invalid.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidStorageClass
    *   *Description:* The storage class you specified is not valid.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidTargetBucketForLogging
    *   *Description:* The target bucket for logging does not exist, is not owned by you, or does not have the appropriate grants for the log-delivery group.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidToken
    *   *Description:* The provided token is malformed or otherwise invalid.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* InvalidURI
    *   *Description:* Couldn't parse the specified URI.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* KeyTooLongError
    *   *Description:* Your key is too long.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MalformedACLError
    *   *Description:* The XML you provided was not well-formed or did not validate against our published schema.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MalformedPOSTRequest
    *   *Description:* The body of your POST request is not well-formed multipart/form-data.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MalformedXML
    *   *Description:* This happens when the user sends malformed XML (XML that doesn't conform to the published XSD) for the configuration. The error message is, "The XML you provided was not well-formed or did not validate against our published schema."
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MaxMessageLengthExceeded
    *   *Description:* Your request was too big.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MaxPostPreDataLengthExceededError
    *   *Description:* Your POST request fields preceding the upload file were too large.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MetadataTooLarge
    *   *Description:* Your metadata headers exceed the maximum allowed metadata size.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MethodNotAllowed
    *   *Description:* The specified method is not allowed against this resource.
    *   *HTTP Status Code:* 405 Method Not Allowed
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MissingAttachment
    *   *Description:* A SOAP attachment was expected, but none were found.
    *   *HTTP Status Code:* N/A
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MissingContentLength
    *   *Description:* You must provide the Content-Length HTTP header.
    *   *HTTP Status Code:* 411 Length Required
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MissingRequestBodyError
    *   *Description:* This happens when the user sends an empty XML document as a request. The error message is, "Request body is empty."
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MissingSecurityElement
    *   *Description:* The SOAP 1.1 request is missing a security element.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* MissingSecurityHeader
    *   *Description:* Your request is missing a required header.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* NoLoggingStatusForKey
    *   *Description:* There is no such thing as a logging status subresource for a key.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* NoSuchBucket
    *   *Description:* The specified bucket does not exist.
    *   *HTTP Status Code:* 404 Not Found
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* NoSuchBucketPolicy
    *   *Description:* The specified bucket does not have a bucket policy.
    *   *HTTP Status Code:* 404 Not Found
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* NoSuchKey
    *   *Description:* The specified key does not exist.
    *   *HTTP Status Code:* 404 Not Found
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* NoSuchLifecycleConfiguration
    *   *Description:* The lifecycle configuration does not exist.
    *   *HTTP Status Code:* 404 Not Found
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* NoSuchUpload
    *   *Description:* The specified multipart upload does not exist. The upload ID might be invalid, or the multipart upload might have been aborted or completed.
    *   *HTTP Status Code:* 404 Not Found
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* NoSuchVersion
    *   *Description:* Indicates that the version ID specified in the request does not match an existing version.
    *   *HTTP Status Code:* 404 Not Found
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* NotImplemented
    *   *Description:* A header you provided implies functionality that is not implemented.
    *   *HTTP Status Code:* 501 Not Implemented
    *   *SOAP Fault Code Prefix:* Server
*   *   *Code:* NotSignedUp
    *   *Description:* Your account is not signed up for the Amazon S3 service. You must sign up before you can use Amazon S3. You can sign up at the following URL: [Amazon S3](http://aws.amazon.com/s3)
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* OperationAborted
    *   *Description:* A conflicting conditional action is currently in progress against this resource. Try again.
    *   *HTTP Status Code:* 409 Conflict
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* PermanentRedirect
    *   *Description:* The bucket you are attempting to access must be addressed using the specified endpoint. Send all future requests to this endpoint.
    *   *HTTP Status Code:* 301 Moved Permanently
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* PreconditionFailed
    *   *Description:* At least one of the preconditions you specified did not hold.
    *   *HTTP Status Code:* 412 Precondition Failed
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* Redirect
    *   *Description:* Temporary redirect.
    *   *HTTP Status Code:* 307 Moved Temporarily
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* RestoreAlreadyInProgress
    *   *Description:* Object restore is already in progress.
    *   *HTTP Status Code:* 409 Conflict
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* RequestIsNotMultiPartContent
    *   *Description:* Bucket POST must be of the enclosure-type multipart/form-data.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* RequestTimeout
    *   *Description:* Your socket connection to the server was not read from or written to within the timeout period.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* RequestTimeTooSkewed
    *   *Description:* The difference between the request time and the server's time is too large.
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* RequestTorrentOfBucketError
    *   *Description:* Requesting the torrent file of a bucket is not permitted.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* SignatureDoesNotMatch
    *   *Description:* The request signature we calculated does not match the signature you provided. Check your Amazon Web Services secret access key and signing method. For more information, see [REST Authentication](https://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html) and [SOAP Authentication](https://docs.aws.amazon.com/AmazonS3/latest/dev/SOAPAuthentication.html) for details.
    *   *HTTP Status Code:* 403 Forbidden
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* ServiceUnavailable
    *   *Description:* Service is unable to handle request.
    *   *HTTP Status Code:* 503 Service Unavailable
    *   *SOAP Fault Code Prefix:* Server
*   *   *Code:* SlowDown
    *   *Description:* Reduce your request rate.
    *   *HTTP Status Code:* 503 Slow Down
    *   *SOAP Fault Code Prefix:* Server
*   *   *Code:* TemporaryRedirect
    *   *Description:* You are being redirected to the bucket while DNS updates.
    *   *HTTP Status Code:* 307 Moved Temporarily
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* TokenRefreshRequired
    *   *Description:* The provided token must be refreshed.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* TooManyBuckets
    *   *Description:* You have attempted to create more buckets than allowed.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* UnexpectedContent
    *   *Description:* This request does not support content.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* UnresolvableGrantByEmailAddress
    *   *Description:* The email address you provided does not match any account on record.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client
*   *   *Code:* UserKeyMustBeSpecified
    *   *Description:* The bucket POST must contain the specified field name. If it is specified, check the order of the fields.
    *   *HTTP Status Code:* 400 Bad Request
    *   *SOAP Fault Code Prefix:* Client

#### Error$Message
The error message contains a generic description of the error condition in English. It is intended for a human audience. Simple programs display the message directly to the end user if they encounter an error condition they don't know how or don't care to handle. Sophisticated programs with more exhaustive error handling and proper internationalization are more likely to ignore the error message.

### Schema: `403` `InvalidObjectState`
Object is archived and inaccessible until restored.

If the object you are retrieving is stored in the S3 Glacier Flexible Retrieval storage class, the S3 Glacier Deep Archive storage class, the S3 Intelligent-Tiering Archive Access tier, or the S3 Intelligent-Tiering Deep Archive Access tier, before you can retrieve the object you must first restore a copy using [RestoreObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_RestoreObject.html). Otherwise, this operation returns an `InvalidObjectState` error. For information about restoring archived objects, see [Restoring Archived Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/restoring-objects.html) in the *Amazon S3 User Guide*.

#### InvalidObjectState$StorageClass


#### InvalidObjectState$AccessTier


### Schema: `400` `InvalidRequest`
A parameter or header in your request isn't valid. For details, see the description of this API operation.

### Schema: `400` `InvalidWriteOffset`
The write offset value that you specified does not match the current object size.

### Schema: `404` `NoSuchBucket`
The specified bucket does not exist.

### Schema: `404` `NoSuchKey`
The specified key does not exist.

### Schema: `404` `NoSuchUpload`
The specified multipart upload does not exist.

### Schema: `404` `NotFound`
The specified content does not exist.

### Schema: `403` `ObjectNotInActiveTierError`
The source object of the COPY action is not in the active tier and is only stored in Amazon S3 Glacier.

### Schema: `400` `TooManyParts`
You have attempted to add more parts than the maximum of 10000 that are allowed for this object. You can use the CopyObject operation to copy this object to another and then add more data to the newly copied object.

## Schema Details

<details>
<summary>Enum Value Details</summary>

#### AnnotationDirective$COPY


#### AnnotationDirective$EXCLUDE


#### ArchiveStatus$ARCHIVE_ACCESS


#### ArchiveStatus$DEEP_ARCHIVE_ACCESS


#### ChecksumAlgorithm$CRC32


#### ChecksumAlgorithm$CRC32C


#### ChecksumAlgorithm$SHA1


#### ChecksumAlgorithm$SHA256


#### ChecksumAlgorithm$CRC64NVME


#### ChecksumAlgorithm$SHA512


#### ChecksumAlgorithm$MD5


#### ChecksumAlgorithm$XXHASH64


#### ChecksumAlgorithm$XXHASH3


#### ChecksumAlgorithm$XXHASH128


#### ChecksumMode$ENABLED


#### ChecksumType$COMPOSITE


#### ChecksumType$FULL_OBJECT


#### EncodingType$url


#### IntelligentTieringAccessTier$ARCHIVE_ACCESS


#### IntelligentTieringAccessTier$DEEP_ARCHIVE_ACCESS


#### LocationType$AvailabilityZone


#### LocationType$LocalZone


#### MetadataDirective$COPY


#### MetadataDirective$REPLACE


#### ObjectCannedACL$private


#### ObjectCannedACL$public_read


#### ObjectCannedACL$public_read_write


#### ObjectCannedACL$authenticated_read


#### ObjectCannedACL$aws_exec_read


#### ObjectCannedACL$bucket_owner_read


#### ObjectCannedACL$bucket_owner_full_control


#### ObjectLockLegalHoldStatus$ON


#### ObjectLockLegalHoldStatus$OFF


#### ObjectLockMode$GOVERNANCE


#### ObjectLockMode$COMPLIANCE


#### ObjectStorageClass$STANDARD


#### ObjectStorageClass$REDUCED_REDUNDANCY


#### ObjectStorageClass$GLACIER


#### ObjectStorageClass$STANDARD_IA


#### ObjectStorageClass$ONEZONE_IA


#### ObjectStorageClass$INTELLIGENT_TIERING


#### ObjectStorageClass$DEEP_ARCHIVE


#### ObjectStorageClass$OUTPOSTS


#### ObjectStorageClass$GLACIER_IR


#### ObjectStorageClass$SNOW


#### ObjectStorageClass$EXPRESS_ONEZONE


#### ObjectStorageClass$FSX_OPENZFS


#### ObjectStorageClass$FSX_ONTAP


#### OptionalObjectAttributes$RESTORE_STATUS


#### ReplicationStatus$COMPLETE


#### ReplicationStatus$PENDING


#### ReplicationStatus$FAILED


#### ReplicationStatus$REPLICA


#### ReplicationStatus$COMPLETED


#### RequestCharged$requester


#### RequestPayer$requester


#### ServerSideEncryption$AES256


#### ServerSideEncryption$aws_fsx


#### ServerSideEncryption$aws_kms


#### ServerSideEncryption$aws_kms_dsse


#### StorageClass$STANDARD


#### StorageClass$REDUCED_REDUNDANCY


#### StorageClass$STANDARD_IA


#### StorageClass$ONEZONE_IA


#### StorageClass$INTELLIGENT_TIERING


#### StorageClass$GLACIER


#### StorageClass$DEEP_ARCHIVE


#### StorageClass$OUTPOSTS


#### StorageClass$GLACIER_IR


#### StorageClass$SNOW


#### StorageClass$EXPRESS_ONEZONE


#### StorageClass$FSX_OPENZFS


#### StorageClass$FSX_ONTAP


#### TaggingDirective$COPY


#### TaggingDirective$REPLACE


#### End of Details
</details>

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#AmazonS3": {
      "type": "service",
      "version": "2006-03-01",
      "operations": [
        {
          "target": "com.amazonaws.s3#AbortMultipartUpload"
        },
        {
          "target": "com.amazonaws.s3#CompleteMultipartUpload"
        },
        {
          "target": "com.amazonaws.s3#CopyObject"
        },
        {
          "target": "com.amazonaws.s3#CreateMultipartUpload"
        },
        {
          "target": "com.amazonaws.s3#DeleteObject"
        },
        {
          "target": "com.amazonaws.s3#DeleteObjects"
        },
        {
          "target": "com.amazonaws.s3#GetObject"
        },
        {
          "target": "com.amazonaws.s3#HeadBucket"
        },
        {
          "target": "com.amazonaws.s3#HeadObject"
        },
        {
          "target": "com.amazonaws.s3#ListMultipartUploads"
        },
        {
          "target": "com.amazonaws.s3#ListObjectsV2"
        },
        {
          "target": "com.amazonaws.s3#ListParts"
        },
        {
          "target": "com.amazonaws.s3#PutObject"
        },
        {
          "target": "com.amazonaws.s3#UploadPart"
        },
        {
          "target": "com.amazonaws.s3#UploadPartCopy"
        }
      ],
      "traits": {
        "aws.auth#sigv4": {
          "name": "s3"
        },
        "aws.protocols#restXml": {
          "noErrorWrapping": true
        },
        "smithy.api#documentation": {
          "$ref": "#shared-schemas"
        },
        "smithy.api#suppress": [
          "RuleSetAuthSchemes"
        ],
        "smithy.api#title": "Amazon Simple Storage Service",
        "smithy.api#xmlNamespace": {
          "uri": "http://s3.amazonaws.com/doc/2006-03-01/"
        }
      }
    },
    "com.amazonaws.s3#AbortDate": {
      "type": "timestamp"
    },
    "com.amazonaws.s3#AbortRuleId": {
      "type": "string"
    },
    "com.amazonaws.s3#AcceptRanges": {
      "type": "string"
    },
    "com.amazonaws.s3#AccessPointAlias": {
      "type": "boolean"
    },
    "com.amazonaws.s3#AccountId": {
      "type": "string"
    },
    "com.amazonaws.s3#AnnotationDirective": {
      "type": "enum",
      "members": {
        "COPY": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "COPY"
          }
        },
        "EXCLUDE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "EXCLUDE"
          }
        }
      }
    },
    "com.amazonaws.s3#ArchiveStatus": {
      "type": "enum",
      "members": {
        "ARCHIVE_ACCESS": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "ARCHIVE_ACCESS"
          }
        },
        "DEEP_ARCHIVE_ACCESS": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "DEEP_ARCHIVE_ACCESS"
          }
        }
      }
    },
    "com.amazonaws.s3#BucketKeyEnabled": {
      "type": "boolean"
    },
    "com.amazonaws.s3#BucketLocationName": {
      "type": "string"
    },
    "com.amazonaws.s3#BucketName": {
      "type": "string"
    },
    "com.amazonaws.s3#BypassGovernanceRetention": {
      "type": "boolean"
    },
    "com.amazonaws.s3#CacheControl": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumAlgorithm": {
      "type": "enum",
      "members": {
        "CRC32": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "CRC32"
          }
        },
        "CRC32C": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "CRC32C"
          }
        },
        "SHA1": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "SHA1"
          }
        },
        "SHA256": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "SHA256"
          }
        },
        "CRC64NVME": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "CRC64NVME"
          }
        },
        "SHA512": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "SHA512"
          }
        },
        "MD5": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "MD5"
          }
        },
        "XXHASH64": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "XXHASH64"
          }
        },
        "XXHASH3": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "XXHASH3"
          }
        },
        "XXHASH128": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "XXHASH128"
          }
        }
      }
    },
    "com.amazonaws.s3#ChecksumAlgorithmList": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#ChecksumAlgorithm"
      }
    },
    "com.amazonaws.s3#ChecksumCRC32": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumCRC32C": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumCRC64NVME": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumMD5": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumMode": {
      "type": "enum",
      "members": {
        "ENABLED": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "ENABLED"
          }
        }
      }
    },
    "com.amazonaws.s3#ChecksumSHA1": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumSHA256": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumSHA512": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumType": {
      "type": "enum",
      "members": {
        "COMPOSITE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "COMPOSITE"
          }
        },
        "FULL_OBJECT": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "FULL_OBJECT"
          }
        }
      }
    },
    "com.amazonaws.s3#ChecksumXXHASH128": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumXXHASH3": {
      "type": "string"
    },
    "com.amazonaws.s3#ChecksumXXHASH64": {
      "type": "string"
    },
    "com.amazonaws.s3#Code": {
      "type": "string"
    },
    "com.amazonaws.s3#CommonPrefix": {
      "type": "structure",
      "members": {
        "Prefix": {
          "target": "com.amazonaws.s3#Prefix",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#commonprefixprefix"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-commonprefix"
        }
      }
    },
    "com.amazonaws.s3#CommonPrefixList": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#CommonPrefix"
      }
    },
    "com.amazonaws.s3#CompletedMultipartUpload": {
      "type": "structure",
      "members": {
        "Parts": {
          "target": "com.amazonaws.s3#CompletedPartList",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedmultipartuploadparts"
            },
            "smithy.api#xmlFlattened": {},
            "smithy.api#xmlName": "Part"
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-completedmultipartupload"
        }
      }
    },
    "com.amazonaws.s3#CompletedPart": {
      "type": "structure",
      "members": {
        "ETag": {
          "target": "com.amazonaws.s3#ETag",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartetag"
            }
          }
        },
        "ChecksumCRC32": {
          "target": "com.amazonaws.s3#ChecksumCRC32",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksumcrc32"
            }
          }
        },
        "ChecksumCRC32C": {
          "target": "com.amazonaws.s3#ChecksumCRC32C",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksumcrc32c"
            }
          }
        },
        "ChecksumCRC64NVME": {
          "target": "com.amazonaws.s3#ChecksumCRC64NVME",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksumcrc64nvme"
            }
          }
        },
        "ChecksumSHA1": {
          "target": "com.amazonaws.s3#ChecksumSHA1",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksumsha1"
            }
          }
        },
        "ChecksumSHA256": {
          "target": "com.amazonaws.s3#ChecksumSHA256",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksumsha256"
            }
          }
        },
        "ChecksumSHA512": {
          "target": "com.amazonaws.s3#ChecksumSHA512",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksumsha512"
            }
          }
        },
        "ChecksumMD5": {
          "target": "com.amazonaws.s3#ChecksumMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksummd5"
            }
          }
        },
        "ChecksumXXHASH64": {
          "target": "com.amazonaws.s3#ChecksumXXHASH64",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksumxxhash64"
            }
          }
        },
        "ChecksumXXHASH3": {
          "target": "com.amazonaws.s3#ChecksumXXHASH3",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksumxxhash3"
            }
          }
        },
        "ChecksumXXHASH128": {
          "target": "com.amazonaws.s3#ChecksumXXHASH128",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartchecksumxxhash128"
            }
          }
        },
        "PartNumber": {
          "target": "com.amazonaws.s3#PartNumber",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#completedpartpartnumber"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-completedpart"
        }
      }
    },
    "com.amazonaws.s3#CompletedPartList": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#CompletedPart"
      }
    },
    "com.amazonaws.s3#ContentDisposition": {
      "type": "string"
    },
    "com.amazonaws.s3#ContentEncoding": {
      "type": "string"
    },
    "com.amazonaws.s3#ContentLanguage": {
      "type": "string"
    },
    "com.amazonaws.s3#ContentLength": {
      "type": "long"
    },
    "com.amazonaws.s3#ContentMD5": {
      "type": "string"
    },
    "com.amazonaws.s3#ContentRange": {
      "type": "string"
    },
    "com.amazonaws.s3#ContentType": {
      "type": "string"
    },
    "com.amazonaws.s3#CopyObjectResult": {
      "type": "structure",
      "members": {
        "ETag": {
          "target": "com.amazonaws.s3#ETag",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultetag"
            }
          }
        },
        "LastModified": {
          "target": "com.amazonaws.s3#LastModified",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultlastmodified"
            }
          }
        },
        "ChecksumType": {
          "target": "com.amazonaws.s3#ChecksumType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumtype"
            }
          }
        },
        "ChecksumCRC32": {
          "target": "com.amazonaws.s3#ChecksumCRC32",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumcrc32"
            }
          }
        },
        "ChecksumCRC32C": {
          "target": "com.amazonaws.s3#ChecksumCRC32C",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumcrc32c"
            }
          }
        },
        "ChecksumCRC64NVME": {
          "target": "com.amazonaws.s3#ChecksumCRC64NVME",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumcrc64nvme"
            }
          }
        },
        "ChecksumSHA1": {
          "target": "com.amazonaws.s3#ChecksumSHA1",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumsha1"
            }
          }
        },
        "ChecksumSHA256": {
          "target": "com.amazonaws.s3#ChecksumSHA256",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumsha256"
            }
          }
        },
        "ChecksumSHA512": {
          "target": "com.amazonaws.s3#ChecksumSHA512",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumsha512"
            }
          }
        },
        "ChecksumMD5": {
          "target": "com.amazonaws.s3#ChecksumMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksummd5"
            }
          }
        },
        "ChecksumXXHASH64": {
          "target": "com.amazonaws.s3#ChecksumXXHASH64",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumxxhash64"
            }
          }
        },
        "ChecksumXXHASH3": {
          "target": "com.amazonaws.s3#ChecksumXXHASH3",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumxxhash3"
            }
          }
        },
        "ChecksumXXHASH128": {
          "target": "com.amazonaws.s3#ChecksumXXHASH128",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copyobjectresultchecksumxxhash128"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-copyobjectresult"
        }
      }
    },
    "com.amazonaws.s3#CopyPartResult": {
      "type": "structure",
      "members": {
        "ETag": {
          "target": "com.amazonaws.s3#ETag",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultetag"
            }
          }
        },
        "LastModified": {
          "target": "com.amazonaws.s3#LastModified",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultlastmodified"
            }
          }
        },
        "ChecksumCRC32": {
          "target": "com.amazonaws.s3#ChecksumCRC32",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksumcrc32"
            }
          }
        },
        "ChecksumCRC32C": {
          "target": "com.amazonaws.s3#ChecksumCRC32C",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksumcrc32c"
            }
          }
        },
        "ChecksumCRC64NVME": {
          "target": "com.amazonaws.s3#ChecksumCRC64NVME",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksumcrc64nvme"
            }
          }
        },
        "ChecksumSHA1": {
          "target": "com.amazonaws.s3#ChecksumSHA1",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksumsha1"
            }
          }
        },
        "ChecksumSHA256": {
          "target": "com.amazonaws.s3#ChecksumSHA256",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksumsha256"
            }
          }
        },
        "ChecksumSHA512": {
          "target": "com.amazonaws.s3#ChecksumSHA512",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksumsha512"
            }
          }
        },
        "ChecksumMD5": {
          "target": "com.amazonaws.s3#ChecksumMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksummd5"
            }
          }
        },
        "ChecksumXXHASH64": {
          "target": "com.amazonaws.s3#ChecksumXXHASH64",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksumxxhash64"
            }
          }
        },
        "ChecksumXXHASH3": {
          "target": "com.amazonaws.s3#ChecksumXXHASH3",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksumxxhash3"
            }
          }
        },
        "ChecksumXXHASH128": {
          "target": "com.amazonaws.s3#ChecksumXXHASH128",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#copypartresultchecksumxxhash128"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-copypartresult"
        }
      }
    },
    "com.amazonaws.s3#CopySource": {
      "type": "string",
      "traits": {
        "smithy.api#pattern": "^\\/?.+\\/.+$"
      }
    },
    "com.amazonaws.s3#CopySourceIfMatch": {
      "type": "string"
    },
    "com.amazonaws.s3#CopySourceIfModifiedSince": {
      "type": "timestamp"
    },
    "com.amazonaws.s3#CopySourceIfNoneMatch": {
      "type": "string"
    },
    "com.amazonaws.s3#CopySourceIfUnmodifiedSince": {
      "type": "timestamp"
    },
    "com.amazonaws.s3#CopySourceRange": {
      "type": "string"
    },
    "com.amazonaws.s3#CopySourceSSECustomerAlgorithm": {
      "type": "string"
    },
    "com.amazonaws.s3#CopySourceSSECustomerKey": {
      "type": "string",
      "traits": {
        "smithy.api#sensitive": {}
      }
    },
    "com.amazonaws.s3#CopySourceSSECustomerKeyMD5": {
      "type": "string"
    },
    "com.amazonaws.s3#CopySourceVersionId": {
      "type": "string"
    },
    "com.amazonaws.s3#Delete": {
      "type": "structure",
      "members": {
        "Objects": {
          "target": "com.amazonaws.s3#ObjectIdentifierList",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#deleteobjects"
            },
            "smithy.api#required": {},
            "smithy.api#xmlFlattened": {},
            "smithy.api#xmlName": "Object"
          }
        },
        "Quiet": {
          "target": "com.amazonaws.s3#Quiet",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#deletequiet"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-delete"
        }
      }
    },
    "com.amazonaws.s3#DeleteMarker": {
      "type": "boolean"
    },
    "com.amazonaws.s3#DeleteMarkerVersionId": {
      "type": "string"
    },
    "com.amazonaws.s3#DeletedObject": {
      "type": "structure",
      "members": {
        "Key": {
          "target": "com.amazonaws.s3#ObjectKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#deletedobjectkey"
            }
          }
        },
        "VersionId": {
          "target": "com.amazonaws.s3#ObjectVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#deletedobjectversionid"
            }
          }
        },
        "DeleteMarker": {
          "target": "com.amazonaws.s3#DeleteMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#deletedobjectdeletemarker"
            }
          }
        },
        "DeleteMarkerVersionId": {
          "target": "com.amazonaws.s3#DeleteMarkerVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#deletedobjectdeletemarkerversionid"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-deletedobject"
        }
      }
    },
    "com.amazonaws.s3#DeletedObjects": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#DeletedObject"
      }
    },
    "com.amazonaws.s3#Delimiter": {
      "type": "string"
    },
    "com.amazonaws.s3#DisplayName": {
      "type": "string"
    },
    "com.amazonaws.s3#ETag": {
      "type": "string"
    },
    "com.amazonaws.s3#EncodingType": {
      "type": "enum",
      "members": {
        "url": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "url"
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-encodingtype"
        }
      }
    },
    "com.amazonaws.s3#EncryptionTypeMismatch": {
      "type": "structure",
      "members": {},
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-400-encryptiontypemismatch"
        },
        "smithy.api#error": "client",
        "smithy.api#httpError": 400
      }
    },
    "com.amazonaws.s3#Error": {
      "type": "structure",
      "members": {
        "Key": {
          "target": "com.amazonaws.s3#ObjectKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#errorkey"
            }
          }
        },
        "VersionId": {
          "target": "com.amazonaws.s3#ObjectVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#errorversionid"
            }
          }
        },
        "Code": {
          "target": "com.amazonaws.s3#Code",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#errorcode"
            }
          }
        },
        "Message": {
          "target": "com.amazonaws.s3#Message",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#errormessage"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-error"
        }
      }
    },
    "com.amazonaws.s3#Errors": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#Error"
      }
    },
    "com.amazonaws.s3#Expiration": {
      "type": "string"
    },
    "com.amazonaws.s3#Expires": {
      "type": "string"
    },
    "com.amazonaws.s3#FetchOwner": {
      "type": "boolean"
    },
    "com.amazonaws.s3#GrantFullControl": {
      "type": "string"
    },
    "com.amazonaws.s3#GrantRead": {
      "type": "string"
    },
    "com.amazonaws.s3#GrantReadACP": {
      "type": "string"
    },
    "com.amazonaws.s3#GrantWriteACP": {
      "type": "string"
    },
    "com.amazonaws.s3#ID": {
      "type": "string"
    },
    "com.amazonaws.s3#IfMatch": {
      "type": "string"
    },
    "com.amazonaws.s3#IfMatchInitiatedTime": {
      "type": "timestamp",
      "traits": {
        "smithy.api#timestampFormat": "http-date"
      }
    },
    "com.amazonaws.s3#IfMatchLastModifiedTime": {
      "type": "timestamp",
      "traits": {
        "smithy.api#timestampFormat": "http-date"
      }
    },
    "com.amazonaws.s3#IfMatchSize": {
      "type": "long"
    },
    "com.amazonaws.s3#IfModifiedSince": {
      "type": "timestamp"
    },
    "com.amazonaws.s3#IfNoneMatch": {
      "type": "string"
    },
    "com.amazonaws.s3#IfUnmodifiedSince": {
      "type": "timestamp"
    },
    "com.amazonaws.s3#Initiated": {
      "type": "timestamp"
    },
    "com.amazonaws.s3#Initiator": {
      "type": "structure",
      "members": {
        "ID": {
          "target": "com.amazonaws.s3#ID",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#initiatorid"
            }
          }
        },
        "DisplayName": {
          "target": "com.amazonaws.s3#DisplayName",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#initiatordisplayname"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-initiator"
        }
      }
    },
    "com.amazonaws.s3#IntelligentTieringAccessTier": {
      "type": "enum",
      "members": {
        "ARCHIVE_ACCESS": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "ARCHIVE_ACCESS"
          }
        },
        "DEEP_ARCHIVE_ACCESS": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "DEEP_ARCHIVE_ACCESS"
          }
        }
      }
    },
    "com.amazonaws.s3#InvalidObjectState": {
      "type": "structure",
      "members": {
        "StorageClass": {
          "target": "com.amazonaws.s3#StorageClass"
        },
        "AccessTier": {
          "target": "com.amazonaws.s3#IntelligentTieringAccessTier"
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-403-invalidobjectstate"
        },
        "smithy.api#error": "client",
        "smithy.api#httpError": 403
      }
    },
    "com.amazonaws.s3#InvalidRequest": {
      "type": "structure",
      "members": {},
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-400-invalidrequest"
        },
        "smithy.api#error": "client",
        "smithy.api#httpError": 400
      }
    },
    "com.amazonaws.s3#InvalidWriteOffset": {
      "type": "structure",
      "members": {},
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-400-invalidwriteoffset"
        },
        "smithy.api#error": "client",
        "smithy.api#httpError": 400
      }
    },
    "com.amazonaws.s3#IsRestoreInProgress": {
      "type": "boolean"
    },
    "com.amazonaws.s3#IsTruncated": {
      "type": "boolean"
    },
    "com.amazonaws.s3#KeyCount": {
      "type": "integer"
    },
    "com.amazonaws.s3#KeyMarker": {
      "type": "string"
    },
    "com.amazonaws.s3#LastModified": {
      "type": "timestamp"
    },
    "com.amazonaws.s3#LastModifiedTime": {
      "type": "timestamp",
      "traits": {
        "smithy.api#timestampFormat": "http-date"
      }
    },
    "com.amazonaws.s3#Location": {
      "type": "string"
    },
    "com.amazonaws.s3#LocationType": {
      "type": "enum",
      "members": {
        "AvailabilityZone": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "AvailabilityZone"
          }
        },
        "LocalZone": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "LocalZone"
          }
        }
      }
    },
    "com.amazonaws.s3#MFA": {
      "type": "string"
    },
    "com.amazonaws.s3#MaxKeys": {
      "type": "integer"
    },
    "com.amazonaws.s3#MaxParts": {
      "type": "integer"
    },
    "com.amazonaws.s3#MaxUploads": {
      "type": "integer"
    },
    "com.amazonaws.s3#Message": {
      "type": "string"
    },
    "com.amazonaws.s3#Metadata": {
      "type": "map",
      "key": {
        "target": "com.amazonaws.s3#MetadataKey"
      },
      "value": {
        "target": "com.amazonaws.s3#MetadataValue"
      }
    },
    "com.amazonaws.s3#MetadataDirective": {
      "type": "enum",
      "members": {
        "COPY": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "COPY"
          }
        },
        "REPLACE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "REPLACE"
          }
        }
      }
    },
    "com.amazonaws.s3#MetadataKey": {
      "type": "string"
    },
    "com.amazonaws.s3#MetadataValue": {
      "type": "string"
    },
    "com.amazonaws.s3#MissingMeta": {
      "type": "integer"
    },
    "com.amazonaws.s3#MpuObjectSize": {
      "type": "long"
    },
    "com.amazonaws.s3#MultipartUpload": {
      "type": "structure",
      "members": {
        "UploadId": {
          "target": "com.amazonaws.s3#MultipartUploadId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#multipartuploaduploadid"
            }
          }
        },
        "Key": {
          "target": "com.amazonaws.s3#ObjectKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#multipartuploadkey"
            }
          }
        },
        "Initiated": {
          "target": "com.amazonaws.s3#Initiated",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#multipartuploadinitiated"
            }
          }
        },
        "StorageClass": {
          "target": "com.amazonaws.s3#StorageClass",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#multipartuploadstorageclass"
            }
          }
        },
        "Owner": {
          "target": "com.amazonaws.s3#Owner",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#multipartuploadowner"
            }
          }
        },
        "Initiator": {
          "target": "com.amazonaws.s3#Initiator",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#multipartuploadinitiator"
            }
          }
        },
        "ChecksumAlgorithm": {
          "target": "com.amazonaws.s3#ChecksumAlgorithm",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#multipartuploadchecksumalgorithm"
            }
          }
        },
        "ChecksumType": {
          "target": "com.amazonaws.s3#ChecksumType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#multipartuploadchecksumtype"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-multipartupload"
        }
      }
    },
    "com.amazonaws.s3#MultipartUploadId": {
      "type": "string"
    },
    "com.amazonaws.s3#MultipartUploadList": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#MultipartUpload"
      }
    },
    "com.amazonaws.s3#NextKeyMarker": {
      "type": "string"
    },
    "com.amazonaws.s3#NextPartNumberMarker": {
      "type": "string"
    },
    "com.amazonaws.s3#NextToken": {
      "type": "string"
    },
    "com.amazonaws.s3#NextUploadIdMarker": {
      "type": "string"
    },
    "com.amazonaws.s3#NoSuchBucket": {
      "type": "structure",
      "members": {},
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-404-nosuchbucket"
        },
        "smithy.api#error": "client",
        "smithy.api#httpError": 404
      }
    },
    "com.amazonaws.s3#NoSuchKey": {
      "type": "structure",
      "members": {},
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-404-nosuchkey"
        },
        "smithy.api#error": "client",
        "smithy.api#httpError": 404
      }
    },
    "com.amazonaws.s3#NoSuchUpload": {
      "type": "structure",
      "members": {},
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-404-nosuchupload"
        },
        "smithy.api#error": "client",
        "smithy.api#httpError": 404
      }
    },
    "com.amazonaws.s3#NotFound": {
      "type": "structure",
      "members": {},
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-404-notfound"
        },
        "smithy.api#error": "client"
      }
    },
    "com.amazonaws.s3#Object": {
      "type": "structure",
      "members": {
        "Key": {
          "target": "com.amazonaws.s3#ObjectKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectkey"
            }
          }
        },
        "LastModified": {
          "target": "com.amazonaws.s3#LastModified",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectlastmodified"
            }
          }
        },
        "ETag": {
          "target": "com.amazonaws.s3#ETag",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectetag"
            }
          }
        },
        "ChecksumAlgorithm": {
          "target": "com.amazonaws.s3#ChecksumAlgorithmList",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectchecksumalgorithm"
            },
            "smithy.api#xmlFlattened": {}
          }
        },
        "ChecksumType": {
          "target": "com.amazonaws.s3#ChecksumType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectchecksumtype"
            }
          }
        },
        "Size": {
          "target": "com.amazonaws.s3#Size",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectsize"
            }
          }
        },
        "StorageClass": {
          "target": "com.amazonaws.s3#ObjectStorageClass",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectstorageclass"
            }
          }
        },
        "Owner": {
          "target": "com.amazonaws.s3#Owner",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectowner"
            }
          }
        },
        "RestoreStatus": {
          "target": "com.amazonaws.s3#RestoreStatus",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectrestorestatus"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-object"
        }
      }
    },
    "com.amazonaws.s3#ObjectCannedACL": {
      "type": "enum",
      "members": {
        "private": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "private"
          }
        },
        "public_read": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "public-read"
          }
        },
        "public_read_write": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "public-read-write"
          }
        },
        "authenticated_read": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "authenticated-read"
          }
        },
        "aws_exec_read": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "aws-exec-read"
          }
        },
        "bucket_owner_read": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "bucket-owner-read"
          }
        },
        "bucket_owner_full_control": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "bucket-owner-full-control"
          }
        }
      }
    },
    "com.amazonaws.s3#ObjectIdentifier": {
      "type": "structure",
      "members": {
        "Key": {
          "target": "com.amazonaws.s3#ObjectKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectidentifierkey"
            },
            "smithy.api#required": {}
          }
        },
        "VersionId": {
          "target": "com.amazonaws.s3#ObjectVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectidentifierversionid"
            }
          }
        },
        "ETag": {
          "target": "com.amazonaws.s3#ETag",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectidentifieretag"
            }
          }
        },
        "LastModifiedTime": {
          "target": "com.amazonaws.s3#LastModifiedTime",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectidentifierlastmodifiedtime"
            }
          }
        },
        "Size": {
          "target": "com.amazonaws.s3#Size",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#objectidentifiersize"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-objectidentifier"
        }
      }
    },
    "com.amazonaws.s3#ObjectIdentifierList": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#ObjectIdentifier"
      }
    },
    "com.amazonaws.s3#ObjectKey": {
      "type": "string",
      "traits": {
        "smithy.api#length": {
          "min": 1
        }
      }
    },
    "com.amazonaws.s3#ObjectList": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#Object"
      }
    },
    "com.amazonaws.s3#ObjectLockLegalHoldStatus": {
      "type": "enum",
      "members": {
        "ON": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "ON"
          }
        },
        "OFF": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "OFF"
          }
        }
      }
    },
    "com.amazonaws.s3#ObjectLockMode": {
      "type": "enum",
      "members": {
        "GOVERNANCE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "GOVERNANCE"
          }
        },
        "COMPLIANCE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "COMPLIANCE"
          }
        }
      }
    },
    "com.amazonaws.s3#ObjectLockRetainUntilDate": {
      "type": "timestamp",
      "traits": {
        "smithy.api#timestampFormat": "date-time"
      }
    },
    "com.amazonaws.s3#ObjectNotInActiveTierError": {
      "type": "structure",
      "members": {},
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-403-objectnotinactivetiererror"
        },
        "smithy.api#error": "client",
        "smithy.api#httpError": 403
      }
    },
    "com.amazonaws.s3#ObjectStorageClass": {
      "type": "enum",
      "members": {
        "STANDARD": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "STANDARD"
          }
        },
        "REDUCED_REDUNDANCY": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "REDUCED_REDUNDANCY"
          }
        },
        "GLACIER": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "GLACIER"
          }
        },
        "STANDARD_IA": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "STANDARD_IA"
          }
        },
        "ONEZONE_IA": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "ONEZONE_IA"
          }
        },
        "INTELLIGENT_TIERING": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "INTELLIGENT_TIERING"
          }
        },
        "DEEP_ARCHIVE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "DEEP_ARCHIVE"
          }
        },
        "OUTPOSTS": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "OUTPOSTS"
          }
        },
        "GLACIER_IR": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "GLACIER_IR"
          }
        },
        "SNOW": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "SNOW"
          }
        },
        "EXPRESS_ONEZONE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "EXPRESS_ONEZONE"
          }
        },
        "FSX_OPENZFS": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "FSX_OPENZFS"
          }
        },
        "FSX_ONTAP": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "FSX_ONTAP"
          }
        }
      }
    },
    "com.amazonaws.s3#ObjectVersionId": {
      "type": "string"
    },
    "com.amazonaws.s3#OptionalObjectAttributes": {
      "type": "enum",
      "members": {
        "RESTORE_STATUS": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "RestoreStatus"
          }
        }
      }
    },
    "com.amazonaws.s3#OptionalObjectAttributesList": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#OptionalObjectAttributes"
      }
    },
    "com.amazonaws.s3#Owner": {
      "type": "structure",
      "members": {
        "DisplayName": {
          "target": "com.amazonaws.s3#DisplayName",
          "traits": {
            "smithy.api#documentation": ""
          }
        },
        "ID": {
          "target": "com.amazonaws.s3#ID",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#ownerid"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-owner"
        }
      }
    },
    "com.amazonaws.s3#Part": {
      "type": "structure",
      "members": {
        "PartNumber": {
          "target": "com.amazonaws.s3#PartNumber",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partpartnumber"
            }
          }
        },
        "LastModified": {
          "target": "com.amazonaws.s3#LastModified",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partlastmodified"
            }
          }
        },
        "ETag": {
          "target": "com.amazonaws.s3#ETag",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partetag"
            }
          }
        },
        "Size": {
          "target": "com.amazonaws.s3#Size",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partsize"
            }
          }
        },
        "ChecksumCRC32": {
          "target": "com.amazonaws.s3#ChecksumCRC32",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksumcrc32"
            }
          }
        },
        "ChecksumCRC32C": {
          "target": "com.amazonaws.s3#ChecksumCRC32C",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksumcrc32c"
            }
          }
        },
        "ChecksumCRC64NVME": {
          "target": "com.amazonaws.s3#ChecksumCRC64NVME",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksumcrc64nvme"
            }
          }
        },
        "ChecksumSHA1": {
          "target": "com.amazonaws.s3#ChecksumSHA1",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksumsha1"
            }
          }
        },
        "ChecksumSHA256": {
          "target": "com.amazonaws.s3#ChecksumSHA256",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksumsha256"
            }
          }
        },
        "ChecksumSHA512": {
          "target": "com.amazonaws.s3#ChecksumSHA512",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksumsha512"
            }
          }
        },
        "ChecksumMD5": {
          "target": "com.amazonaws.s3#ChecksumMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksummd5"
            }
          }
        },
        "ChecksumXXHASH64": {
          "target": "com.amazonaws.s3#ChecksumXXHASH64",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksumxxhash64"
            }
          }
        },
        "ChecksumXXHASH3": {
          "target": "com.amazonaws.s3#ChecksumXXHASH3",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksumxxhash3"
            }
          }
        },
        "ChecksumXXHASH128": {
          "target": "com.amazonaws.s3#ChecksumXXHASH128",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partchecksumxxhash128"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-part"
        }
      }
    },
    "com.amazonaws.s3#PartNumber": {
      "type": "integer"
    },
    "com.amazonaws.s3#PartNumberMarker": {
      "type": "string"
    },
    "com.amazonaws.s3#Parts": {
      "type": "list",
      "member": {
        "target": "com.amazonaws.s3#Part"
      }
    },
    "com.amazonaws.s3#PartsCount": {
      "type": "integer"
    },
    "com.amazonaws.s3#Prefix": {
      "type": "string"
    },
    "com.amazonaws.s3#Quiet": {
      "type": "boolean"
    },
    "com.amazonaws.s3#Range": {
      "type": "string"
    },
    "com.amazonaws.s3#Region": {
      "type": "string",
      "traits": {
        "smithy.api#length": {
          "min": 0,
          "max": 20
        }
      }
    },
    "com.amazonaws.s3#ReplicationStatus": {
      "type": "enum",
      "members": {
        "COMPLETE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "COMPLETE"
          }
        },
        "PENDING": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "PENDING"
          }
        },
        "FAILED": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "FAILED"
          }
        },
        "REPLICA": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "REPLICA"
          }
        },
        "COMPLETED": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "COMPLETED"
          }
        }
      }
    },
    "com.amazonaws.s3#RequestCharged": {
      "type": "enum",
      "members": {
        "requester": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "requester"
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-requestcharged"
        }
      }
    },
    "com.amazonaws.s3#RequestPayer": {
      "type": "enum",
      "members": {
        "requester": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "requester"
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-requestpayer"
        }
      }
    },
    "com.amazonaws.s3#ResponseCacheControl": {
      "type": "string"
    },
    "com.amazonaws.s3#ResponseContentDisposition": {
      "type": "string"
    },
    "com.amazonaws.s3#ResponseContentEncoding": {
      "type": "string"
    },
    "com.amazonaws.s3#ResponseContentLanguage": {
      "type": "string"
    },
    "com.amazonaws.s3#ResponseContentType": {
      "type": "string"
    },
    "com.amazonaws.s3#ResponseExpires": {
      "type": "timestamp",
      "traits": {
        "smithy.api#timestampFormat": "http-date"
      }
    },
    "com.amazonaws.s3#Restore": {
      "type": "string"
    },
    "com.amazonaws.s3#RestoreExpiryDate": {
      "type": "timestamp"
    },
    "com.amazonaws.s3#RestoreStatus": {
      "type": "structure",
      "members": {
        "IsRestoreInProgress": {
          "target": "com.amazonaws.s3#IsRestoreInProgress",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#restorestatusisrestoreinprogress"
            }
          }
        },
        "RestoreExpiryDate": {
          "target": "com.amazonaws.s3#RestoreExpiryDate",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#restorestatusrestoreexpirydate"
            }
          }
        }
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-restorestatus"
        }
      }
    },
    "com.amazonaws.s3#S3RegionalOrS3ExpressBucketArnString": {
      "type": "string",
      "traits": {
        "smithy.api#length": {
          "min": 1,
          "max": 128
        },
        "smithy.api#pattern": "^arn:[^:]+:(s3|s3express):"
      }
    },
    "com.amazonaws.s3#SSECustomerAlgorithm": {
      "type": "string"
    },
    "com.amazonaws.s3#SSECustomerKey": {
      "type": "string",
      "traits": {
        "smithy.api#sensitive": {}
      }
    },
    "com.amazonaws.s3#SSECustomerKeyMD5": {
      "type": "string"
    },
    "com.amazonaws.s3#SSEKMSEncryptionContext": {
      "type": "string",
      "traits": {
        "smithy.api#sensitive": {}
      }
    },
    "com.amazonaws.s3#SSEKMSKeyId": {
      "type": "string",
      "traits": {
        "smithy.api#sensitive": {}
      }
    },
    "com.amazonaws.s3#ServerSideEncryption": {
      "type": "enum",
      "members": {
        "AES256": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "AES256"
          }
        },
        "aws_fsx": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "aws:fsx"
          }
        },
        "aws_kms": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "aws:kms"
          }
        },
        "aws_kms_dsse": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "aws:kms:dsse"
          }
        }
      }
    },
    "com.amazonaws.s3#Size": {
      "type": "long"
    },
    "com.amazonaws.s3#StartAfter": {
      "type": "string"
    },
    "com.amazonaws.s3#StorageClass": {
      "type": "enum",
      "members": {
        "STANDARD": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "STANDARD"
          }
        },
        "REDUCED_REDUNDANCY": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "REDUCED_REDUNDANCY"
          }
        },
        "STANDARD_IA": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "STANDARD_IA"
          }
        },
        "ONEZONE_IA": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "ONEZONE_IA"
          }
        },
        "INTELLIGENT_TIERING": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "INTELLIGENT_TIERING"
          }
        },
        "GLACIER": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "GLACIER"
          }
        },
        "DEEP_ARCHIVE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "DEEP_ARCHIVE"
          }
        },
        "OUTPOSTS": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "OUTPOSTS"
          }
        },
        "GLACIER_IR": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "GLACIER_IR"
          }
        },
        "SNOW": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "SNOW"
          }
        },
        "EXPRESS_ONEZONE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "EXPRESS_ONEZONE"
          }
        },
        "FSX_OPENZFS": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "FSX_OPENZFS"
          }
        },
        "FSX_ONTAP": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "FSX_ONTAP"
          }
        }
      }
    },
    "com.amazonaws.s3#StreamingBlob": {
      "type": "blob",
      "traits": {
        "smithy.api#streaming": {}
      }
    },
    "com.amazonaws.s3#TagCount": {
      "type": "integer"
    },
    "com.amazonaws.s3#TaggingDirective": {
      "type": "enum",
      "members": {
        "COPY": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "COPY"
          }
        },
        "REPLACE": {
          "target": "smithy.api#Unit",
          "traits": {
            "smithy.api#enumValue": "REPLACE"
          }
        }
      }
    },
    "com.amazonaws.s3#TaggingHeader": {
      "type": "string"
    },
    "com.amazonaws.s3#Token": {
      "type": "string"
    },
    "com.amazonaws.s3#TooManyParts": {
      "type": "structure",
      "members": {},
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#schema-400-toomanyparts"
        },
        "smithy.api#error": "client",
        "smithy.api#httpError": 400
      }
    },
    "com.amazonaws.s3#UploadIdMarker": {
      "type": "string"
    },
    "com.amazonaws.s3#WebsiteRedirectLocation": {
      "type": "string"
    },
    "com.amazonaws.s3#WriteOffsetBytes": {
      "type": "long"
    }
  }
}
```

</details>
