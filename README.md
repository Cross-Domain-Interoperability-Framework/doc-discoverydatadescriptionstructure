# CDIF Discovery + Data Description + Data Structure (application profile)

This repository holds the published artifacts for the **CDIF Discovery / Data Description / Data Structure** composite application profile — the `DiscoveryDataDescriptionStructure` profile from the [metadataBuildingBlocks](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks) source register.

> **Scope.** This composite extends the Data Description profile with the full DDI-CDI structural model: data structures (`DataStructure` / `Dimensional` / `Long` / `Wide`), component subclasses (`Identifier`, `Measure`, `Attribute`, `Dimension`, `VariableValue`, `VariableDescriptor`), represented variables, value domains, and keys. Distribution items are expected to carry `cdi:isStructuredBy` pointing at a Data Structure node. It composes the profile modules published in [profile-core](https://github.com/Cross-Domain-Interoperability-Framework/profile-core), [profile-discovery](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery), [profile-datadescription](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription), and [profile-datastructure](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure).

## Specification

- **[CDIFDiscoveryDataDescriptionStructureImplementationGuide.md](CDIFDiscoveryDataDescriptionStructureImplementationGuide.md)** — Documentation for the composite profile.
- **[CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json](CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json)** — JSON Schema (Draft 2020-12), generated from the source register with `tools/resolve_schema.py`.
- **[discoveryDataDescriptionStructureRules.shacl](discoveryDataDescriptionStructureRules.shacl)** — Self-contained SHACL shapes, merged from every composing building block plus the profile-level shapes.

## Conformance

A conforming catalog record declares, in its `dcterms:conformsTo`:

- `https://w3id.org/cdif/core/1.0`
- `https://w3id.org/cdif/discovery/1.0`
- `https://w3id.org/cdif/data_description/1.0`
- `https://w3id.org/cdif/data_structure/1.0`

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

## Development branch

Active work for the 2026-06 review revision is on the `reviewRevision202606` branch. `main` reflects the prior release state. New changes should target the review branch; it is merged to main on release.


## License

This work is dedicated to the public domain under [CC0 1.0 Universal](LICENSE).
