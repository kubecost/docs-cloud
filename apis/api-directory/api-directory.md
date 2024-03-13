# API Directory

{% hint style="info" %}
Before accessing Kubecost Cloud API endpoints, you must generate an API key value and a personal user token. See our [Accessing API Endpoints] guide for more information.
{% endhint %}

The following APIs are available in Kubecost Cloud:

## Monitoring APIs

* [Allocation API](/apis/api-directory/allocation-api.md)
* [Assets API](/apis/api-directory/assets-api.md)
* [Cloud Cost API](/apis/api-directory/cloud-cost-api.md)

## Savings APIs

* [Cluster Right-Sizing Recommendations API](/apis/api-directory/cluster-right-sizing-api.md)
* [Request Right-Sizing Recommendations API](/apis/api-directory/request-right-sizing-api.md)
* [Abandoned Workloads API](/apis/api-directory/abandoned-workloads-api.md)

## Using Kubecost APIs

### Using the `window` parameter

The `window` parameter exists for all monitoring APIs as well as select savings APIs which usually refers to the date range of data for Kubecost to sample. Acceptable formats for using `window` parameter include:

* Units of time (ex:"15m", "24h", "7d", "48h", etc.)

* Relative time references (ex: "today", "yesterday", "week", "month", "lastweek", "lastmonth")

* Start and end unix timestamps (ex: "1586822400,1586908800")

* Start and end UTC RFC3339 pairs (ex: "2020-04-01T00:00:00Z,2020-04-03T00:00:00Z")