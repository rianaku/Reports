# Reports

[Evidence](https://evidence.dev) reports, built from SQL + Markdown against the DataPipelines
Postgres database. This uses the classic, self-hosted `@evidence-dev/evidence` npm package
(not the newer Evidence Studio CLI) - no cloud account, no login, builds to a static site.

## Local development

```
npm install --legacy-peer-deps
cp .env.example .env   # fill in EVIDENCE_SOURCE__datapipelines__user / password
npm run dev
```

## Adding a report

Add a `.md` file under `pages/`. Each page can embed a SQL block that queries the
`datapipelines` source (see `sources/datapipelines/connection.yaml`) and chart/table
components that reference it. See `pages/index.md` for an example.

## Building

```
npm run build
```

Outputs a static site to `build/`.

## Deployment

Building and publishing (both into the internal DataPipelines.Web app and to this repo's
public `gh-pages` branch for GitHub Pages) is handled entirely by `Deploy.ps1` in the
DataPipelines repo - there is no CI/CD workflow in this repo. The database is only reachable
from the deploy box, so the build must run there.
