Currently, Restyler only processes public repositories. Full support for private repositories (through paid plans) is coming soon via the GitHub Marketplace. That said, motivated users who are interested in trying out Restyled and offering feedback at this early stage can request a private repository plan by [emailing support](mailto:support@restyled.io).

Before doing so, please consider the following:

1. You may be more interested in our [privacy policy](https://restyled.io/privacy-policy) and understanding generally how we handle your repository data, since we're no longer talking about data that's already public anyway.

1. Repository pages (e.g. `/gh/:owner/repos/:repo/*`) on restyled.io will verify with GitHub that the current user has access to this repository. Even in the unauthorized case, this can make the request take a little longer than a normal 404. This could be considered a [Timing Attack](https://en.wikipedia.org/wiki/Timing_attack), which leaks the fact that a private repository of the given name exists on GitHub. This only applies if (a) the repository is private and (b) the current user has authenticated with GitHub.

