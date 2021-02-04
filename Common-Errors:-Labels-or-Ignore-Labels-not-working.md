Restyled supports adding labels to its PRs, or ignoring PRs that have certain labels. This is often used to tell automation to ignore Restyled PRs, or to tell Restyled to ignore other automations' PRs. Unfortunately, it doesn't (and can't) work 100% of the time.

The GitHub API doesn't support creating a PR with labels as a single step (like you can do in the UI). Therefore, any automation (Restyled included) must create the PR first, then attach labels after. If some other automation (Restyled included) operates on the PR in the moment it's been created but before the labels have been added, it will not be aware of them and may proceed unexpectedly.

We apologize for this inconvenience, but without support in the GitHub API for creating PRs with labels as a single step, and without all automation (Restyled included) being sure to use that approach, there isn't much we can do.

We are currently [extending our ignore functionality](https://github.com/restyled-io/restyler/issues/129). Since these new options are not label-based, they won't suffer form this problem and will be more robust.