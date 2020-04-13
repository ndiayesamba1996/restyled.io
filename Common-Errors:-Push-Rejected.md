## Stale info

```
To https://github.com/....git
 ! [rejected]              ... -> ... (stale info)
error: failed to push some refs to 'https://<SCRUBBED>@github.com/....git'
Setting status of error for ...
```

This means that multiple Restyled jobs ran at once, and things changed between the time this Job cloned and the time it finished and attempted this push. Since we use `--force-with-lease`, `git` is refusing to overwrite possibly more correct work (theirs) with possibly less correct (ours).

This is an intermittent race condition, and pushing a new commit to your PR should Restyle correctly. We're exploring options for handling this scenario better, feel free to follow along in https://github.com/restyled-io/restyler/issues/88.

## Refusing to allow a bot...

```
To https://github.com/....git
 ! [remote rejected] ... (refusing to allow a bot to create or update workflow `.github/workflows/...`)
error: failed to push some refs to 'https://<SCRUBBED>@github.com/....git'
[Info] Setting status of error for ...
```

GitHub may prevent a bot user (which we are) from pushing a change to anything under `.github/workflows`. This means any Restyle PRs that attempt to change such a file will encounter this error.

To work around this, we have a default global [`exclude`](https://github.com/restyled-io/restyler/blob/master/config/default.yaml#L24) for them. If you are encountering this error and you've changed that setting, please update it to keep the `.github/workflows` element.

Even with this setting, there is still another way you may run into this: if you have a Fork PR that itself edits or adds such a file.

With Fork PRs, we open a new PR which includes the original change and the restyling. If the original change is one disallowed for bots by GitHub, we'll see this error.

Not much can be done to avoid this, and you will most likely want to merge such PRs without Restyled feedback. If you really wanted to get Restyled working, you could:

- Locally use [`restyle-path`](https://github.com/restyled-io/restyler/blob/master/bin/restyle-path) and push the results to the original PR. If there are no differences, Restyled will not attempt to open a Restyle PR and so will not error.

- Separate any changes to `.github/workflows` and merge them first (without Restyled's feedback), then open the rest of the changes again. Restyled shouldn't error on that new PR.