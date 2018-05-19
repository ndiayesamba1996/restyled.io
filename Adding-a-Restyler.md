Adding a new Restyler can be done by anyone, though it is a multi-step process:

1. A PR in [`restyled-io/restylers`](https://github.com/restyled-io/restylers), to add `./{name}/Dockerfile`

   Usage for invoking the docker image should be `[command] [arguments] [path, ...]` and should restyle the (repository-relative) `path`s in place. `command` can be specified when you add your restyler to the system (step 3), along with default `arguments`, which can be overridden by users in their own configurations.

   Any support files can live in the `./{name}` directory as well. Please see all the existing directories here for reference.

   Feel free to open an early or uncertain PR and I'll be glad to help it along.

1. Once accepted, I will build and push the image to Docker Hub

1. A PR in [`restyled-io/restyler`](https://github.com/restyled-io/restyler) to:

   - Add the restyler to [`allRestylers`](https://github.com/restyled-io/restyler/blob/85eb1c50ed6f8fa25c20bcd21f7318fd9494fc7f/src/Restyler/Config.hs#L64)

     This is where you define what file extensions (and/or shebangs) you operate on and what arguments are needed when running the tool (in addition to the paths to restyle).

   -  Add an [example file](https://github.com/restyled-io/restyler/tree/master/test/core/fixtures) with some bad styling and a [test case](https://github.com/restyled-io/restyler/blob/85eb1c50ed6f8fa25c20bcd21f7318fd9494fc7f/test/core/main.t#L246) that exercises your restyler

1. Finally, update [this wiki](https://github.com/restyled-io/restyled.io/wiki/Available-Restylers) with information about your new restyler.