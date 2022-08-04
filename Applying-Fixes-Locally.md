When Restyled runs, it captures the style fixing commits it makes by running `git format-patch <base>`. This output is made available by appending `/patch` to the Job URL. This means that running,

```console
curl <job-url>/patch | git am
```

Will apply Restyled's fixes directly to your local checkout.

This process even preserves all metadata about the commits: timestamp, message, and Restyled as author.

## FAQ

### Why is this useful?

Restyled is unable to make sibling PRs from forks because we don't have access in the fork repository, and making a PR in the original repository that includes contributions and fixes (as we used to do) is insecure. Therefore, an open source project that primarily accepts contributes from forks cannot make have Restyled's fixes automatically applied. This features allows he following feature for open source projects:

- A contributor opens a PR
- The Restyled PR status is Red, "differences found"
- The link to the Job shows the `curl | git am` command
- The contributors runs this command and pushes
- The Restyled PR status is now Green

This workflow is available in all scenarios, not just open source repositories, so anyone who finds this easier should take advantage! You can even configure Restyled to never open PRs, if you prefer.

### My Jobs say the patch is empty

The patch is retained as part of the Job log (which is literally the `stdout` of the `restyler` CLI). We retrain these logs for 30 days, after which the log and patch will be pruned.

The patch may also be literally empty, if there were no style fixes to make.

If you believe this is an error (neither of the above are true), please reach out to support@restyled.io.