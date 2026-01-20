# Cloud Portable Storage

_S3-compatible APIs have become the de-facto standard for digital storage._

This repo defines a Request for Comments (RFC) to establish a minimal, strictly-defined subset of the S3 API that guarantees interoperability. If your storage implements this spec, applications can switch providers with zero code changes. This is an open specification. The spec itself is freely available and implementable by anyone—proprietary or open source providers.

Write code once. Run it on AWS, Cloudflare, or [Garage](https://garagehq.deuxfleurs.fr/) on your own server. Any bucket will do.

## Why does this matter?

"In a world where digital infrastructure is often foreign-owned and opaque, wanting more control is natural. 
But sovereignty, if defined as owning every layer of a technological stack, can quickly become counterproductive. It leads to balkanized systems, limited interoperability and a retrenchment from global cooperation. 
The incentives for creative development disappear. Competition and innovation subside."
From [The Services Gap](https://lisboncouncil.net/the-service-gap-europe-international-digital-strategy-2025/)

The underlying thinking of this project is to create a path to agency and sovereignty through portability.  Open specifications, that recognize as opposed to displace, existing defacto standards can further commodify mature technologies—starting and this increase optionality. It is unrealistic and would be counterproductive to have every country build its own digital infrastructure within the next decade. Success means making provider switching simple and creating a liquid market where domestic, international and inhouse providers can compete on service, resiliance and legal reliability, not lock-in. 

If a sufficiently clear specification exists, governments can leverage it to become market shapers: using procurement power to demand interoperability, recognizing de facto standards, and coordinating across borders. A small number of governments, operating around a clear set of specificiations can shape a market. This is how the railway market was shaped - governments chose what is now referred to the "standard gauge". We need to once again choose some standards for the core building blocks of a digital society.

To be clear, the goal isn't to attack or remove hyperscalers it is to create a fluid market through specifications that make agency - through portability and choice - for all countries, not just the wealthy. Countries can use these specificiations to use foreign companies, domestic providers or even build their operate their own services, in a manner that does minimizes vendor or technology lock-in.

Read more on the thinking about this [here](https://www.techpolicy.press/the-path-to-a-sovereign-tech-stack-is-via-a-commodified-tech-stack/)

## Scope

Compliance is divided into 3 tiers. To be considered "S3 Compatible", a service must fully implement Tier 1.

### Tier 1: Core

The minimum requirements for a system to function as an object store. Enables basic CRUD operations. Most static site generators and simple backup tools only need this. 

### Tier 2: Reliability

Essential for production workloads handling files larger than 100MB. Ensures reliability over unstable networks via multipart uploads.

### Tier 3: Advanced

Features required for complex application architectures, including presigned URLs for client-side uploads and custom metadata handling.

## Implementors

The following tools and providers already implement portable S3-compatible storage interfaces

### Open Source Tools

| Tool | Tier 1 (Core) | Tier 2 (Reliability) | Tier 3 (Advanced) | Documentation |
| :--- | :---: | :---: | :---: | :--- |
| **Ceph (RGW)** | ✅ | ✅ | ✅ | [Ceph S3 API](https://docs.ceph.com/en/latest/radosgw/s3/) |
| **Garage** | ✅ | ✅ | ⚠️ | [Compatibility Matrix](https://garagehq.deuxfleurs.fr/documentation/reference-manual/s3-compatibility/) |
| **MinIO** | ✅ | ✅ | ✅ | [MinIO S3 API](https://min.io/docs/minio/linux/reference/minio-server/minio-server.html#s3-api-compatibility) |
| **OpenStack Swift** | ✅ | ✅ | ⚠️ | [Swift S3 Compat](https://docs.openstack.org/swift/latest/s3_compat.html) |
| **SeaweedFS** | ✅ | ✅ | ⚠️ | [SeaweedFS S3 API](https://github.com/seaweedfs/seaweedfs/wiki/Amazon-S3-API) |
| **Zenko (CloudServer)** | ✅ | ✅ | ✅ | [CloudServer](https://github.com/scality/cloudserver) |

### Cloud Services

| Provider | Tier 1 (Core) | Tier 2 (Reliability) | Tier 3 (Advanced) | Documentation |
| :--- | :---: | :---: | :---: | :--- |
| **AWS S3** | ✅ | ✅ | ✅ | [API Reference](https://aws.amazon.com/s3/) |
| **Backblaze B2** | ✅ | ✅ | ⚠️ | [S3 Compatible API](https://www.backblaze.com/docs/cloud-storage-s3-compatible-api) |
| **Cloudflare R2** | ✅ | ✅ | ⚠️ | [S3 Compatibility](https://developers.cloudflare.com/r2/api/s3/api/) |
| **DigitalOcean Spaces** | ✅ | ✅ | ⚠️ | [Spaces API](https://docs.digitalocean.com/products/spaces/reference/s3-compatibility/) |
| **Google Cloud Storage** | ✅ | ✅ | ⚠️ | [Interoperability](https://cloud.google.com/storage/docs/interoperability) |
| **IBM Cloud Object Storage** | ✅ | ✅ | ⚠️ | [Compatibility API](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-compatibility-api) |
| **IDrive e2** | ✅ | ✅ | ⚠️ | [Developer Guide](https://www.idrive.com/s3-storage-e2/guides/s3_developer_guide) |
| **Linode (Akamai)** | ✅ | ✅ | ⚠️ | [Object Storage API](https://techdocs.akamai.com/cloud-computing/docs/object-storage-s3-api) |
| **Oracle Cloud (OCI)** | ✅ | ✅ | ⚠️ | [S3 Compatibility API](https://docs.oracle.com/en-us/iaas/Content/Object/Tasks/s3compatibleapi.htm) |
| **Scaleway Object Storage** | ✅ | ✅ | ⚠️ | [API Call List](https://www.scaleway.com/en/docs/object-storage/api-cli/using-api-call-list/) |
| **Tigris** | ✅ | ✅ | ⚠️ | [S3 Compatibility](https://www.tigrisdata.com/docs/s3/) |
| **Wasabi** | ✅ | ✅ | ✅ | [API Support](https://wasabi.com/s3-compatible-cloud-storage/) |

_See something missing? Open a [pull request](https://github.com/cloud-portable/storage/pulls) to add it._

## How decisions are made

This specification is developed through open RFC process. Please open an issue if you have a topic you think should be added to this specfication. 
Conversations will be moderated by @deaves and @olizilla until more admins are added.

## Core Principles

- Recognize and build off defacto standards
- Working sub-optimal systems over idealized perfect systems
- Simplicity and interoperability over incremental additional value

## Get involved

- Review the draft specification in [rfc-storage-tier-1.md](https://github.com/cloud-portable/storage/blob/main/rfc-storage-tier-1.md)
- Share feedback and ask questions via [GitHub issues](https://github.com/cloud-portable/storage/issues)
- Propose changes via [pull requests](https://github.com/cloud-portable/storage/pulls)
- Please follow our [code of conduct](https://github.com/cloud-portable/storage/blob/main/CODE_OF_CONDUCT.md) and be kind
