There are three ways to prevent Restyled from operating on a repository:

1. Commit a [configuration](https://github.com/restyled-io/restyled.io/wiki/Configuration-Reference) in the repository with `enabled: false`

   **.restyled.yaml**

   ```yaml
   ---
   enabled: false
   ```

1. Configure the GitHub App with a specific repositories list, and omit any repositories that you want disabled

   ![](https://help.github.com/assets/images/help/marketplace/marketplace-choose-repo-install-option.png)

1. Un-install the GitHub App entirely

The last two items can be found under *Settings* > *Applications* for a user account, or *Organization Settings* > *Installed GitHub Apps* for an Organization.