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

## Who can see a report: reports.registry.json

A report page is only ever published to the public GitHub Pages site if it's listed here with
`"visibility": "Public"` or `"Both"`. Any page not listed defaults to `Internal` - nothing goes
public by accident just because this file wasn't updated.

```json
{
  "reports": [
    { "slug": "finance/overview", "title": "Finance overview", "visibility": "Public" }
  ]
}
```

`slug` is the page's path under `pages/`, without the `.md` extension (e.g. `pages/finance/overview.md`
-> `finance/overview`). The home page (`pages/index.md`) is always shipped everywhere regardless
of the registry, since it's not meaningful to hide the site's own root.

Internal-only reports are still fully built and still visible in the internal DataPipelines.Web
app - the registry only decides what additionally leaves the internal network.

## Deployment

Building and publishing (both into the internal DataPipelines.Web app and to this repo's
public `gh-pages` branch for GitHub Pages) is handled entirely by `DataPipelines.Deploy` (a
console app in the DataPipelines repo) - there is no CI/CD workflow in this repo. The database
is only reachable from the deploy box, so the build must run there.
