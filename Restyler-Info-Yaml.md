Every Restyler is described by a file at `{name}/info.yaml` in the
[restylers](https://github.com/restyled-io/restylers) repository. This page
documents that file.

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

## Schema Details

### Info

| Key | Type | Default value | Details
| --- | --- | --- | --- |
| `enabled` | `bool` | `false` | Run in the default configuration? |
| `name` | `string` | **required** | Unique name for this Restyler |
| `image` | `string` | | See below |
| `version` | `string` | | See below |
| `command` | `[string]` | `[$name]` | Auto-formatting command, and any "all the time" argument (e.g. `--inplace`) |
| `arguments` | `[string]` | `[]` | Additional arguments to include by default, but not required to function |
| `include` | `[pattern]` | `[]` | [Include Patterns](http://docs.restyled.io/restyler/restyler-0.2.0.0/Restyler-Config-Include.html) to match files this Restyler should operate on |
| `interpreters` | `[interpreter]` | `[]` | [Interpreters](http://docs.restyled.io/restyler/restyler-0.2.0.0/Restyler-Config-Interpreter.html) to match extension-less files this Restyler should operate on |
| `supports_arg_sep` | `bool` | `true` | Does this Restyler support `--` to separate paths from options? |
| `supports_multiple_path` | `bool` | `true` | Does this Restyler accept multiple paths at once? |
| `documentation` | `[string]` | `[]` | URLs to documentation that is useful during configuration or trouble-shooting |
| `metadata` | `Metadata` | |

One of `image` or `version` is required. When `image` is not given,

```
restyled/restyler-${name}:${version}
```

is used. When `image` *is* given, `version` is ignored.

### Metadata

Information not used in the actual *execution* of a Restyler.

| Key | Type | Default value | Details
| --- | --- | --- | --- |
| `languages` | `[string]` | `[]` | Free-form names of languages this Restyler supports |
| `tests` | `[Test]` | `[]` | |

### Test

Examples of what this Restyler fixes.

| Key | Type | Default value | Details
| --- | --- | --- | --- |
| `support` | `Support` | none | Support file (e.g. `.rubocop.yaml`) needed for the test |
| `extension` | `string` | `.temp` | Extension to use for restyled file |
| `contents` | `string` | **required** | Content to be restyled as the test |
| `restyled` | `string` | **required** | Expected content after restyling |

### Support

Other files that must be present for the test cases, e.g. `.rubocop.yaml` or
`foo.csproj`. **NOTE**: A support file will be present for *all* test cases if
*any* test case defines it. This is just a historical accident that it's defined
on a per-test level at the moment.

| Key | Type | Default value | Details
| --- | --- | --- | --- |
| `path` | `string` | **required** | Name of the file |
| `contents` | `string` | **required** | Contents of the file |