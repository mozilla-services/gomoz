# go.mozilla.org

This repository contains the source files of go.mozilla.org.

## Add a new project

Copy one of the project file and edit the source and godoc locations to match
your own. The name you use in `go.mozilla.org/<project>` does not need to be the
same as the one you host the code under. For example,
`https://github.com/mozilla-services/hawk-go` uses `go.mozilla.org/hawk`.

## Publish files

As an AWS admin of the prod IAM, use this command:

```bash
aws s3 sync . s3://go.mozilla.org --exclude "README.md" --exclude ".git/*"
```
