### Examples that should pass validation

#### Example 1: Valid `x-bte` Extension
This example correctly defines both `x-bte-kgs-operations` and `x-bte-response-mapping` properties, adhering to the schema requirements:  

```yaml
components:
  x-bte-kgs-operations:
    disease-intervention:
      - supportBatch: true
        useTemplating: true
        inputs:
          - id: UMLS
            semantic: Disease
        requestBody:
          body:
            q: "{{ queryInputs }}"  
            scopes: "object.UMLS"  
        outputs:
          - id: UMLS
            semantic: Treatment
        parameters:
          fields: "subject.UMLS, association.edge_attributes, source.edge_sources"
          size: 1000
        predicate: "related_to"
        response_mapping:
          "$ref": "#/components/x-bte-response-mapping/treatment"
  x-bte-response-mapping:
    treatment:
      UMLS: "subject.UMLS"
      edge-attributes: "association.edge_attributes"
      trapi_sources: "source.edge_sources"
``` 

### Examples with ERRORS that should fail validation
   
#### Example 1: Missing `supportBatch` Property
The `supportBatch` property is required when defining operations within `x-bte-kgs-operations`. This example will fail validation because `supportBatch` is missing:
  
```yaml
components:
  x-bte-kgs-operations:
    disease-intervention:
        useTemplating: true
        inputs:
          - id: UMLS
            semantic: Disease
        requestBody:
          body:
            q: "{{ queryInputs }}"  
            scopes: "object.UMLS"  
        outputs:
          - id: UMLS
            semantic: Treatment
        parameters:
          fields: "subject.UMLS, association.edge_attributes, source.edge_sources"
          size: 1000
        predicate: "related_to"
        response_mapping:
          "$ref": "#/components/x-bte-response-mapping/treatment"
  x-bte-response-mapping:
    treatment:
      UMLS: "subject.UMLS"
      edge-attributes: "association.edge_attributes"
      trapi_sources: "source.edge_sources"
```
#### Example 2: Incorrect CURIE Format in `source`
The `source` field must follow the `infores:` CURIE format. The following example will fail because the `source` field does not include the required `infores:` prefix:

```yaml
components:
  x-bte-kgs-operations:
    pathway-gene:
      - supportBatch: true
        useTemplating: true
        inputs:
          - id: ncats.bioplanet
            semantic: Pathway
        requestBody:
          body: 
            q: "{{ queryInputs }}"
            scopes: "object.PATHWAY_ID"
        outputs:
          - id: NCBIGene
            semantic: Gene
        parameters:
          fields: "subject.GENE_ID, object.PATHWAY_NAME"
          size: 1000
        predicate: "has_participant"
        source: "bioplanet"  # This should be prefixed with "infores:"
        knowledge_level: "knowledge_assertion"
        agent_type: "manual_agent"
        response_mapping:
          "$ref": "#/components/x-bte-response-mapping/gene"
        testExamples:
          - qInput: "ncats.bioplanet:bioplanet_869"
            oneOutput: "NCBIGene:9672"
```

---