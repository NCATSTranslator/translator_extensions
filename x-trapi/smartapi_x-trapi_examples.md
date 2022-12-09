## Examples that should pass validation

This should work.

```yaml
tags:
  - name: trapi
  - name: biothings
info:
  x-trapi:
    version: 1.2.0
    operations:
      - lookup
    test_data_location: 'https://github.com/biothings/biothings.api/blob/master/test_data'
```

This also should work, since both the `info.x-trapi` section and the `trapi` tag aren't present.

```yaml
tags:
  - name: biothings
```

## info.x-trapi.test_data_location examples

Note that several formats are now supported:

* a single string (a single url; original format)
* an array of such strings
* an object with one or more key-value pairs.
  * Key options are `default`, `production`, `staging`, `testing`, `development`
  * (the last 4 correspond to the x-maturity level of different servers, defined by Architecture/ITRB environments). The `default` test data may be used to test servers of any x-maturity level.
  * The value is either a string or array (as defined above).

Thus, the following complex variants of the `test_data_location` should also work:

Variant A - `test_data_location` property value that is a list of URLs

```yaml
tags:
  - name: trapi
  - name: biothings
info:
  x-trapi:
    version: 1.2.0
    operations:
      - lookup
    test_data_location:
      - https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/master/biothings_explorer/sri-test-service-provider.json
      - https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/master/biothings_explorer/sri-test-multiomics.json
      - https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/master/biothings_explorer/sri-test-text-mining.json
```

Variant B - `test_data_location` property value in 'object' form

```yaml
tags:
  - name: trapi
  - name: biothings
info:
  contact:
    email: cwu@scripps.edu
    name: Chunlei Wu
    x-id: 'https://github.com/newgene'
    x-role: responsible developer
  description: Separate registration for the BTE's Service-Provider-specific endpoints.
  termsOfService: 'https://biothings.io/about'
  title: Service Provider TRAPI
  version: 2.8.1
  x-translator:
    biolink-version: 2.4.8
    component: KP
    infores: 'infores:service-provider-trapi'
    team:
      - Service Provider
  x-trapi:
    asyncquery: true
    operations:
      - lookup
    test_data_location:
      default:
        url:
          - >-
            https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/master/biothings_explorer/sri-test-service-provider.json
          - >-
            https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/master/biothings_explorer/sri-test-multiomics.json
          - >-
            https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/master/biothings_explorer/sri-test-text-mining.json
      development:
        url: >-
          https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/biolink3/biothings_explorer/qualifier-sri-test-service.json
    version: 1.3.0
```


## Examples with ERRORS that should fail validation

Within `info.x-trapi`, then `version` and `operations` are mandatory. Below, the version property is missing. 

```yaml
tags:
  - name: trapi
  - name: biothings
info:
  x-trapi:
    operations:
      - lookup
    test_data_location: 'https://github.com/biothings/biothings.api/blob/master/test_data'
```

Below, the `operations` property is missing.

```yaml
tags:
  - name: trapi
  - name: biothings
info:
  x-trapi:
    version: 1.2.0
    test_data_location: 'https://github.com/biothings/biothings.api/blob/master/test_data'
```

The `info.x-trapi.version` value should be a full TRAPI SemVer, with 'major.minor.patch'. Below, it is an integer. 

```yaml
tags:
  - name: trapi
  - name: biothings
info:
  x-trapi:
    version: '1'
    operations:
      - lookup
    test_data_location: 'https://github.com/biothings/biothings.api/blob/master/test_data'
```

The value for the `operations` property cannot be a string (it can be a one-element array).

```yaml
tags:
  - name: trapi
  - name: biothings
info:
  x-trapi:
    version: 1.2.0
    operations: lookup
    test_data_location: 'https://github.com/biothings/biothings.api/blob/master/test_data'
```

If the value of the `test_data_location` is a string, it should be an internet resolvable URL. Below, it is not.

```yaml
tags:
  - name: trapi
  - name: biothings
info:
  x-trapi:
    version: 1.2.0
    operations:
      - lookup
    test_data_location: test_data
```

A couple of things going wrong below...

* `test_data_location` should have valid keys which are `default` and/or a valid x-maturity: one of `production`, `staging`, `testing` or `development` (i.e. `main_server` is invalid)
* Test url objects also need to specifically have a `url` tag (`my_urls` is an invalid key)

```yaml
tags:
  - name: trapi
  - name: biothings
info:
  x-trapi:
    version: 1.2.0
    operations:
      - lookup
    test_data_location:
      default:
        my_urls:
          - >-
            https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/master/biothings_explorer/sri-test-service-provider.json
          - >-
            https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/master/biothings_explorer/sri-test-multiomics.json
          - >-
            https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/master/biothings_explorer/sri-test-text-mining.json
      main_server:
        url: >-
          https://raw.githubusercontent.com/NCATS-Tangerine/translator-api-registry/biolink3/biothings_explorer/qualifier-sri-test-service.json
```
