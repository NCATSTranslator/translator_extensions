## Examples that should pass validation

This should work.

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

This also should work, because both the `x-translator` extension and the `translator` tag aren't present.

```yaml
tags:
  - name: biothings
```


## Examples with ERRORS that should fail validation

Error: the `team` value cannot be a string (can be a one-element array).

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

The `component` value and `team` value must be in their respective enums. Below, both have invalid values.

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-translator:
    component: thing
    team:
      - outsideForNow
```

The `info.x-translator` section is present but the `translator` tag is missing or misspelled.

```yaml
tags:
  - name: transltor
  - name: biothings
info:
  x-translator:
    component: KP
    team:
      - Service Provider
```

Below, the `info.x-translator` field is misspelled.

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-transltor:
    component: KP
    team:
      - Service Provider
```

The `team` field cannot have repeated values.

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-translator:
    component: KP
    team:
      - Service Provider
      - Service Provider
```

The `team ` field cannot be an empty array.

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-translator:
    component: KP
    team: []
```

The `biolink-version` value should be a full SemVer, with 'major.minor.patch'. Below, the field's value is an integer.

```yaml
tags:
  - name: translator
  - name: biothings
info:
  x-translator:
    component: KP
    team:
      - Service Provider
    biolink-version: '1'
    infores: 'infores:biolink-api'
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
