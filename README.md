# CDIF Discovery + Data Description + Data Structure (application profile)

This repository holds the published artifacts for the **CDIF Discovery / Data Description / Data Structure** composite application profile — the `DiscoveryDataDescriptionStructure` profile from the [metadataBuildingBlocks](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks) source register.

> **Scope.** This composite extends the Data Description profile with the full DDI-CDI structural model: data structures (`DataStructure` / `Dimensional` / `Long` / `Wide`), component subclasses (`Identifier`, `Measure`, `Attribute`, `Dimension`, `VariableValue`, `VariableDescriptor`), represented variables, value domains, and keys. Distribution items are expected to carry `cdi:isStructuredBy` pointing at a Data Structure node. It composes the profile modules published in [profile-core](https://github.com/Cross-Domain-Interoperability-Framework/profile-core), [profile-discovery](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery), [profile-datadescription](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription), and [profile-datastructure](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure).

## Specification

- **[CDIFDiscoveryDataDescriptionStructureImplementationGuide.md](CDIFDiscoveryDataDescriptionStructureImplementationGuide.md)** — Documentation for the composite profile.
- **[CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json](CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json)** — JSON Schema (Draft 2020-12), generated from the source register with `tools/resolve_schema.py`.
- **[discoveryDataDescriptionStructureRules.shacl](discoveryDataDescriptionStructureRules.shacl)** — Self-contained SHACL shapes, merged from every composing building block plus the profile-level shapes.

## Conformance

A conforming catalog record declares, in its `dcterms:conformsTo`:

- `https://w3id.org/cdif/core/1.1`
- `https://w3id.org/cdif/discovery/1.1`
- `https://w3id.org/cdif/data_description/1.1`
- `https://w3id.org/cdif/data_structure/1.1`

## Examples

```bash
python FrameAndValidate.py examples/exampleCDIFDataStructureComplete.json --validate \
  --schema CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json \
  --frame CDIFDiscoveryDataDescriptionStructure-frame.jsonld
```

`FrameAndValidate.py` frames the document, array-wraps the multi-valued properties (variables, statistics, structure components, keys), then validates against the JSON Schema. Validation is open-world: unknown properties pass.

## Synced from metadataBuildingBlocks

Generated artifacts; re-sync manually when the source register changes:

| file | source |
|---|---|
| `CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json` | `python tools/resolve_schema.py DiscoveryDataDescriptionStructure -o <file>` |
| `discoveryDataDescriptionStructureRules.shacl` | `python tools/validate_shacl.py DiscoveryDataDescriptionStructure --emit-shapes <file>` |

Source profile: `_sources/profiles/cdifCompositeProfile/DiscoveryDataDescriptionStructure/`.

## Changelog — reviewRevision202606 (updates since branched from `main`)

This release-review branch has diverged from `main` with the following updates,
synced from the CDIF **metadataBuildingBlocks** source (see
`git log main..reviewRevision202606` for the full per-commit history):

- **Populated from metadataBuildingBlocks** — `*StructuredSchema.json`, merged SHACL,
  JSON-LD frame, examples, and the normative `FrameAndValidate.py` generated from the
  building-block source; `Examples/` renamed to `examples/`.
- **CDIF v1.1** — profile conformance URIs migrated `/1.0` → `/1.1`.
- **License** standardized on CC-BY-4.0.
- **`@id`-reference tightening** — bare `{@id}` reference slots sealed
  (`additionalProperties: false` + `required: ['@id']`); a canonical `objectReference`
  building block introduced as the strict node reference.
- **`prov:used` wrapper reconciliation** — the base `generatedBy.prov:used` accepts
  role-keyed wrappers (`schema:instrument` / `bios:computationalTool` / `prov:reagent`)
  alongside string / `{@id}` / inline `prov:Entity`; profiles pin a wrapper's shape via
  a constraint-only `if/then` (never a narrowed `anyOf`).
- **`skos:notation` → single string** at concept level (consistent with the codelist
  single-notation design).
- **`FrameAndValidate.py`** (normative, drift-checked against
  `Cross-Domain-Interoperability-Framework/validation`) — two-frame root-`@type`
  selection, context-aware `schema:about`, `--conformance` detection, `cdif:`-`@id`
  re-expansion, and (2026-08) reference-collapse on all document types + blank-node
  dedupe + agent `schema:identifier` unwrap, so `@embed:@always`-framed documents
  validate against the tightened schemas.
- **Examples** conformed to the tightened schemas throughout (PrimaryKey →
  `cdi:ComponentPosition`, reference slots → `{@id}`, CVE `hasIntendedDataType` →
  string, `skos:notation` → string, `schema:additionalType` URI → `{@id}`).


## Development branch

Active work for the 2026-06 review revision is on the `reviewRevision202606` branch. `main` reflects the prior release state. New changes should target the review branch; it is merged to main on release.


## License

This work is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).
