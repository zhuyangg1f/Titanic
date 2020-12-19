# Workflows

If your project contains R package dependencies from private GitHub repositories
you will need to authenticate the workflows here to read from those repositories
by [creating a Personal Access Token (PAT)](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)
and storing it in a GitHub secret for this repository (**these actions expect
  the secret to be named 'PAT'**).

See documentation for creating GH secrets
[here](https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets#creating-encrypted-secrets-for-a-repository).

Store your GitHub PAT in a secure place (such as Keepass) so you can re-use it
for other repositories (you can only view/copy PATs once from GH).

## r-cmd-check.yaml
Runs:
``` R
devtools::check(cran = FALSE, error_on = "error")
```

## test-coverage
Runs:
``` R
covr::codecov(token = "${{ secrets.CODECOV_TOKEN }}")
```
This workflow integrates with [codecov.io](https://codecov.io/). To work properly
please create a codecov.io token (see website) and create a new GH secret called
`CODECOV_TOKEN` containing the token.

## test-R
Runs:
``` R
devtools::test(stop_on_failure=TRUE)
```
This workflow will fail if any unit test fails. You can see which tests failed
by navigating to `github.com/.../repo/actions`, selecting the failed test-R
workflow, and expanding the `Run Tests` step.
