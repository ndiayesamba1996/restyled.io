Whenever you open a Pull Request, Restyled will clone your repository, branch from that branch, and run auto-formatters on any files changed in your original Pull Request.

If differences are found, a new Pull Request is opened and brought to your attention:

![Restyled.io Status](https://restyled.io/static/img/docs/differences-status.png)

If you want to adopt the restyled version, just merge that Pull Request into yours before continuing with your normal flow.

## Errors

If the Restyling process errors, a Red status will be added to the PR with a link to the Build details.

![Restyled.io Error](https://restyled.io/static/img/docs/error-details.png)

In addition to this URL, you can find Build lists at these paths too:

- All builds for a Pull Request: `https://restyled.io/gh/:owner/repos/:name/pulls/:number/jobs`
- All builds for a Repository: `https://restyled.io/gh/:owner/repos/:name/jobs`

## Cleanup

If at any point the PR's style becomes correct (i.e. restyling produces no differences), Restyled will close any Restyled PR it still has open, delete the `-restyled` branch, and set a Green status.