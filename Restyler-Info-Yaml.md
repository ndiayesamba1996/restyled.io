Every Restyler is described by a file at `{name}/info.yaml` in the
[restylers](https://github.com/restyled-io/restylers) repository. This page
documents that file.

## Minimal example

```yaml
---
name: foo

# And ONE of the following
version_cmd: |
  echo "v0.0.1"
version: v0.0.1
```

## Maximal example

```yaml
---
enabled: false
name: foo
# version, version_cmd
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

| Key | Type | Default value | Details
| --- | --- | --- | --- |
| `enabled` | `bool` | `false` | Run in the default configuration? |
| `name` | `string` | **required** | Unique name for this Restyler |
| `version_cmd`* | `string` | | A command to run to get the Restyler's version |
| `version`* | `string` | | An explicit version to use (overrides `version_cmd`) |
| `command` | `[string]` | `[$name]` | Auto-formatting command, and any "all the time" argument (e.g. `--inplace`) |
| `arguments` | `[string]` | `[]` | Additional arguments to include by default, but not required to function |
| `include` | `[pattern]` | `[]` | [Include Patterns](http://docs.restyled.io/restyler/restyler-0.2.0.0/Restyler-Config-Include.html) to match files this Restyler should operate on |
| `interpreters` | `[interpreter]` | `[]` | [Interpreters](http://docs.restyled.io/restyler/restyler-0.2.0.0/Restyler-Config-Interpreter.html) to match extension-less files this Restyler should operate on |
| `supports_arg_sep` | `bool` | `true` | Does this Restyler support `--` to separate paths from options? |
| `supports_multiple_path` | `bool` | `true` | Does this Restyler accept multiple paths at once? |
| `documentation` | `[string]` | `[]` | URLs to documentation that is useful during configuration or trouble-shooting |
| `metadata` | `Metadata` | |
| `metadata.languages` | `[string]` | `[]` | Free-form names of languages this Restyler supports |
| `metadata.tests` | `[Test]` | `[]` | |
| `metadata.tests[].support`** | `Support` | none | Support file (e.g. `.rubocop.yaml`) needed for the test |
| `metadata.tests[].support.path` | `string` | **required** | Name of the file |
| `metadata.tests[].support.contents` | `string` | **required** | Contents of the file |
| `metadata.tests[].extension` | `string` | `.temp` | Extension to use for restyled file |
| `metadata.tests[].contents` | `string` | **required** | Content to be restyled as the test |
| `metadata.tests[].restyled` | `string` | **required** | Expected content after restyling |

\* One of `version_cmd`, `version` is required. If possible, using
`version_cmd` is better because it means all you have to do is update
the actual `Dockerfile` and not manually keep `version` in sync. The
resulting Docker image we use will be named:

```
[{registry}/]restyled/restyler-${name}:${version-or-output-of-version_cmd}
```

\**: A `tests[].support` file will be present for *all* test cases if
*any* test case defines it. This is just a historical accident that we've
yet to correct.