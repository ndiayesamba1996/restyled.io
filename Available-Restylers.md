## astyle

C, C++, C#, Java\*, Objective-C

```yaml
---
image: restyled/restyler-astyle:v3.1
command:
- astyle
arguments: []
include:
- "**/*.c"
- "**/*.cc"
- "**/*.cpp"
- "**/*.cxx"
- "**/*.c++"
- "**/*.C"
- "**/*.cs"
- "**/*.h"
- "**/*.hh"
- "**/*.hpp"
- "**/*.hxx"
- "**/*.h++"
- "**/*.H"
- "**/*.m"
- "**/*.mm"
interpreters: []
```

- http://astyle.sourceforge.net/astyle.html

## autopep8

Python

```yaml
---
image: restyled/restyler-autopep8:v1.4.4
command:
- autopep8
- "--in-place"
arguments: []
include:
- "**/*.py"
interpreters:
- python
```

- https://github.com/hhatto/autopep8

## black

Python

```yaml
---
image: restyled/restyler-black:v19.10b0
command:
- black
arguments: []
include:
- "**/*.py"
interpreters:
- python
```

- https://github.com/python/black

## brittany\*

Haskell

```yaml
---
image: restyled/restyler-brittany:v0.12.1.1
command:
- brittany
- "--write-mode=inplace"
arguments: []
include:
- "**/*.hs"
interpreters: []
```

- https://github.com/lspitzner/brittany
- https://github.com/restyled-io/restyled.io/wiki/Common-Errors:-Brittany

## clang-format

C, C++, Java, JavaScript, Objective-C, Protobuf, C#

```yaml
---
image: restyled/restyler-clang-format:v9.0.0
command:
- clang-format
- "-i"
arguments: []
include:
- "**/*.c"
- "**/*.cc"
- "**/*.cpp"
- "**/*.cxx"
- "**/*.c++"
- "**/*.C"
- "**/*.cs"
- "**/*.h"
- "**/*.hh"
- "**/*.hpp"
- "**/*.hxx"
- "**/*.h++"
- "**/*.H"
- "**/*.java"
- "**/*.js"
- "**/*.m"
interpreters: []
```

- https://clang.llvm.org/docs/ClangFormat.html

## dfmt

D

```yaml
---
image: restyled/restyler-dfmt:v0.8.2
command:
- dfmt
- "--inplace"
arguments: []
include:
- "**/*.d"
interpreters: []
```

- https://github.com/dlang-community/dfmt#readme

## dotnet-format\*

C#, VB.NET

```yaml
---
image: restyled/restyler-dotnet-format:v0.0.1-2
command:
- dotnet-format-files
arguments: []
include:
- "**/*.cs"
- "**/*.vb"
interpreters: []
```

- https://github.com/dotnet/format

## elm-format

Elm

```yaml
---
image: restyled/restyler-elm-format:v0.6.1-alpha-2
command:
- elm-format
- "--yes"
arguments: []
include:
- "**/*.elm"
interpreters: []
```

- https://github.com/avh4/elm-format

## fantomas

F#

```yaml
---
image: restyled/restyler-fantomas:v3.3.0
command:
- fantomas
arguments: []
include:
- "**/*.fs"
- "**/*.fsi"
- "**/*.fsx"
interpreters: []
```

- https://github.com/fsprojects/fantomas

## google-java-format\*

Java

```yaml
---
image: restyled/restyler-google-java-format:v1.6
command:
- google-java-format
- "--replace"
arguments: []
include:
- "**/*.java"
interpreters: []
```

- https://github.com/google/google-java-format#readme

## hindent\*

Haskell

```yaml
---
image: restyled/restyler-hindent:v5.3.1
command:
- hindent
arguments: []
include:
- "**/*.hs"
interpreters: []
```

- https://github.com/commercialhaskell/hindent

## hlint\*

Haskell

```yaml
---
image: restyled/restyler-hlint:v2.1.11
command:
- hlint
- lint
- "--refactor"
- "--refactor-options=-i"
arguments: []
include:
- "**/*.hs"
interpreters: []
```

- https://github.com/ndmitchell/hlint#readme
- https://github.com/restyled-io/restyled.io/wiki/Common-Errors:-HLint

## isort

Python

```yaml
---
image: restyled/restyler-isort:v4.3.21
command:
- isort
arguments: []
include:
- "**/*.py"
interpreters:
- python
```

- https://github.com/timothycrosley/isort/

## jdt\*

Java, JavaScript\*, CSS, HTML, JSON, XML

```yaml
---
image: restyled/restyler-jdt:v2.10.0
command:
- formatter
arguments: []
include:
- "**/*.java"
- "**/*.css"
- "**/*.html"
- "**/*.json"
- "**/*.xml"
interpreters: []
```

- https://code.revelc.net/formatter-maven-plugin/

## jq

JSON

```yaml
---
image: restyled/restyler-jq:v1.6-2
command:
- jq-write
arguments: []
include:
- "**/*.json"
interpreters: []
```

- https://stedolan.github.io/jq/

## ormolu\*

Haskell

```yaml
---
image: restyled/restyler-ormolu:v0.0.1.0
command:
- ormolu
- "--mode"
- inplace
arguments: []
include:
- "**/*.hs"
interpreters: []
```

- https://github.com/tweag/ormolu#readme

## pg_format

PSQL

```yaml
---
image: restyled/restyler-pg_format:v3.3
command:
- pg_format-inplace
arguments: []
include:
- "**/*.sql"
interpreters: []
```

- https://github.com/darold/pgFormatter#readme

## php-cs-fixer

PHP

```yaml
---
image: restyled/restyler-php-cs-fixer:v2.14.2
command:
- php-cs-fixer
- fix
arguments: []
include:
- "**/*.php"
interpreters: []
```

- https://github.com/FriendsOfPHP/PHP-CS-Fixer

## prettier-json

JSON

```yaml
---
image: restyled/restyler-prettier:v2.0.2-1
command:
- prettier
- "--write"
arguments: []
include:
- "**/*.json"
interpreters: []
```

- https://prettier.io/docs/en/options.html#parser

## prettier-markdown

Markdown

```yaml
---
image: restyled/restyler-prettier:v2.0.2-1
command:
- prettier
- "--write"
arguments:
- "--print-width"
- '80'
- "--prose-wrap"
- always
include:
- "**/*.md"
- "**/*.markdown"
interpreters: []
```

- https://prettier.io/blog/2017/11/07/1.8.0.html

## prettier-ruby\*

Ruby

```yaml
---
image: restyled/restyler-prettier-ruby:v0.15.0-3
command:
- prettier
- "--write"
arguments: []
include:
- "**/*.rb"
interpreters:
- ruby
```

- https://prettier.io/docs/en/
- https://github.com/prettier/plugin-ruby

## prettier-yaml

Yaml

```yaml
---
image: restyled/restyler-prettier:v2.0.2-1
command:
- prettier
- "--write"
arguments: []
include:
- "**/*.yml"
- "**/*.yaml"
interpreters: []
```

- https://prettier.io/docs/en/
- https://prettier.io/blog/2018/07/29/1.14.0.html

## prettier

JavaScript

```yaml
---
image: restyled/restyler-prettier:v2.0.2-1
command:
- prettier
- "--write"
arguments: []
include:
- "**/*.js"
- "**/*.jsx"
interpreters: []
```

- https://prettier.io/docs/en/

## reorder-python-imports

Python

```yaml
---
image: restyled/restyler-reorder-python-imports:v1.6.0
command:
- reorder-python-imports
- "--exit-zero-even-if-changed"
arguments: []
include:
- "**/*.py"
interpreters:
- python
```

- https://github.com/asottile/reorder_python_imports

## rubocop

Ruby

```yaml
---
image: restyled/restyler-rubocop:v0.72.0-4
command:
- rubocop
- "--auto-correct"
- "--fail-level"
- fatal
arguments: []
include:
- "**/*.rb"
interpreters:
- ruby
```

- https://rubocop.readthedocs.io/en/latest/

## rustfmt

Rust

```yaml
---
image: restyled/restyler-rustfmt:v1.4.14-nightly
command:
- rustfmt
arguments: []
include:
- "**/*.rs"
interpreters: []
```

- https://github.com/rust-lang-nursery/rustfmt#readme
- https://github.com/restyled-io/restyled.io/wiki/Common-Errors:-Rustfmt

## shellharden

POSIX sh, Bash

```yaml
---
image: restyled/restyler-shellharden:v4.1.1-2
command:
- shellharden
- "--replace"
arguments: []
include:
- "**/*.sh"
- "**/*.bash"
interpreters:
- sh
- bash
```

- https://github.com/anordal/shellharden#readme

## shfmt

POSIX sh, Bash

```yaml
---
image: restyled/restyler-shfmt:v3.0.1
command:
- shfmt
- "-w"
arguments:
- "-i"
- '2'
- "-ci"
include:
- "**/*.sh"
- "**/*.bash"
interpreters:
- sh
- bash
```

- https://github.com/mvdan/sh#shfmt

## stylish-haskell

Haskell

```yaml
---
image: restyled/restyler-stylish-haskell:v0.9.2.2-2
command:
- stylish-haskell
- "--inplace"
arguments: []
include:
- "**/*.hs"
interpreters: []
```

- https://github.com/jaspervdj/stylish-haskell
- https://github.com/restyled-io/restyled.io/wiki/Common-Errors-Stylish-Haskell

## terraform

Terraform

```yaml
---
image: restyled/restyler-terraform:v0.12.24
command:
- terraform
- fmt
arguments: []
include:
- "**/*.tf"
interpreters: []
```

- https://www.terraform.io/docs/commands/fmt.html

## whitespace

\*

```yaml
---
image: restyled/restyler-whitespace:v0.1.0.0-2
command:
- whitespace
arguments: []
include:
- "**/*"
- "!**/*.gif"
- "!**/*.ico"
- "!**/*.jpeg"
- "!**/*.jpg"
- "!**/*.pdf"
- "!**/*.png"
interpreters: []
```

- https://github.com/restyled-io/restylers/blob/master/whitespace/README.md

## yapf

Python

```yaml
---
image: restyled/restyler-yapf:v0.27.0
command:
- yapf
- "--in-place"
arguments: []
include:
- "**/*.py"
interpreters:
- python
```

- https://github.com/google/yapf

\**Must be explicitly enabled.*

## Restyler Sets

- **master**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/master/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20200428...master)
- **20200428**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20200428/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20200426...20200428)
- **20200426**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20200426/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20200420...20200426)
- **20200420**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20200420/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20200331...20200420)
- **20200331**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20200331/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20200326...20200331)
- **20200326**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20200326/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20200209...20200326)
- **20200209**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20200209/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20200113...20200209)
- **20200113**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20200113/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191226...20200113)
- **20191226**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191226/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191216...20191226)
- **20191216**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191216/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191213...20191216)
- **20191213**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191213/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191212...20191213)
- **20191212**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191212/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191206...20191212)
- **20191206**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191206/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191203...20191206)
- **20191203**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191203/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191114...20191203)
- **20191114**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191114/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191031...20191114)
- **20191031**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191031/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191012...20191031)
- **20191012**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191012/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20191004...20191012)
- **20191004**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191004/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190930...20191004)
- **20190930**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190930/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190922...20190930)
- **20190922**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190922/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190910...20190922)
- **20190910**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190910/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190904...20190910)
- **20190904**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190904/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190903...20190904)
- **20190903**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190903/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190821...20190903)
- **20190821**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190821/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190801...20190821)
- **20190801**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190801/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190715...20190801)
- **20190715**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190715/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190628...20190715)
- **20190628**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190628/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190627...20190628)
- **20190627**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190627/restylers.yaml), [changes](https://github.com/restyled-io/restylers/compare/20190625...20190627)
- **20190625**: [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190625/restylers.yaml)

See [Restyler Versions](https://github.com/restyled-io/restyled.io/wiki/Restyler-Versions) for how to use these Sets in your configuration.

## Adding Restylers

If you know of something that would make a good Restyler, please [create an Issue](https://github.com/restyled-io/restylers/issues/new?title=some-auto-formatter&body=https://their-homepage.com).

If you're interested in contributing, see [Adding a Restyler](https://github.com/restyled-io/restyled.io/wiki/Adding-a-Restyler).
