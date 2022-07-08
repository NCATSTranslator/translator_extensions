# translator_extensions

The Translator-specific metadata extensions used in the SmartAPI registry

## x-translator

#### Status

This extension's schema has been written, and registry files currently contain this extension. It is closely related to the [TRAPI standard](https://github.com/NCATSTranslator/ReasonerAPI).

#### Description

This extension is inside the OpenAPI info object and contains basic API-level metadata. Currently, there are two required properties: component (KP, ARA, ARS, or Utility) and team (one or more Translator team names). 

Current properties include:

- **infores:** Information Resource CURIE identifying the Translator Resource (see the [current inventory of Infores CURIEs](https://docs.google.com/spreadsheets/d/1Ak1hRqlTLr1qa-7O0s5bqeTHukj9gSLQML1-lg6xIHM) and general information about them [here](https://docs.google.com/document/d/177sOmjTueIK4XKJ0GjxsARg909CaU71tReIehAp5DDo/edit#bookmark=id.8sdy3vk2umkd)).
- **component:** One of  "KP", "ARA", "ARS", "Utility"
- **team**: List of contributing Translator teams (see [schema file](https://github.com/NCATSTranslator/translator_extensions/blob/main/x-translator/smartapi_x-translator_schema.json) for the full list)
- **biolink-version:**  One SemVer-formatted version number (e.g. '1.8.2', '2.2.16', etc.). This is the [release](https://github.com/biolink/biolink-model/releases) of the [Biolink Model](https://github.com/biolink/biolink-model) that to which the Translator Resource is currently compliant."

Associated rules:

- if the API is tagged "translator" in the OpenAPI top-level tags, then the x-translator extension must be present (in OpenAPI info)
- if x-translator is present (in OpenAPI info), then the API must be tagged "translator" in the OpenAPI top-level tags  

#### Examples  

 An example of a valid `x-translator` extension in YAML is: 

```
  tags:
  - name: translator    ## required translator tag
  - name: biothings
  info:
    x-translator:      ## required x-translator extension
      infores: "infores:biolink-api"
      component: KP
      team:
      - "Service Provider"
      biolink-version: "2.2.16"
```

Other examples of valid and invalid `x-translator` annotations (in JSON) are in [this example file](https://github.com/NCATSTranslator/translator_extensions/blob/main/x-translator/smartapi_x-translator_examples.txt). 

## x-trapi

#### Status

This extension's schema has been started, and registry files currently contain this extension. It is closely related to the [TRAPI standard](https://github.com/NCATSTranslator/ReasonerAPI).

#### Description

This extension is inside the OpenAPI info object and contains basic API-level metadata. Currently, there are 2 required properties (`version` and `operations`).

Current properties include:

- **version:** States the TRAPI standard version, as a SemVer formatted number (e.g. '1.1', '1.2', '1.3'), indicating that the Translator Resource is compliant with / built to this version.
- **asyncquery:** Boolean. true if this service has TRAPI-compliant asyncquery endpoints. Otherwise, false. 
- **operations:** List of implemented operations. Use the IDs of the operations in this [standard](http://standards.ncats.io/operation.json).
- **batch_size_limit:** Maximum number of CURIEs allowed in _any_ 'ids', 'categories', or 'predicates' list. -1 indicates no limit. See ImplementationRules.md in the TRAPI standard github repo for details. 
- **rate_limit**: Maximum number of requests allowed per _minute_ from each client. -1 indicates no limit.
- **test_data_location:** URL that resolves to [SRI-Testing-harness](https://github.com/TranslatorSRI/SRI_testing) -compliant test data in some public internet location (e.g. in the API implementation source code repository).

#### Examples

An example of a valid `x-trapi` extension in YAML is:  

```
  tags:
  - name: translator    ## required translator tag
  - name: biothings
  info:
    x-trapi:      ## required x-translator extension
      version: "1.2.0"
      operations:
      - lookup
      test_data_location: "https://github.com/biothings/biothings.api/blob/master/test_data"  # just a fictional example link
```

Other examples of valid and invalid instances (in JSON) are in [this example file](https://github.com/NCATSTranslator/translator_extensions/blob/main/x-trapi/smartapi_x-trapi_examples.txt).
