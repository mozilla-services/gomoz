# go.mozilla.org

This repository contains the source files of go.mozilla.org.

## Add a new project

Copy one of the project file and edit the source and godoc locations to match
your own. The name you use in `go.mozilla.org/<project>` does not need to be the
same as the one you host the code under. For example,
`https://github.com/mozilla-services/hawk-go` uses `go.mozilla.org/hawk`.

## Using a custom import path in your project

In the source go files of your project, next to the package name, add a line
that references the `go.mozilla.org` import path:
```go
package main // import "go.mozilla.org/myproject"
```

Note that your project must now live under
`$GOPATH/src/go.mozilla.org/myproject`. 

If you use a CI platform, make sure the source code is located in the right
place before running tests, which may not be the case if the CI just clone the
repo without trying to place it in the right location.

For example, in TravisCI:
```yaml
language: go
go:
  - 1.5
  - 1.6
  - tip
before_install:
  - mkdir -p $GOPATH/src/go.mozilla.org
  - mv $GOPATH/src/github.com/mozilla-services/myproject $GOPATH/src/go.mozilla.org/myproject
script:
  - go test go.mozilla.org/myproject
```

## Publish files in S3

As an AWS admin of the prod IAM, use this command:

```bash
aws s3 sync . s3://go.mozilla.org \
    --exclude "README.md" --exclude ".git/*" \
    --content-type "text/html" --acl public-read
```
