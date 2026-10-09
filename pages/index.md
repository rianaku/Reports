---
title: Reports
---

Welcome. This is the home page for DataPipelines reports, built with [Evidence](https://evidence.dev).

Add new pages as `.md` files under `pages/`. Each page can run SQL directly against the
`datapipelines` source (see `sources/datapipelines/connection.yaml`):

```sql orders_by_day
select
  date_trunc('day', created_at) as day,
  count(*) as orders
from orders
group by 1
order by 1
```

<LineChart data={orders_by_day} x=day y=orders />
