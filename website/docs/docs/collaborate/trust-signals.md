---
title: "Trust signals"
sidebar_label: "Trust signals"
id: trust-signals
description: "Learn about dbt Explorer and how to interact with it to understand, improve, and leverage your dbt projects."
image: /img/docs/collaborate/dbt-explorer/trust-signal-health.jpg
---

# Trust signals <Lifecycle status="preview" />

Trust signal icons offer a quick, at-a-glance view of data health when browsing your resources in dbt Explorer. These icons keep you informed on the status of your resource's health using the indicators **Healthy**, **Caution**, **Degraded**, or **Unknown**.

- Supported resources are [models](/docs/build/models), [sources](/docs/build/sources), and [exposures](/docs/build/exposures).
- For accurate health data, ensure the resource is up-to-date and had a recent job run.
- Each trust signal icon reflects key data health components, such as test success status, missing resource descriptions, missing tests, absence of builds in 30-day windows, [and more](#trust-signal-criteria)

<Lightbox src="/img/docs/collaborate/dbt-explorer/trust-signal-health.jpg" width="55%" title="View trust signals for your models."/> 

## Access trust signals

Access trust signal icons in the following places:
- [Search function](/docs/collaborate/explore-projects#search-resources) or click on **Models**, **Sources** or **Exposures** under the **Resource** tab. 
   - For sources, the trust signal also indicates the source freshness status.
- View the icons under the **Health** column in [each resource's details page](#view-resource-details). Hover over or click the trust signal to view detailed information.
- Public models tables under the **Health** column.
- DAG lineage graph 


<Lightbox src="/img/docs/collaborate/dbt-explorer/icons-trust-signal.gif" width="95%" title="Access trust signal icons in multiple places in dbt Explorer."/> 

## Trust signal criteria

Each resource has a health state that is determined by specific set of criteria. Select the following tabs to view the criteria for that resource type.
<Tabs>
<TabItem value="models" label="Models">

The health state of a model is determined by the following criteria:

| **Health state** | **Criteria**   |
|-------------------|---------------|
| ✅ **Healthy**    | - Built successfully in the last run AND<br />- Built in the last 30 days AND<br />- Model has tests configured AND<br />- All tests passed AND<br />- All upstream sources are fresh or freshness is not applicable (set to `null`) AND<br />- Has a description |
| 🟡 **Caution**   | - Not built in the last 30 days OR<br />- Tests are not configured OR<br />- Tests return warnings OR<br />- One or more upstream sources are stale:<br />&nbsp;&nbsp;&nbsp;&nbsp;- Has a freshness check configured<br />&nbsp;&nbsp;&nbsp;&nbsp;- Freshness check ran in the past 30 days<br />&nbsp;&nbsp;&nbsp;&nbsp;- Freshness check returned a warning<br />- Missing a description |
| 🔴 **Degraded**  | - Model failed to build OR<br />- Model has failing tests OR<br />- One or more upstream sources are stale:<br />&nbsp;&nbsp;&nbsp;&nbsp;- Freshness check hasn’t run in the past 30 days<br />&nbsp;&nbsp;&nbsp;&nbsp;- Freshness check returned an error |
| ⚪ **Unknown**    | - Unable to determine health of resource; ingestion hasn't run           |

</TabItem>

<TabItem value="sources" label="Sources">

The health state of a source is determined by the following criteria:

| **Health state** | **Criteria**   |
|-------------------|---------------|
| ✅ Healthy	| - Freshness check configured AND<br />- Freshness check passed AND<br />- Freshness check ran in the past 30 days AND<br />- Has a description |
| 🟡 Caution	| - Freshness check returned a warning OR<br />- Freshness check not configured OR<br />- Freshness check not run in the past 30 days OR<br />- Missing a description |
| 🔴 Degraded	| - Freshness check returned an error |
| ⚪ Unknown	| Unable to determine health of resource; ingestion has not run |

</TabItem>

<TabItem value="exposures" label="Exposures">

The health state of an exposure is determined by the following criteria:

| **Health state** | **Criteria**   |
|-------------------|---------------|
| ✅ Healthy	| - Underlying sources are fresh AND<br />- Underlying models built successfully AND<br />- Underlying models’ tests passing AND<br />- (TBD) Underlying models built in the last 30 days |
| 🟡 Caution	| - At least one underlying source’s freshness checks returned a warning OR<br />- At least one underlying model was skipped OR<br />- At least one underlying model’s tests returned a warning OR<br />- (TBD) At least one model not built in the last 30 days |
| 🔴 Degraded	| At least one underlying source’s freshness checks returned an error OR<br />- At least one underlying model did not build successfully OR<br />- At least one model’s tests returned an error |

</TabItem>

<TabItem value="source-collection" label="Source collection health">

The health state of a source collection is determined by the following criteria:

Functions as an aggregate of underlying sources

| **Health state** | **Criteria**   |
|-------------------|---------------|
| ✅ Healthy	| - All underlying sources have freshness checks configured OR<br />- All passed their freshness checks OR<br />- All freshness checks ran in the past 30 days OR<br /> - All sources have a description |
| 🟡 Caution	| - One or more sources lack freshness checks OR<br />- One or more freshness checks returned a warning OR<br />- One or more freshness checks not run in the past 30 days OR<br />- One or more sources missing a description |
| 🔴 Degraded	| - One or more underlying sources’ freshness checks returned error |

</TabItem>

</Tabs>
