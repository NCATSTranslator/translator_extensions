### Examples that should pass validation

#### Example 1: Valid `x-bte` Extension
This example correctly defines both `x-bte-kgs-operations` and `x-bte-response-mapping` properties, adhering to the schema requirements:

```yaml
components:
  x-bte-kgs-operations:
    myOperation:
      - supportBatch: true
        useTemplating: true
        inputs:
          - id: CHEBI
            semantic: SmallMolecule
        requestBody:
          body: "example request body"
        outputs:
          - id: RHEA
            semantic: MolecularActivity
        parameters:
          fields: "exampleField"
          size: 1000
        predicate: "participates_in"
        source: "infores:rhea"
        response_mapping:
          source_url: "http://example.com/response"
  x-bte-response-mapping:
    exampleMapping:
      source_url: "http://example.com"
      edge-attributes: "example attributes"
      ref_input: "example input"
      ref_output: "example output"
```

### Examples with ERRORS that should fail validation

#### Example 1: Missing `supportBatch` Property
The `supportBatch` property is required when defining operations within `x-bte-kgs-operations`. This example will fail validation because `supportBatch` is missing:

```yaml
components:
  x-bte-kgs-operations:
    myOperation:
      - useTemplating: true
        inputs:
          - id: CHEBI
            semantic: SmallMolecule
        requestBody:
          body: "example request body"
        outputs:
          - id: RHEA
            semantic: MolecularActivity
        parameters:
          fields: "exampleField"
          size: 1000
        predicate: "participates_in"
        source: "infores:rhea"
        response_mapping:
          source_url: "http://example.com/response"
```

#### Example 2: `team` Field is Not an Array
The `team` field should be an array, even if it contains only one element. This example will fail because `team` is provided as a string:

```yaml
components:
  x-bte-kgs-operations:
    myOperation:
      - supportBatch: true
        useTemplating: true
        inputs:
          - id: CHEBI
            semantic: SmallMolecule
        requestBody:
          body: "example request body"
        outputs:
          - id: RHEA
            semantic: MolecularActivity
        parameters:
          fields: "exampleField"
          size: 1000
        predicate: "participates_in"
        source: "infores:rhea"
        response_mapping:
          source_url: "http://example.com/response"
info:
  x-translator:
    component: KP
    team: Service Provider
```

#### Example 3: Incorrect CURIE Format in `source`
The `source` field must follow the `infores:` CURIE format. The following example will fail because the `source` field does not include the required `infores:` prefix:

```yaml
components:
  x-bte-kgs-operations:
    myOperation:
      - supportBatch: true
        useTemplating: true
        inputs:
          - id: CHEBI
            semantic: SmallMolecule
        requestBody:
          body: "example request body"
        outputs:
          - id: RHEA
            semantic: MolecularActivity
        parameters:
          fields: "exampleField"
          size: 1000
        predicate: "participates_in"
        source: "rhea"  # This should be prefixed with "infores:"
        response_mapping:
          source_url: "http://example.com/response"
```