Restyled is built in the open and community contributions are very much encouraged.

## CLA

Restyled does not yet have a [Contributor License Agreement](https://en.wikipedia.org/wiki/Contributor_License_Agreement), though if/when contributions start happening that will be something that gets set up quickly. Until then, code you add becomes licensed according to the `LICENSE` files in the repositories you are contributing to. At this time, everything is [Commons Claused](https://commonsclause.com/) [MIT](https://en.wikipedia.org/wiki/MIT_License)-licensed.

## Repositories

Restyled has 3 major repositories:

[**restyled.io**](https://github.com/restyled-io/restyled.io)

This is the source behind https://restyled.io, most notably its `/webhooks` endpoint. This repository also encompasses our database models and Job abstraction for en-queuing and processing restyling jobs.

It's unlikely an outside contribution in this area would be needed. Besides the webhook-handling and Jobs abstraction, very little happens here.

[**restyler**](https://github.com/restyled-io/restyler)

This is the complete process of restyling a single Pull Request encapsulated as a CLI and shipped in a Docker image. This is where to go if you want to change how Pull Request status updates happen, how errors are handled, how the comments look, etc.

Documentation for the restyler codebase can be found [here](docs.restyled.io.s3-website-us-east-1.amazonaws.com/restyler/). It may or may not be useful, it may or may not be up to date.

[**restylers**](https://github.com/restyled-io/restylers)

This is a collection of `Dockerfile`s and a [Shake](https://shakebuild.com/) script for building, testing, and pushing them. This is where you want to go to add or fix an individual restyler.

## Getting Help

1. Open an Issue on the repository in question,
1. Hop in `#restyled` on Freenode (beware: it's new and very quiet), or
1. [Email Support](mailto:support@restyled.io)