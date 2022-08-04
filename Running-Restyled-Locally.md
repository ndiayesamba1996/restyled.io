The CLI that Restyled uses to actually execute auto-fixers can be run locally as a Docker image.

## Pre-requisites

cURL, Docker.

Have `~/.local/bin` (or equivalent) on `$PATH`.

## Installation

```console
curl -o ~/.local/bin/restyle-path \
  https://raw.githubusercontent.com/restyled-io/restyler/master/bin/restyle-path
chmod +x ~/.local/bin/restyle-path
```

## Usage

```console
% restyle-path -h
Usage: restyle-path [-t] [-p] [-d] <path...>
Options:
  -t            Docker tag of restyled/restyler image to run. Default is
                master-final, which is continiously built on CI of our master
                branch.

  -p            Instruct restyle-path to explicitly pull this image, even if it
                exists locally already.

                This can be useful if recent changes (typically to configuration
                handling) are causing the older image to no longer work.

  -d            Log verbosely.

Installation:
  Copy this script somewhere on $PATH.
```

### Examples

```
restyle-path src/**/*.hs
```

```
find src/ -name '*.hs' -exec restyle-path {} +
```

## Caveats

`restyle-path` will only pull the Docker image when it doesn't exist already. To update the image, use `-p`.

`restyle-path` will use the `remote_files` stanza of your configuration if present. This may leave extra files in your working directory afterwards.

Restyled, in general, operates on only files changed in a PR and in a fresh clone. This makes it run quickly and avoid many issues related to unexpected or non-source files. If you chose to run (e.g.) `restyle-path .`, these expectations are not met and that may lead to errors.