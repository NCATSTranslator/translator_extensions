This should work:

```yaml
tags:
  - name: translator
  - name: kge
info:
  x-kge:
    version:
      latest: '1.0'
```

This should also work, because both the `x-kge` extension and the `kge` tag aren't present.

```yaml
tags:
  - name: biothings
```
