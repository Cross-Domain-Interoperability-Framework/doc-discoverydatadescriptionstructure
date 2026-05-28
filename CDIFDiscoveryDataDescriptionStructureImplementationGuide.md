# CDIF Discovery + Data Description + Data Structure Profile — Implementation Guide

## 1. Purpose and scope

This composite **application profile** describes a dataset at the deepest CDIF level: it makes the dataset findable (Discovery), documents the meaning and physical representation of its variables (Data Description), and captures the full structural model that relates those variables to each other and to the records of the data file (Data Structure).

It extends the Data Description profile with the DDI-CDI structural model:

- **Data structures** — `cdi:DataStructure` and its flavours `cdi:DimensionalDataStructure`, `cdi:LongDataStructure`, and `cdi:WideDataStructure`.
- **Components** — the typed roles a variable plays in a structure: `IdentifierComponent`, `MeasureComponent`, `AttributeComponent`, `DimensionComponent`, `VariableValueComponent`, `VariableDescriptorComponent`.
- **Represented variables and value domains** — `cdi:RepresentedVariable`, `cdi:ValueAndConceptDescription`.
- **Keys** — `PrimaryKey` and `ForeignKey`.

Distribution items are expected to carry **`cdi:isStructuredBy`** pointing at a Data Structure node.

This profile composes the published profile modules: [profile-core](https://github.com/Cross-Domain-Interoperability-Framework/profile-core), [profile-discovery](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery), [profile-datadescription](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription), and [profile-datastructure](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure). Consult those modules for property-by-property documentation of each layer.

## 2. Conformance

A conforming catalog record declares all four profile identifiers on `dcterms:conformsTo`:

```json
"schema:subjectOf": {
  "@type": ["schema:CreativeWork", "dcat:CatalogRecord"],
  "dcterms:conformsTo": [
    "https://w3id.org/cdif/core/1.0",
    "https://w3id.org/cdif/discovery/1.0",
    "https://w3id.org/cdif/data_description/1.0",
    "https://w3id.org/cdif/data_structure/1.0"
  ]
}
```

Each layer builds on the one below it, so all four identifiers are required.

## 3. The data-structure layer

A dataset that conforms to this profile describes how its variables are organized:

1. **`schema:variableMeasured`** lists the dataset's variables as `cdi:InstanceVariable` nodes (carried up from the Data Description layer), each with its physical data type, definition, value domain, and represented concept.
2. **A Data Structure node** (`cdi:DataStructure` or a flavour) declares the components. Each component points at the instance variable that fills it and assigns it a structural role:
   - **Identifier** components form the logical key of a record;
   - **Measure** components hold the observed/measured values;
   - **Attribute** components qualify measures (e.g. units, status flags);
   - **Dimension** components index dimensional (cube) data.
3. **Keys** — `cdif:hasPrimaryKey` (and foreign keys) declare the instance variables that uniquely identify, or reference, a record.
4. **`cdi:isStructuredBy`** on each `schema:distribution` item links the physical file to the Data Structure that describes it.

Choose the data-structure flavour that matches the physical layout: `Wide` (one row per observation unit, one column per variable), `Long` (key–value rows), or `Dimensional` (a data cube indexed by dimensions).

## 4. Validation

- **JSON Schema** — `CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json` (Draft 2020-12).
- **SHACL** — `discoveryDataDescriptionStructureRules.shacl`, a self-contained shapes graph merged from all 32 composing building blocks plus the profile-level shapes.

```bash
python FrameAndValidate.py examples/exampleCDIFDataStructureComplete.json --validate \
  --schema CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json \
  --frame CDIFDiscoveryDataDescriptionStructure-frame.jsonld
```

`FrameAndValidate.py` array-wraps multi-valued properties before validating. Note that `cdif:statistics` and `cdif:isComposedOf` must appear in its `ARRAY_PROPERTIES` list. Validation is **open-world**: properties beyond the profile are permitted.

## 5. Examples

- `examples/exampleCDIFDataStructureMinimal.json` — the smallest conforming record.
- `examples/exampleCDIFDataStructureComplete.json` — a fully populated record exercising components, keys, value domains, and statistics.

## 6. Provenance of the artifacts

Generated from the canonical [metadataBuildingBlocks](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks) register:

- `CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json` ← `tools/resolve_schema.py DiscoveryDataDescriptionStructure`
- `discoveryDataDescriptionStructureRules.shacl` ← `tools/validate_shacl.py DiscoveryDataDescriptionStructure --emit-shapes`

Source profile directory: `_sources/profiles/cdifCompositeProfile/DiscoveryDataDescriptionStructure/`. Re-sync whenever the source register changes.
