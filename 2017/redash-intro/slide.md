<!-- $theme: gaia -->
<!-- *template: invert -->

re:dash
===  

#### [1ambda](https://github.com/1ambda)
#### ZEPL
##### May 2, 2017

---

<!-- page_number: true -->
<!-- *template: invert -->

### [re:dash](https://github.com/getredash/redash)

- ==SQL== is the first citizen
- [Features](https://redash.io/features/): Query / Visualize / Share
- Sass / OSS (6400+ stars)
  * [Plans](https://redash.io/pricing/)
  * Limited ==saved queries==
  * Limited ==dashboards== in Lite Plan
- Archiecture
  * uses ==postgres==, ==redis==, ==celery== for persistence, cache, distributed task queue

---

<!-- page_number: true -->
<!-- *template: invert -->

### Impressions

- has great ==onbording==:
  * noti, [tutorial](https://redash.io/help/getting_started/getting_started.html), [help center](https://redash.io/help/) (docs), [articles](https://blog.redash.io/)

- some nice (killer) features:
  * [alerts](http://demo.redash.io/alerts), sharing (slack, email)

- awesome ==setup guide== for OSS version including
  * provide [AMI](https://redash.io/help-onpremise/setup/setting-up-redash-instance.html) for all regions
  * provisioning script
  * OAuth support (google)

---

### Impressions (cont)

<!-- page_number: true -->
<!-- *template: invert -->

- focuses on ==Query== connecting to [20+ DBs](https://redash.io/integrations/)
  * JDBC, ElasticSearch, Cassandra, Graphite, MongoDB, influxDB, ...
  * ==stat== for queries (queue, outdated, ...)
  
- even intergrated with 
  * [GA](https://redash.io/help/data-sources/google-analytics.html#queries), Google Spreadsheets, [JIRA (JQL)](https://redash.io/help/queries/querying_jira_jql.html), [JSON](http://demo.redash.io/queries/4449/source#6268)...

- Enable to ==fork== existing queries, ==reuse== same query result to create multiple dashboards
- Some advanced vis: [sankey, sunburst](http://demo.redash.io/queries/2280#3113), [map](http://demo.redash.io/queries/580#800), [cohort](http://demo.redash.io/queries/3460#4624), [counter, pivot-table](http://demo.redash.io/queries/4448/source#6266)


---

### Summary

need to be improved

- UI, minor bugs
- hard to query in some backends (e.g [mongo](https://redash.io/help/queries/querying_mongodb.html))

however, really easy to
- search and re-use existing queries!
- share results (==slack==, ==email==) w/ **alert**!
- and has ==useful integrations==: GA, spreadsheets
- with great [doc](https://redash.io/help/), [setup guide](https://redash.io/help-onpremise/setup/setting-up-redash-instance.html): AMIs, [provision script](https://github.com/getredash/redash/blob/master/setup/ubuntu/bootstrap.sh)



---

<!-- *template: invert -->

### Resources

- site [re:dash online demo](http://demo.redash.io/queries/4448/source#6266)
- slide [re:dash is awesome](https://www.slideshare.net/toyama0919/redash-is-awesome)

---

<!-- *page_number: false -->

# Thanks

![bg](http://t1.daumcdn.net/friends/prod/editor/FRPBFRPSC0001_B_00.jpg)
