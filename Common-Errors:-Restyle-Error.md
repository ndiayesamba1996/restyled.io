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