# ListMultipartUploads

[Request](#request) → [Response](#response-200-success)

Lists in-progress multipart uploads in a bucket.

**In-Progress Uploads:** An in-progress multipart upload is a session that has been initiated via `CreateMultipartUpload` but has not yet been completed (using `CompleteMultipartUpload`) or aborted (using `AbortMultipartUpload`).

**Pagination and Sorting:** The operation returns up to 1,000 multipart uploads per response. If there are more uploads matching the criteria, the response returns `<IsTruncated>true</IsTruncated>` along with `<NextKeyMarker>` and `<NextUploadIdMarker>`. Clients must pass these values in subsequent requests as `key-marker` and `upload-id-marker` query parameters to paginate through the remaining results. Multipart uploads are sorted first by key in ascending order, and then by initiation time for uploads sharing the same key.

**Permissions:** Requesters must have permission to list multipart uploads. Under the standard S3 IAM policy model, this requires the `s3:ListBucketMultipartUploads` action.

## Request

```HTTP
GET /{bucket}?uploads HTTP/1.1
```

### Path: `bucket`

The name of the bucket containing the in-progress multipart uploads to list.

### Query: `delimiter`

A character used to group keys that share a common prefix. Using a delimiter groups keys containing that delimiter into `CommonPrefixes` results, returning only the unique common prefix rather than all matching uploads.

### Query: `encoding-type`

Specifies the encoding method to use for object keys in the response (e.g. `url`). This is useful for handling keys that contain characters not supported by standard XML 1.0 parsers.

### Query: `key-marker`

Specifies the object key after which listing should begin. This is used in conjunction with `upload-id-marker` to paginate results. If `upload-id-marker` is also specified, the listing includes uploads for keys equal to the marker if their upload IDs are lexicographically greater than the marker.

### Query: `max-uploads`

Sets the maximum number of multipart uploads to return in the response body (between 1 and 1,000).

### Query: `prefix`

Restricts the returned uploads to those whose object keys begin with the specified prefix.

### Query: `upload-id-marker`

Together with `key-marker`, specifies the multipart upload identifier after which listing should begin. If `key-marker` is not specified, this parameter is ignored. When both are specified, the listing includes uploads for keys equal to the `key-marker` only if their upload IDs are lexicographically greater than `upload-id-marker`.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<ListMultipartUploadsResult>
	<Bucket>string</Bucket>
	<KeyMarker>string</KeyMarker>
	<UploadIdMarker>string</UploadIdMarker>
	<NextKeyMarker>string</NextKeyMarker>
	<Prefix>string</Prefix>
	<Delimiter>string</Delimiter>
	<NextUploadIdMarker>string</NextUploadIdMarker>
	<MaxUploads>integer</MaxUploads>
	<IsTruncated>boolean</IsTruncated>
	<Upload>
		<UploadId>string</UploadId>
		<Key>string</Key>
		<Initiated>timestamp</Initiated>
		<StorageClass>string</StorageClass>
		<Owner>
			<DisplayName>string</DisplayName>
			<ID>string</ID>
		</Owner>
		<Initiator>
			<ID>string</ID>
			<DisplayName>string</DisplayName>
		</Initiator>
		<ChecksumAlgorithm>string</ChecksumAlgorithm>
		<ChecksumType>string</ChecksumType>
	</Upload>
	<CommonPrefixes>
		<Prefix>string</Prefix>
	</CommonPrefixes>
	<EncodingType>string</EncodingType>
</ListMultipartUploadsResult>
```

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

### Body: `<ListMultipartUploadsResult>`

#### `<Bucket>`

The name of the bucket containing the listed multipart uploads.

#### `<KeyMarker>`

The object key marker at which listing began.

#### `<UploadIdMarker>`

The upload identifier marker at which listing began.

#### `<NextKeyMarker>`

When a response is truncated, this specifies the key marker to pass in the subsequent request to retrieve the next page of results.

#### `<Prefix>`

The prefix string used to filter results, if specified in the request.

#### `<Delimiter>`

The delimiter character used to group keys, if specified in the request.

#### `<NextUploadIdMarker>`

When a response is truncated, this specifies the upload identifier marker to pass in the subsequent request to retrieve the next page of results.

#### `<MaxUploads>`

The maximum number of uploads requested in the query.

#### `<IsTruncated>`

A boolean flag indicating whether there are more results than were returned in this response.

#### `<Upload>`

A container element detailing a single in-progress multipart upload session.

Type: Array of [MultipartUpload](shared-shapes.md#schema-multipartupload)

#### `<CommonPrefixes>`

A container listing distinct prefixes grouped by the delimiter parameter.

Type: Array of [CommonPrefix](shared-shapes.md#schema-commonprefix)

#### `<EncodingType>`

The encoding method used for object keys in the response (such as `url`), if specified in the request. When present, values in the `Delimiter`, `KeyMarker`, `Prefix`, `NextKeyMarker`, and `Key` elements are encoded.

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#ListMultipartUploads": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#ListMultipartUploadsRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#ListMultipartUploadsOutput"
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#listmultipartuploads"
        },
        "smithy.api#examples": [
          {
            "title": "List next set of multipart uploads when previous result is truncated",
            "documentation": "The following example specifies the upload-id-marker and key-marker from previous truncated response to retrieve next setup of multipart uploads.",
            "input": {
              "Bucket": "examplebucket",
              "KeyMarker": "nextkeyfrompreviousresponse",
              "MaxUploads": 2,
              "UploadIdMarker": "valuefrompreviousresponse"
            },
            "output": {
              "UploadIdMarker": "",
              "NextKeyMarker": "someobjectkey",
              "Bucket": "acl1",
              "NextUploadIdMarker": "examplelo91lv1iwvWpvCiJWugw2xXLPAD7Z8cJyX9.WiIRgNrdG6Ldsn.9FtS63TCl1Uf5faTB.1U5Ckcbmdw--",
              "Uploads": [
                {
                  "Initiator": {
                    "DisplayName": "ownder-display-name",
                    "ID": "examplee7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
                  },
                  "Initiated": "2014-05-01T05:40:58.000Z",
                  "UploadId": "gZ30jIqlUa.CInXklLQtSMJITdUnoZ1Y5GACB5UckOtspm5zbDMCkPF_qkfZzMiFZ6dksmcnqxJyIBvQMG9X9Q--",
                  "StorageClass": "STANDARD",
                  "Key": "JavaFile",
                  "Owner": {
                    "DisplayName": "mohanataws",
                    "ID": "852b113e7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
                  }
                },
                {
                  "Initiator": {
                    "DisplayName": "ownder-display-name",
                    "ID": "examplee7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
                  },
                  "Initiated": "2014-05-01T05:41:27.000Z",
                  "UploadId": "b7tZSqIlo91lv1iwvWpvCiJWugw2xXLPAD7Z8cJyX9.WiIRgNrdG6Ldsn.9FtS63TCl1Uf5faTB.1U5Ckcbmdw--",
                  "StorageClass": "STANDARD",
                  "Key": "JavaFile",
                  "Owner": {
                    "DisplayName": "ownder-display-name",
                    "ID": "examplee7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
                  }
                }
              ],
              "KeyMarker": "",
              "MaxUploads": 2,
              "IsTruncated": true
            }
          },
          {
            "title": "To list in-progress multipart uploads on a bucket",
            "documentation": "The following example lists in-progress multipart uploads on a specific bucket.",
            "input": {
              "Bucket": "examplebucket"
            },
            "output": {
              "Uploads": [
                {
                  "Initiator": {
                    "DisplayName": "display-name",
                    "ID": "examplee7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
                  },
                  "Initiated": "2014-05-01T05:40:58.000Z",
                  "UploadId": "examplelUa.CInXklLQtSMJITdUnoZ1Y5GACB5UckOtspm5zbDMCkPF_qkfZzMiFZ6dksmcnqxJyIBvQMG9X9Q--",
                  "StorageClass": "STANDARD",
                  "Key": "JavaFile",
                  "Owner": {
                    "DisplayName": "display-name",
                    "ID": "examplee7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
                  }
                },
                {
                  "Initiator": {
                    "DisplayName": "display-name",
                    "ID": "examplee7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
                  },
                  "Initiated": "2014-05-01T05:41:27.000Z",
                  "UploadId": "examplelo91lv1iwvWpvCiJWugw2xXLPAD7Z8cJyX9.WiIRgNrdG6Ldsn.9FtS63TCl1Uf5faTB.1U5Ckcbmdw--",
                  "StorageClass": "STANDARD",
                  "Key": "JavaFile",
                  "Owner": {
                    "DisplayName": "display-name",
                    "ID": "examplee7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
                  }
                }
              ]
            }
          }
        ],
        "smithy.api#http": {
          "method": "GET",
          "uri": "/{Bucket}?uploads",
          "code": 200
        }
      }
    },
    "com.amazonaws.s3#ListMultipartUploadsOutput": {
      "type": "structure",
      "members": {
        "Bucket": {
          "target": "com.amazonaws.s3#BucketName",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#bucket"
            }
          }
        },
        "KeyMarker": {
          "target": "com.amazonaws.s3#KeyMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#keymarker"
            }
          }
        },
        "UploadIdMarker": {
          "target": "com.amazonaws.s3#UploadIdMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#uploadidmarker"
            }
          }
        },
        "NextKeyMarker": {
          "target": "com.amazonaws.s3#NextKeyMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#nextkeymarker"
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
        "NextUploadIdMarker": {
          "target": "com.amazonaws.s3#NextUploadIdMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#nextuploadidmarker"
            }
          }
        },
        "MaxUploads": {
          "target": "com.amazonaws.s3#MaxUploads",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#maxuploads"
            }
          }
        },
        "IsTruncated": {
          "target": "com.amazonaws.s3#IsTruncated",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#istruncated"
            }
          }
        },
        "Uploads": {
          "target": "com.amazonaws.s3#MultipartUploadList",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#upload"
            },
            "smithy.api#xmlFlattened": {},
            "smithy.api#xmlName": "Upload"
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
        "smithy.api#xmlName": "ListMultipartUploadsResult"
      }
    },
    "com.amazonaws.s3#ListMultipartUploadsRequest": {
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
            "smithy.api#httpQuery": "encoding-type",
            "smithy.api#documentation": {
              "$ref": "#query-encoding-type"
            }
          }
        },
        "KeyMarker": {
          "target": "com.amazonaws.s3#KeyMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-key-marker"
            },
            "smithy.api#httpQuery": "key-marker"
          }
        },
        "MaxUploads": {
          "target": "com.amazonaws.s3#MaxUploads",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-max-uploads"
            },
            "smithy.api#httpQuery": "max-uploads"
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
        "UploadIdMarker": {
          "target": "com.amazonaws.s3#UploadIdMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-upload-id-marker"
            },
            "smithy.api#httpQuery": "upload-id-marker"
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
        "RequestPayer": {
          "target": "com.amazonaws.s3#RequestPayer",
          "traits": {
            "smithy.api#httpHeader": "x-amz-request-payer",
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-request-payer"
            }
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
