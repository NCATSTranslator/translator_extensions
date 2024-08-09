### Examples that should pass validation

#### Example 1: Valid `x-bte` Extension
This example correctly defines both `x-bte-kgs-operations` and `x-bte-response-mapping` properties, adhering to the schema requirements:

```yaml
components:
  x-bte-kgs-operations:
    ma_has_subclass_ma:
      - supportBatch: false
        useTemplating: true
        inputs:
          - id: GO
            semantic: MolecularActivity
        parameters:
          goid: "{{ queryInputs | replPrefix('GO') }}"
        outputs:
          - id: GO
            semantic: MolecularActivity
        predicate: "superclass_of"
        source: "infores:go"
        knowledge_level: "knowledge_assertion"
        agent_type: "manual_agent"
        response_mapping:
          "$ref": "#/components/x-bte-response-mapping/ma_has_subclass_ma"
        testExamples:
          - qInput: "GO:0000082"
            oneOutput: "GO:2000045"
  x-bte-response-mapping:
    ma_has_subclass_ma:
      GO: "results.children.id"
```

### Examples with ERRORS that should fail validation

#### Example 1: Missing `supportBatch` Property
The `supportBatch` property is required when defining operations within `x-bte-kgs-operations`. This example will fail validation because `supportBatch` is missing:

```yaml
components:
  x-bte-kgs-operations:
    ma_has_subclass_ma:
      - useTemplating: true
        inputs:
          - id: GO
            semantic: MolecularActivity
        parameters:
          goid: "{{ queryInputs | replPrefix('GO') }}"
        outputs:
          - id: GO
            semantic: MolecularActivity
        predicate: "superclass_of"
        source: "infores:go"
        knowledge_level: "knowledge_assertion"
        agent_type: "manual_agent"
        response_mapping:
          "$ref": "#/components/x-bte-response-mapping/ma_has_subclass_ma"
        testExamples:
          - qInput: "GO:0000082"
            oneOutput: "GO:2000045"
```

#### Example 2: `team` Field is Not an Array
The `team` field should be an array, even if it contains only one element. This example will fail because `team` is provided as a string:

```yaml
info:
  x-translator:
    component: KP
    team: Service Provider  # This should be an array
components:
  x-bte-kgs-operations:
    ma_has_subclass_ma:
      - supportBatch: false
        useTemplating: true
        inputs:
          - id: GO
            semantic: MolecularActivity
        parameters:
          goid: "{{ queryInputs | replPrefix('GO') }}"
        outputs:
          - id: GO
            semantic: MolecularActivity
        predicate: "superclass_of"
        source: "infores:go"
        knowledge_level: "knowledge_assertion"
        agent_type: "manual_agent"
        response_mapping:
          "$ref": "#/components/x-bte-response-mapping/ma_has_subclass_ma"
        testExamples:
          - qInput: "GO:0000082"
            oneOutput: "GO:2000045"
```

#### Example 3: Incorrect CURIE Format in `source`
The `source` field must follow the `infores:` CURIE format. The following example will fail because the `source` field does not include the required `infores:` prefix:

```yaml
components:
  x-bte-kgs-operations:
    ma_has_subclass_ma:
      - supportBatch: false
        useTemplating: true
        inputs:
          - id: GO
            semantic: MolecularActivity
        parameters:
          goid: "{{ queryInputs | replPrefix('GO') }}"
        outputs:
          - id: GO
            semantic: MolecularActivity
        predicate: "superclass_of"
        source: "go"  # This should be prefixed with "infores:"
        knowledge_level: "knowledge_assertion"
        agent_type: "manual_agent"
        response_mapping:
          "$ref": "#/components/x-bte-response-mapping/ma_has_subclass_ma"
        testExamples:
          - qInput: "GO:0000082"
            oneOutput: "GO:2000045"
```

---