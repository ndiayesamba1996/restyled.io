Restyled is built in the open and community contributions are very much encouraged.

## CLA

When you open your first Pull Request, https://cla-assistant.io/ will post a red status and ask you to sign our CLA.

## Repositories

Restyled has 3 major repositories:

[**restyled.io**](https://github.com/restyled-io/restyled.io)

This is the source behind https://restyled.io, most notably its `/webhooks` endpoint. This repository also encompasses our database models and Job abstraction for en-queuing and processing restyling jobs.

[**restyler**](https://github.com/restyled-io/restyler)

This is the complete process of restyling a single Pull Request encapsulated as a CLI and shipped in a Docker image. This is where to go if you want to change how Pull Request status updates happen, how errors are handled, how the comments look, etc.

Documentation for the restyler codebase can be found [here](http://docs.restyled.io/restyler/). It may or may not be useful, it may or may not be up to date.

[**restylers**](https://github.com/restyled-io/restylers)

This is a collection of `Dockerfile`s and `make` tasks for building, testing, and pushing them. This is where you want to go to add or fix an individual restyler. See https://github.com/restyled-io/restyled.io/wiki/Adding-a-Restyler.

## Getting Help

1. Open an Issue on the repository in question,
1. Hop in `#restyled` on Freenode (beware: it's new and very quiet), or
1. [Email Support](mailto:support@restyled.io)