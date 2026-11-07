# jamesdouthit.com

Static pages for the root of jamesdouthit.com. The 500 client is a separate repo and is published under
`/500cardgame/` by its own workflow; this repo never touches that prefix.

Pages: `index.html`, `mtg/`, `prose/`, `icecream/`, `404.html`, one shared `styles.css`. Square corners, same palette
as the game.

## Deploy

`.github/workflows/deploy.yml` syncs the repo root to `s3://jamesdouthit-com-site/` excluding `500cardgame/*`,
then invalidates CloudFront. It assumes the role `github-site-deploy`, defined in `deploy/cfn/cicd.yaml` in the
500CardGameServer repo: it may write anywhere in the bucket except under `500cardgame/`, and invalidate the
distribution `E24MLQ27TF1W8W`.

Local preview: `python3 -m http.server 8080` from the repo root.

## MTG Sealed Sims

`mtg/index.html` is hand-written. The four report pages under `mtg/100k-4player/` and `mtg/10k-17player/` are
generated: run `python3 scripts/publish_site.py --out <sim out dir> --site <this repo>` from the
MTGSealedTourneySim repo, then commit here. Card images load from Scryfall.
