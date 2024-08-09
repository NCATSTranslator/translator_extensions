## Examples that should pass validation

This should work:

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-translator:
    component: KP
    team:
      - Service Provider
    biolink-version: 2.2.16
    infores: "infores:biolink-api"
```

## Examples with ERRORS that should fail validation

Error: the `team` value cannot be a string (it must be an array, even if it’s a one-element array).

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-translator:
    component: KP
    team: Service Provider
```

Within `info.x-translator`, `component` and `team` both need to be present. Below, the property `component` is misspelled.

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-translator:
    compnent: KP
    team:
      - Service Provider
```

The `team` field cannot be an empty array.

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-translator:
    component: KP
    team: []
```

The `infores` field's value must be a full CURIE with namespace prefix `infores:`. Below, the field's value is missing the prefix.

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-translator:
    component: KP
    team:
      - Service Provider
    biolink-version: 2.2.16
    infores: biolink-api
```
---