## Restylers

### astyle

Languages:

- C
- C++
- C#
- Java\*
- Objective-C

Configuration:

```yaml
---
name: astyle
enabled: true
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

Links:

- http://astyle.sourceforge.net/astyle.html

### autopep8

Languages:

- Python

Configuration:

```yaml
---
name: autopep8
enabled: true
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

Links:

- https://github.com/hhatto/autopep8

### black

Languages:

- Python

Configuration:

```yaml
---
name: black
enabled: true
image: restyled/restyler-black:v19.3b0
command:
- black
arguments: []
include:
- "**/*.py"
interpreters:
- python
```

Links:

- https://github.com/python/black

### brittany

Languages:

- Haskell\*

Configuration:

```yaml
---
name: brittany
enabled: false
image: restyled/restyler-brittany:v0.12.0.0
command:
- brittany
- "--write-mode=inplace"
arguments: []
include:
- "**/*.hs"
interpreters: []
```

Links:

- https://github.com/lspitzner/brittany
- https://github.com/restyled-io/restyled.io/wiki/Common-Errors:-Brittany

### clang-format

Languages:

- C
- C++
- Java
- JavaScript
- Objective-C
- Protobuf
- C#

Configuration:

```yaml
---
name: clang-format
enabled: true
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

Links:

- https://clang.llvm.org/docs/ClangFormat.html

### dfmt

Languages:

- D

Configuration:

```yaml
---
name: dfmt
enabled: true
image: restyled/restyler-dfmt:v0.8.2
command:
- dfmt
- "--inplace"
arguments: []
include:
- "**/*.d"
interpreters: []
```

Links:

- https://github.com/dlang-community/dfmt#readme

### elm-format

Languages:

- Elm

Configuration:

```yaml
---
name: elm-format
enabled: true
image: restyled/restyler-elm-format:v0.6.1-alpha
command:
- elm-format
- "--yes"
arguments: []
include:
- "**/*.elm"
interpreters: []
```

Links:

- https://github.com/avh4/elm-format

### google-java-format

Languages:

- Java

Configuration:

```yaml
---
name: google-java-format
enabled: false
image: restyled/restyler-google-java-format:v1.6
command:
- google-java-format
- "--replace"
arguments: []
include:
- "**/*.java"
interpreters: []
```

Links:

- https://github.com/google/google-java-format#readme

### hindent

Languages:

- Haskell\*

Configuration:

```yaml
---
name: hindent
enabled: false
image: restyled/restyler-hindent:v5.2.5
command:
- hindent
arguments: []
include:
- "**/*.hs"
interpreters: []
```

Links:

- https://github.com/commercialhaskell/hindent

### hlint

Languages:

- Haskell

Configuration:

```yaml
---
name: hlint
enabled: false
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

Links:

- https://github.com/ndmitchell/hlint#readme
- https://github.com/restyled-io/restyled.io/wiki/Common-Errors:-HLint

### jdt

Languages:

- Java
- JavaScript\*
- CSS
- HTML
- JSON
- XML

Configuration:

```yaml
---
name: jdt
enabled: false
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

Links:

- https://code.revelc.net/formatter-maven-plugin/

### ormolu

Languages:

- Haskell\*

Configuration:

```yaml
---
name: ormolu
enabled: false
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

Links:

- https://github.com/tweag/ormolu#readme

### pg_format

Languages:

- PSQL

Configuration:

```yaml
---
name: pg_format
enabled: true
image: restyled/restyler-pg_format:v3.3
command:
- pg_format-inplace
arguments: []
include:
- "**/*.sql"
interpreters: []
```

Links:

- https://github.com/darold/pgFormatter#readme

### php-cs-fixer

Languages:

- PHP

Configuration:

```yaml
---
name: php-cs-fixer
enabled: true
image: restyled/restyler-php-cs-fixer:v2.14.2
command:
- php-cs-fixer
- fix
arguments: []
include:
- "**/*.php"
interpreters: []
```

Links:

- https://github.com/FriendsOfPHP/PHP-CS-Fixer

### prettier-markdown

Languages:

- Markdown

Configuration:

```yaml
---
name: prettier-markdown
enabled: true
image: restyled/restyler-prettier:v1.18.2
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

Links:

- https://prettier.io/blog/2017/11/07/1.8.0.html

### prettier-ruby

Languages:

- Ruby

Configuration:

```yaml
---
name: prettier-ruby
enabled: true
image: restyled/restyler-prettier-ruby:v0.15.0
command:
- prettier
- "--write"
arguments: []
include:
- "**/*.rb"
interpreters:
- ruby
```

Links:

- https://prettier.io/docs/en/
- https://github.com/prettier/plugin-ruby

### prettier-yaml

Languages:

- Yaml

Configuration:

```yaml
---
name: prettier-yaml
enabled: true
image: restyled/restyler-prettier:v1.18.2
command:
- prettier
- "--write"
arguments: []
include:
- "**/*.yml"
- "**/*.yaml"
interpreters: []
```

Links:

- https://prettier.io/docs/en/
- https://prettier.io/blog/2018/07/29/1.14.0.html

### prettier

Languages:

- JavaScript

Configuration:

```yaml
---
name: prettier
enabled: true
image: restyled/restyler-prettier:v1.18.2
command:
- prettier
- "--write"
arguments: []
include:
- "**/*.js"
- "**/*.jsx"
interpreters: []
```

Links:

- https://prettier.io/docs/en/

### reorder-python-imports

Languages:

- Python

Configuration:

```yaml
---
name: reorder-python-imports
enabled: true
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

Links:

- https://github.com/asottile/reorder_python_imports

### rubocop

Languages:

- Ruby

Configuration:

```yaml
---
name: rubocop
enabled: true
image: restyled/restyler-rubocop:v0.72.0-2
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

Links:

- https://rubocop.readthedocs.io/en/latest/

### rustfmt

Languages:

- Rust

Configuration:

```yaml
---
name: rustfmt
enabled: true
image: restyled/restyler-rustfmt:v1.4.8-nightly
command:
- rustfmt
arguments: []
include:
- "**/*.rs"
interpreters: []
```

Links:

- https://github.com/rust-lang-nursery/rustfmt#readme
- https://github.com/restyled-io/restyled.io/wiki/Common-Errors:-Rustfmt

### shellharden

Languages:

- POSIX sh
- Bash

Configuration:

```yaml
---
name: shellharden
enabled: true
image: restyled/restyler-shellharden:v4.1.1
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

Links:

- https://github.com/anordal/shellharden#readme

### shfmt

Languages:

- POSIX sh
- Bash

Configuration:

```yaml
---
name: shfmt
enabled: true
image: restyled/restyler-shfmt:v2.4.0-2
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

Links:

- https://github.com/mvdan/sh#shfmt

### stylish-haskell

Languages:

- Haskell

Configuration:

```yaml
---
name: stylish-haskell
enabled: true
image: restyled/restyler-stylish-haskell:v0.9.2.2
command:
- stylish-haskell
- "--inplace"
arguments: []
include:
- "**/*.hs"
interpreters: []
```

Links:

- https://github.com/jaspervdj/stylish-haskell
- https://github.com/restyled-io/restyled.io/wiki/Common-Errors-Stylish-Haskell

### terraform

Languages:

- Terraform

Configuration:

```yaml
---
name: terraform
enabled: true
image: restyled/restyler-terraform:v0.11.7
command:
- terraform
- fmt
arguments: []
include:
- "**/*.tf"
interpreters: []
```

Links:

- https://www.terraform.io/docs/commands/fmt.html

### yapf

Languages:

- Python

Configuration:

```yaml
---
name: yapf
enabled: true
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

Links:

- https://github.com/google/yapf

\**Language must be explicitly enabled.*

## Restyler Sets

### master

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/master/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20191004...master)

### 20191004

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20191004/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190930...20191004)

### 20190930

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190930/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190922...20190930)

### 20190922

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190922/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190910...20190922)

### 20190910

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190910/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190904...20190910)

### 20190904

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190904/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190903...20190904)

### 20190903

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190903/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190821...20190903)

### 20190821

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190821/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190801...20190821)

### 20190801

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190801/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190715...20190801)

### 20190715

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190715/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190628...20190715)

### 20190628

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190628/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190627...20190628)

### 20190627

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190627/restylers.yaml)
- [Changes](https://github.com/restyled-io/restylers/compare/20190625...20190627)

### 20190625

- [`restylers.yaml`](https://github.com/restyled-io/restylers/blob/20190625/restylers.yaml)

See [Restyler Versions](https://github.com/restyled-io/restyled.io/wiki/Restyler-Versions) for how to use these Sets in your configuration.

## Adding Restylers

If you know of something that would make a good Restyler, please [create an Issue](https://github.com/restyled-io/restylers/issues/new?title=some-auto-formatter&body=https://their-homepage.com).

If you're interested in contributing, see [Adding a Restyler](https://github.com/restyled-io/restyled.io/wiki/Adding-a-Restyler).
