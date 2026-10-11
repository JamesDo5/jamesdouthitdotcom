# jamesdouthit.com

Static pages for the root of jamesdouthit.com. The 500 client is a separate repo and is published under
`/500cardgame/` by its own workflow; this repo never touches that prefix.

Pages: `index.html`, `prose/`, `icecream/`, `404.html`, one shared `styles.css`. Square corners, same palette
as the game.

## Deploy

`.github/workflows/deploy.yml` syncs the repo root to `s3://jamesdouthit-com-site/` excluding `500cardgame/*`,
then invalidates CloudFront. It assumes the role `github-site-deploy`, which does not exist yet. Add it to
`deploy/cfn/cicd.yaml` in the server repo next to `ClientDeployRole`, trusting `repo:JamesDo5/jamesdouthitdotcom:ref:refs/heads/main`,
with `s3:ListBucket` on the bucket, `s3:PutObject` and `s3:DeleteObject` on `arn:aws:s3:::jamesdouthit-com-site/*`
with a `StringNotLike` condition denying `s3:prefix` / keys under `500cardgame/*`, and `cloudfront:CreateInvalidation`
on the distribution. Set `DISTRIBUTION_ID` in the workflow once the site stack exists.

Local preview: `python3 -m http.server 8080` from the repo root.
