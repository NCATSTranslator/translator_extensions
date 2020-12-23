# translator_extensions
The Translator-specific metadata extensions used in the SmartAPI registry

## x-translator
#### Status
This extension's schema has been written, and registry files currently contain this extension.

#### Description
This extension is inside the OpenAPI info object and contains basic API-level metadata. Currently...

- there are two (required) properties: component (KP, ARA, ARS, or Utility) and team (one or more Translator team names)
- there are rules:  
  - if the API is tagged "translator" in the OpenAPI top-level tags, then the x-translator extension must be present (in OpenAPI info)
  - if x-translator is present (in OpenAPI info), then the API must be tagged "translator" in the OpenAPI top-level tags  

#### Examples  
An example of a valid x-translator extension in YAML is:  
```
  tags:
  - name: translator    ## required translator tag
  - name: biothings
  info:
    x-translator:      ## required x-translator extension
      component: KP
      team:
      - "Service Provider"
```
This same example in JSON and other examples of valid and invalid instances (in JSON) are in [this file](https://github.com/NCATSTranslator/translator_extensions/blob/main/x-translator/smartapi_x-translator_examples.txt). 

## x-trapi
This extension's schema has not been written / registry files do not contain this extension. It will likely contain API- and operation-level metadata that TRAPI expects.