# ListObjectsV2

[Request](#request) → [Response](#response-200-success) 𝄁 [Error](#errors)

Lists the objects within a bucket, returning up to 1,000 object keys per request.

**Pagination and Filtering:** You can use query parameters to filter results by prefix, group keys using a delimiter, or paginate through large datasets using a continuation token. Objects are sorted in lexicographical order by their key name.

**Permissions:** Requesters must have permission to list the contents of the bucket. Under the standard S3 IAM policy model, this requires the `s3:ListBucket` action.

## Request

```HTTP
GET /{bucket}?list-type=2 HTTP/1.1
```

### Path: `bucket`

The name of the bucket containing the objects to list.

### Query: `delimiter`

A character used to group keys that share a common prefix. Using a delimiter groups keys containing that delimiter into `CommonPrefixes` results, returning only the unique common prefix rather than all matching objects. Grouped prefixes are excluded from the results if they are not lexicographically greater than the `start-after` value (if specified).

### Query: `encoding-type`

Specifies the encoding method to use for object keys in the response (e.g. `url`). This is useful for handling keys that contain characters (such as control characters or non-ASCII characters) that are not supported by standard XML 1.0 parsers. When URL encoding is used, characters not compatible with XML are percent-encoded in UTF-8 format.

### Query: `max-keys`

Sets the maximum number of keys returned in the response body (between 1 and 1,000).

### Query: `prefix`

Restricts the returned keys to those that begin with the specified prefix.

### Query: `continuation-token`

A token returned by a previous truncated response, indicating that the listing should continue from where it left off. Clients must pass this value in subsequent requests to paginate through remaining results.

### Query: `fetch-owner`

By default, owner information is omitted from the response to optimize performance. Setting this parameter to `true` forces the server to return owner metadata for each listed object.

### Query: `start-after`

Specifies the object key after which listing should begin. This parameter can be any key in the bucket and is used to start the listing at a specific point without using a pagination token.

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `x-amz-optional-object-attributes`

A comma-separated list of optional object attributes to include in the response (such as `RestoreStatus`). Fields not explicitly requested are omitted.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult>
	<IsTruncated>boolean</IsTruncated>
	<Contents>
		<Key>string</Key>
		<LastModified>timestamp</LastModified>
		<ETag>string</ETag>
		<ChecksumAlgorithm>string</ChecksumAlgorithm>
		<ChecksumType>string</ChecksumType>
		<Size>integer</Size>
		<StorageClass>string</StorageClass>
		<Owner>
			<DisplayName>string</DisplayName>
			<ID>string</ID>
		</Owner>
		<RestoreStatus>
			<IsRestoreInProgress>boolean</IsRestoreInProgress>
			<RestoreExpiryDate>timestamp</RestoreExpiryDate>
		</RestoreStatus>
	</Contents>
	<Name>string</Name>
	<Prefix>string</Prefix>
	<Delimiter>string</Delimiter>
	<MaxKeys>integer</MaxKeys>
	<CommonPrefixes>
		<Prefix>string</Prefix>
	</CommonPrefixes>
	<EncodingType>string</EncodingType>
	<KeyCount>integer</KeyCount>
	<ContinuationToken>string</ContinuationToken>
	<NextContinuationToken>string</NextContinuationToken>
	<StartAfter>string</StartAfter>
</ListBucketResult>
```

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

### Body: `<ListBucketResult>`

#### `<IsTruncated>`

A boolean flag indicating whether there are more results than were returned in this response.

#### `<Contents>`

Metadata about each object returned.

Type: Array of [Object](shared-shapes.md#schema-object)

#### `<Name>`

The name of the bucket containing the listed objects.

#### `<Prefix>`

The prefix string used to filter results, if specified in the request.

#### `<Delimiter>`

The delimiter character used to group keys, if specified in the request.

#### `<MaxKeys>`

The maximum number of keys requested in the query.

#### `<CommonPrefixes>`

A container listing distinct prefixes grouped by the delimiter parameter. Each distinct prefix is returned as a `<Prefix>` child element and represents keys that share a common prefix up to the delimiter character. These grouped prefixes act similarly to subdirectories in the path structure.

Type: Array of [CommonPrefix](shared-shapes.md#schema-commonprefix)

#### `<EncodingType>`

The encoding method used for object keys in the response (such as `url`), if specified in the request. When present, values in the `Delimiter`, `Prefix`, `Key`, and `StartAfter` elements are encoded.

#### `<KeyCount>`

The number of keys (including rolled-up common prefixes) returned in this response page.

#### `<ContinuationToken>`

The pagination token that was passed in the request, if specified.

#### `<NextContinuationToken>`

When a response is truncated, this specifies the pagination token to pass in the subsequent request to retrieve the next page of results.

#### `<StartAfter>`

The `start-after` key that was passed in the request, if specified.

## Errors

### `404` `NoSuchBucket`
The specified bucket does not exist.

```HTTP
HTTP/1.1 404 Not Found
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>NoSuchBucket</Code>
	<Message>The specified bucket does not exist.</Message>
</Error>
```

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#ListObjectsV2": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#ListObjectsV2Request"
      },
      "output": {
        "target": "com.amazonaws.s3#ListObjectsV2Output"
      },
      "errors": [
        {
          "target": "com.amazonaws.s3#NoSuchBucket"
        }
      ],
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#listobjectsv2"
        },
        "smithy.api#examples": [
          {
            "title": "To get object list",
            "documentation": "The following example retrieves object list. The request specifies max keys to limit response to include only 2 object keys. ",
            "input": {
              "Bucket": "DOC-EXAMPLE-BUCKET",
              "MaxKeys": 2
            },
            "output": {
              "Name": "DOC-EXAMPLE-BUCKET",
              "MaxKeys": 2,
              "Prefix": "",
              "KeyCount": 2,
              "NextContinuationToken": "1w41l63U0xa8q7smH50vCxyTQqdxo69O3EmK28Bi5PcROI4wI/EyIJg==",
              "IsTruncated": true,
              "Contents": [
                {
                  "LastModified": "2014-11-21T19:40:05.000Z",
                  "ETag": "\"70ee1738b6b21e2c8a43f3a5ab0eee71\"",
                  "StorageClass": "STANDARD",
                  "Key": "happyface.jpg",
                  "Size": 11
                },
                {
                  "LastModified": "2014-05-02T04:51:50.000Z",
                  "ETag": "\"becf17f89c30367a9a44495d62ed521a-1\"",
                  "StorageClass": "STANDARD",
                  "Key": "test.jpg",
                  "Size": 4192256
                }
              ]
            }
          }
        ],
        "smithy.api#http": {
          "method": "GET",
          "uri": "/{Bucket}?list-type=2",
          "code": 200
        },
        "smithy.api#paginated": {
          "inputToken": "ContinuationToken",
          "outputToken": "NextContinuationToken",
          "pageSize": "MaxKeys"
        }
      }
    },
    "com.amazonaws.s3#ListObjectsV2Output": {
      "type": "structure",
      "members": {
        "IsTruncated": {
          "target": "com.amazonaws.s3#IsTruncated",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#istruncated"
            }
          }
        },
        "Contents": {
          "target": "com.amazonaws.s3#ObjectList",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#contents"
            },
            "smithy.api#xmlFlattened": {}
          }
        },
        "Name": {
          "target": "com.amazonaws.s3#BucketName",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#name"
            }
          }
        },
        "Prefix": {
          "target": "com.amazonaws.s3#Prefix",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#prefix"
            }
          }
        },
        "Delimiter": {
          "target": "com.amazonaws.s3#Delimiter",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#delimiter"
            }
          }
        },
        "MaxKeys": {
          "target": "com.amazonaws.s3#MaxKeys",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#maxkeys"
            }
          }
        },
        "CommonPrefixes": {
          "target": "com.amazonaws.s3#CommonPrefixList",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#commonprefixes"
            },
            "smithy.api#xmlFlattened": {}
          }
        },
        "EncodingType": {
          "target": "com.amazonaws.s3#EncodingType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#encodingtype"
            }
          }
        },
        "KeyCount": {
          "target": "com.amazonaws.s3#KeyCount",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#keycount"
            }
          }
        },
        "ContinuationToken": {
          "target": "com.amazonaws.s3#Token",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#continuationtoken"
            }
          }
        },
        "NextContinuationToken": {
          "target": "com.amazonaws.s3#NextToken",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#nextcontinuationtoken"
            }
          }
        },
        "StartAfter": {
          "target": "com.amazonaws.s3#StartAfter",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#startafter"
            }
          }
        },
        "RequestCharged": {
          "target": "com.amazonaws.s3#RequestCharged",
          "traits": {
            "smithy.api#httpHeader": "x-amz-request-charged",
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-request-charged"
            }
          }
        }
      },
      "traits": {
        "smithy.api#output": {},
        "smithy.api#xmlName": "ListBucketResult"
      }
    },
    "com.amazonaws.s3#ListObjectsV2Request": {
      "type": "structure",
      "members": {
        "Bucket": {
          "target": "com.amazonaws.s3#BucketName",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#path-bucket"
            },
            "smithy.api#httpLabel": {},
            "smithy.api#required": {},
            "smithy.rules#contextParam": {
              "name": "Bucket"
            }
          }
        },
        "Delimiter": {
          "target": "com.amazonaws.s3#Delimiter",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-delimiter"
            },
            "smithy.api#httpQuery": "delimiter"
          }
        },
        "EncodingType": {
          "target": "com.amazonaws.s3#EncodingType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-encoding-type"
            },
            "smithy.api#httpQuery": "encoding-type"
          }
        },
        "MaxKeys": {
          "target": "com.amazonaws.s3#MaxKeys",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-max-keys"
            },
            "smithy.api#httpQuery": "max-keys"
          }
        },
        "Prefix": {
          "target": "com.amazonaws.s3#Prefix",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-prefix"
            },
            "smithy.api#httpQuery": "prefix",
            "smithy.rules#contextParam": {
              "name": "Prefix"
            }
          }
        },
        "ContinuationToken": {
          "target": "com.amazonaws.s3#Token",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-continuation-token"
            },
            "smithy.api#httpQuery": "continuation-token"
          }
        },
        "FetchOwner": {
          "target": "com.amazonaws.s3#FetchOwner",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-fetch-owner"
            },
            "smithy.api#httpQuery": "fetch-owner"
          }
        },
        "StartAfter": {
          "target": "com.amazonaws.s3#StartAfter",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-start-after"
            },
            "smithy.api#httpQuery": "start-after"
          }
        },
        "RequestPayer": {
          "target": "com.amazonaws.s3#RequestPayer",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-request-payer"
            },
            "smithy.api#httpHeader": "x-amz-request-payer"
          }
        },
        "ExpectedBucketOwner": {
          "target": "com.amazonaws.s3#AccountId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-expected-bucket-owner"
            },
            "smithy.api#httpHeader": "x-amz-expected-bucket-owner"
          }
        },
        "OptionalObjectAttributes": {
          "target": "com.amazonaws.s3#OptionalObjectAttributesList",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-optional-object-attributes"
            },
            "smithy.api#httpHeader": "x-amz-optional-object-attributes"
          }
        }
      },
      "traits": {
        "smithy.api#input": {}
      }
    }
  }
}
```

</details>
