The restyling process can be configured by committing a YAML file named `.restyled.yaml` at the root of your repository.

Note that the `.restyled.yaml` **in the branch being Restyled** is what is used. If you make a configuration change on another branch (e.g. `master`), you will need to bring that change into any open Pull Requests (e.g. rebase) before Restyled will see it there.

The current default configuration is available [here](https://github.com/restyled-io/restyler/blob/master/config/default.yaml). Please see the source-comments in that file for details about all available options.