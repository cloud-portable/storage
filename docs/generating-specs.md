# Generating Portable Storage Specs

_How we get to a vendor-neutral, machine-readable object storage spec subset._

**The gist**

- Filter the [AWS S3 smithy spec] down to the operations defined in [tier-1.yml](../tier-1.yml).
- Write new API docs to be vendor neutral and coherent for that subset.
- Merge the new docs into the smithy spec.
- Translate the smithy spec into an openAPI spec for general consumption.

Once the baseline spec is agreed: 

- We `test` tools and services against it.
- We `diff` other api specs against our baseline.

**The layout**

### `storage` repo

* **`tier-1.yaml`**: Define the allowed operations, parameters, and headers.
* **`operations/*.md`**: Documentation for each operation.
* **Spec Outputs**: The generated `tier-1.smithy.json` and `tier-1.openapi.yaml`.

### `storage-spec-cli` repo

* **`bootstrap`**: Extracts a subset of shapes and operations from a source AWS Smithy model.
* **`compile`**: Combines smithy AST and markdown docs to create new smithy and openapi spec.
* **`test`**: Record a compatibilty test run for a service.
* **`diff`**: Compare smithy ASTs.

## Getting started

Ensure both the `storage` and `storage-spec-cli` repositories are cloned as siblings. Link the CLI globally to run it locally:

```bash
cd ../storage-spec-cli
npm install
npm run build
npm link
cd ../storage
```

The `storage-spec` binary is now available globally.

### Workflow

1. **Bootstrap the smithy AST** - Download and filter based on `tier-1.yaml`. Extracts initial Markdown doc templates under `operations/`:
   ```bash
   storage-spec bootstrap --source ./tier-1.yaml --output ./tier-1.smithy.bare.json --extract-docs ./operations
   ```
2. **Refine the docs** - Edit the extracted markdown files in `operations/` to improve the descriptions.
3. **Compile the Specs** - Merge the bare AST and the md docs into the final Smithy AST and OpenAPI artifacts:
   ```bash
   storage-spec compile --input ./tier-1.smithy.bare.json --docs ./operations --output-smithy ./tier-1.smithy.json --output-openapi ./tier-1.openapi.yaml
   ```
4. **Generate HTML Documentation**
   Builds the OpenAPI YAML into a client-facing HTML document:
   ```bash
   npx @redocly/cli build-docs ./tier-1.openapi.yaml -o ./docs/index.html -t ./docs/template.hbs
   ```

## Specification Formats

### Smithy AST JSON (`tier-1.smithy.json`)

* Serves as the primary source of truth.
* **Rationale**: S3 routes multiple distinct operations (e.g., `CopyObject` and `PutObject`) to identical HTTP paths and methods using headers and query parameters, and relies on AWS SigV4 traits. Smithy models these protocol-specific traits and signatures natively.
* **Usage**: Client routing engines, service proxies, and compliance validators.

### OpenAPI 3.1 YAML (`tier-1.openapi.yaml`)

* Developer-facing REST representation.
* **Rationale**: Exposes S3 features to standard API tooling ecosystems (Redocly, Postman, SDK generators).
* **Limitations**: The S3 API does things that cannot be described in openAPI. See: [openapi-issues.md](./openapi-issues.md)


[AWS S3 smithy spec]: https://github.com/aws/api-models-aws/tree/main/models/s3/service/2006-03-01