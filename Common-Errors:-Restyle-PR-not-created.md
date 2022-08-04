The following are scenarios in which Restyled may not create a PR:

## Restyling error

If we encounter an error and are unable to open the PR. If this is the case, you should see:

1. A Red Restyled PR status
1. Details about some error in the Job log

## Restyled found no differences

If nothing needed fixing, no PR will be created. If this is the case, you should see

1. A Green Restyled PR status
1. "Restyling produced no differences" in the Job log

## Disabled by config

If your `restyled.yaml` contains `pull_requests:false`, a PR will not be created.

https://github.com/restyled-io/restyled.io/wiki/Configuring-Restyled#pull-requests

## The original PR is from a fork

Restyled cannot create a sibling PR in the fork repository since it may not be installed there.

To work around this, Restyled _used to_ create a PR in the original repository that contained the original contributions and the style fixes together. Fork PRs come with guard rails to safely handle external contributions (e.g. hiding secrets, requiring approval) since such contributions might update CI configuration to exfiltrate data or otherwise compromise the system. Non-fork PRs have non of these guardrails. Restyled was effectively copying external contributions from a fork PR into a non-fork PR, bypassing these guard rails.

Therefore, Restyled will not create PRs for this scenario any more.

Instead, the Job log will direct your contributor to [apply the fixes locally](https://github.com/restyled-io/restyled.io/wiki/Applying-Fixes-Locally)