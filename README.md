# Cloud Portable Storage

## What is this?

A Request for Comments (RFC) to establish a minimal, strictly-defined subset of the S3 API that guarantees interoperability. If your storage implements this spec, applications can switch providers with zero code changes.
This is an open specification, not open source software. The spec itself is freely available and implementable by anyone—proprietary or open source providers.

Write code once. Run it on AWS, Cloudflare, MinIO, or your own server. Any bucket will do.

## Why does this matter?

"In a world where digital infrastructure is often foreign-owned and opaque, wanting more control is natural. 
But sovereignty, if defined as owning every layer of a technological stack, can quickly become counterproductive. It leads to balkanized systems, limited interoperability and a retrenchment from global cooperation. 
The incentives for creative development disappear. Competition and innovation subside."
From: https://lisboncouncil.net/the-service-gap-europe-international-digital-strategy-2025/

The underlying thinking of this project is to create a path to agency and sovergnity through portability.  Open specifications, that recognize as opposed to displace, existing defacto standards can further commodify mature technologies—starting and this increase optionality. It is unrealistic and would be counterproductive to have every country build its own digital infrastructure within the next decade. Success means making provider switching simple and creating a liquid market where domestic, international and inhouse providers can compete on service, resiliance and legal reliability, not lock-in. 

If a sufficiently clear specfication exists, governments can leverage it to become market shapers: using procurement power to demand interoperability, recognizing de facto standards, and coordinating across borders. A small number of governments, operating around a clear set of specficiations can shape a market. This is how the railway market was shaped - governments chose what is now referred to the "standard gauge". We need to once again choose some standards for the core building blocks of a digital society.

To be clear, the goal isn't to attack or remove hyperscalers it is to create a fluid market through specifications that make agency - through portiability and choice - for all countries, not just the wealthy. Countries can use these specficiations to use foreign companies, domestic providers or even build their operate their own services, in a manner that does minimizes vendor or technology lock-in.

Read more on the thinking about this here: https://www.techpolicy.press/the-path-to-a-sovereign-tech-stack-is-via-a-commodified-tech-stack/

## Scope

This specification covers:
- Core bucket operations (create, delete, list)
- Object operations (put, get, delete, copy)
- Basic metadata and ACLs
- [specific list]

Explicitly excluded:
- Versioning
- Advanced lifecycle policies
- [specific list]

## How decisions are made

This specification is developed through open RFC process. [Describe: who reviews? how do things get accepted? is there a steering committee?]

## Get involved

- Review the draft specification in `/spec`
- Submit feedback via GitHub issues
- Propose changes via pull requests
- Join the discussion: [link]
