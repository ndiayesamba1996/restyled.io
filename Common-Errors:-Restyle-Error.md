## Exit code: 137

```
We had trouble with the {restyler} restyler
  Exited non-zero (137) for the following paths, {paths}
```

### Why does this happen?

Restylers are run in a restricted container, [including a memory limit of 512m](https://github.com/restyled-io/restyler/blob/d882e938025d72e6e21503f22890ebbbc4263e7c/src/Restyler/Restyler/Run.hs#L255-L256). If the process requires more memory than that, you will see a 137 exit code.

Generally speaking, Restyler images do little else beyond invoke the auto-formatter it wraps. Therefore, this usually represents that auto-formatter (e.g. clang-format, prettier, brittany) requiring too much memory to process the files that were changed in the PR. This means it is unlikely to be a coding error causing unusually high memory usage. It's just that this particular Restyler, on this particular set of paths, requires more memory than we allow.

That said, if you feel there is a bug in Restyled causing this, please do open an Issue with your evidence.

### What can I do about it?

The easiest thing is to just merge away. Optionally, correct the style locally using [`restyle-path`](https://github.com/restyled-io/restyled.io/wiki/Running-Restyled-Locally). Since this error is a function of the particular paths being changed, it's unlikely to be a persistent issue.

Check to see if there is an unusually large file being processed and leading to high memory usage. [Excluding this file](https://github.com/restyled-io/restyled.io/wiki/Excluding) may resolve the issues.

Check to see if there are files you don't intend to include being changed, such as built-and-commit assets. Excluding these may also help.

Break up the Pull Request into smaller ones that change fewer files. It's not ideal to change things _just_ to accommodate Restyled, but often breaking things up into smaller PRs can remain just as atomic, be easier to review, and safer to deploy.

## Too Many Changed Paths

### Why does this happen?

Restyled is a small project with limited resources. We're able to offer our services affordably to many users because we typically only operate on a small number of changed files per Pull Request. This allows us to run fewer services and instances and keep costs surprisingly low (about a penny per Job). When a Pull Request enters our system with a massive number of changed files, it:

- Takes too long to complete to be useful to the user that triggered it
- Backs up our main queues until auto-scaling kicks in, effecting other users
- Increases our costs while such Jobs run

None of this would be a problem if the work were useful, but (as any team that cares about code review will tell you) massive PRs are typically not those that represent real changes that need review and/or restyling. Most often, these large PRs are opened by bots or other automation, for example to synchronize a mirror. Restyled has no business operating on these PRs.

Therefore, we implemented a limit on the number of changed files we're willing to Restyle. This is not a hard limit, it's just a default set `.restyled.yaml`:

```yaml
changed_paths:
  maximum: 1000
  outcome: error
```

*By default*, if we see a Pull Request with over 1,000 changed files, we will report a Restyling Error.

### What can I do about it?

Anecdotally, Pull Requests with a high quantity of changed files fall into the following categories:

1. You're performing some synchronization between branches (such as updating a mirror)

   If you do this frequently, consider adjusting the `changed_paths` setting to silently skip these PRs:

   ```yaml
   changed_paths:
     maximum: 1000
     outcome: skip  # <--
   ```

1. You've made some bulk changes to not-actually-source files

   If you have a directory that is not actually source files, you can ignore them via the global [`exclude`](https://github.com/restyled-io/restyled.io/wiki/Configuring-Restyled#exclude) setting:

   ```yaml
   exclude:
     - "**/*.patch"
     - "**/node_modules/**/*"
     - "**/vendor/**/*"
     - ".github/workflows/**/*"
     - "**/assets/**/*"  # <--
   ```

   Changes to these files will no longer impact Restyled.

1. You're doing some kind of one-time mass change and you **don't** want Restyled to operate on all the changed files

   If Restyled is not a _required status_, or you have Admin rights, we suggest you just merge around the failing check. You don't want to add unnecessary `exclude`s or change the `outcome` to `skip` for this sort of one-time event.

1. You're doing some kind of one-time mass change and you **do** want Restyled to operate on all the changed files

   For this case, you could temporarily raise the `maximum` as necessary in the branch with the mass change:

   ```yaml
   changed_paths:
     maximum: 10000  # <--
     outcome: error
   ```

   When doing this, we ask that you consider the following:

   - Allowing Restyled to run on a massive number of files could be very slow, possibly fail, and impact our other customers. It's mostly fine, but please be responsible in this choice.
   - If it's not too much trouble, you could restyle the branch locally, using [`restyle-path`](https://github.com/restyled-io/restyler/blob/master/bin/restyle-path)