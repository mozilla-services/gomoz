# go.mozilla.org

This repository contains the source files of go.mozilla.org.

## Add a new project

Create a directory with the name of your project and create an index.html file
in the directory that references the git location of the source code.

You can copy an existing index.html from another project and edit it.

The name you use in `go.mozilla.org/<project>` does not need to be the
same as the one you host the code under. For example,
`https://github.com/mozilla-services/hawk-go` uses `go.mozilla.org/hawk`.

If your project has packages, create a directory structure that matches the
packages structure, and give each package its own index.html file.

For example:
```bash
userplex/
├── index.html
└── modules
    ├── authorizedkeys
    │   └── index.html
    ├── aws
    │   └── index.html
    ├── datadog
    │   └── index.html
    └── index.html

```

Note that sub-packages must use the git location of the top-level repository.
For example, 
```
<meta name="go-import"
    content="go.mozilla.org/userplex/modules/aws
             git https://github.com/mozilla-services/userplex">
```

The sub-package `go.mozilla.org/userplex/modules/aws` uses the top-level git
repository `https://github.com/mozilla-services/userplex`.

## Set a custom import path in your packages

This is only needed for packages that are importable. The `main` package does
not need to specify a custom import path since it cannot be imported by other
Go programs.

In the source go files of your project, next to the package name, add a line
that references the `go.mozilla.org` import path:
```go
package mymodule // import "go.mozilla.org/myproject/mymodule"
```

Note that your project must now live under
`$GOPATH/src/go.mozilla.org/myproject`. 

## Use custom GOPATH in CI

### travis-ci

TravisCI has a directive to indicate the import path of your package, just add
`go_import_path` to your `.travis.yml`.

```yaml
language: go
go:
  - 1.5
  - 1.6
  - tip
go_import_path: go.mozilla.org/myproject
script:
  - go test go.mozilla.org/myproject
```

## Publish files in S3

As an AWS admin of the prod IAM, use this command:

```bash
aws s3 sync . s3://go.mozilla.org \
    --exclude "README.md" --exclude ".git/*" \
    --content-type "text/html" --acl public-read \
    --delete
```

Then invalidate the cloudfront cache

```bash
$ aws cloudfront create-invalidation --distribution-id ESQYDMA17GDLC --paths '/*'
```
