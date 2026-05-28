# AGENTS.md — AI Agent Guidance for CDIF Discovery+DataDescription+DataStructure

## Project context

This repository publishes the **CDIF Discovery / Data Description / Data Structure** composite application profile (`DiscoveryDataDescriptionStructure`). It composes the `profile-core`, `profile-discovery`, `profile-datadescription`, and `profile-datastructure` modules and adds the full DDI-CDI structural model (data structures, component subclasses, represented variables, value domains, keys). Distribution items are expected to carry `cdi:isStructuredBy`.

## Key files

- `CDIFDiscoveryDataDescriptionStructureImplementationGuide.md` — profile documentation
- `CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json` — JSON Schema (generated)
- `discoveryDataDescriptionStructureRules.shacl` — merged SHACL shapes (generated)
- `CDIFDiscoveryDataDescriptionStructure-frame.jsonld` — JSON-LD frame used by `FrameAndValidate.py`
- `examples/` — validated JSON-LD examples
- `FrameAndValidate.py` — frame + JSON Schema validation

## Synced files (manual sync from metadataBuildingBlocks)

- `CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json` ← `python tools/resolve_schema.py DiscoveryDataDescriptionStructure -o <file>`
- `discoveryDataDescriptionStructureRules.shacl` ← `python tools/validate_shacl.py DiscoveryDataDescriptionStructure --emit-shapes <file>`

Source profile dir: `metadataBuildingBlocks/_sources/profiles/cdifCompositeProfile/DiscoveryDataDescriptionStructure/`.

## Conventions that bite

- `cdif:statistics` is an **array** — it must be in `FrameAndValidate.py`'s `ARRAY_PROPERTIES` (it is). When syncing `FrameAndValidate.py` from another repo, re-check that structure array props (`cdif:statistics`, `cdif:isComposedOf`) are present.
- `cdif:physicalDataType` is dual-context: array on a `cdi:InstanceVariable`, string on a physical mapping.
- `schema:contentSize` is a **string**.
- Catalog record `dcterms:conformsTo` must include `core/1.0`, `discovery/1.0`, `data_description/1.0`, and `data_structure/1.0`.
- Never strip unknown properties — validation is open-world.

## Validation

```bash
python FrameAndValidate.py examples/<file>.json --validate \
  --schema CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json \
  --frame CDIFDiscoveryDataDescriptionStructure-frame.jsonld
```
