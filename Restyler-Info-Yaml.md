Every Restyler is described by a file at `{name}/info.yaml` in the [restylers](https://github.com/restyled-io/restylers) repository. This page documents that file.

## Minimal example

```yaml
---
name: foo
version: v0.0.1
```

## Maximal example

```yaml
---
enabled: false
name: foo
image: restyled/restyler-foo:v0.0.1
command:
- foo
arguments: []
include: []
interpreters: []
supports_arg_sep: true
supports_multiple_path: true
documentation: []
metadata:
  languages: []
  tests:
    - support:
        path: x.conf
        contents: |
          Content for x.conf
      extension: .foo
      contents: |
        Content before fixing
      restyled: |
        Content after fixing
```

## Details

**enabled:true|false**: Should this Restyler run by default?

**name:string**: Unique name for this Restyler, required.

**image:string**: Docker image to pull and run. If not specified, **version:string** is required, and this will be defaulted to `restyled/restyler-$name:$version`.

**version:string**: See above. Required if not specifying `image`.

...