# How to publish a new version of go.mozilla.org

## Publish files in S3

As an AWS admin of the prod IAM, use this command:

```bash
aws s3 sync . s3://go.mozilla.org \
    --exclude "README.md" --exclude ".git/*" \
    --content-type "text/html" --acl public-read
```

Then invalidate the cloudfront cache

```bash
$ aws cloudfront create-invalidation --distribution-id ESQYDMA17GDLC --paths '/*'
```
