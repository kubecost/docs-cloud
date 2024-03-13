# Assets API

{% swagger method="get" path="query/assets/query" baseUrl="http://app.kubecost.com/model" summary="Assets API (Querying)" %}
{% swagger-description %}
The Assets API retrieves backing cost data broken down by individual assets in your cluster but also provides various aggregations of this data.
{% endswagger-description %}

{% swagger-parameter in="path" name="window" required="true" type="string" %}
Duration of time over which to query. Accepts multiple formats including units of time, relative time units, and unix timestamps. See this section on [using the `window` parameter](/apis/api-directory/api-directory.md#using-the-window-parameter) for more information.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="format" type="string" required="false" %}
File type when exporting query. Currently only supports `json`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="aggregate" type="string" required="false" %}
Used to consolidate cost model data. Supported values are `account`, `cluster`, `project`, `providerid`, `provider`, and `type`. Passing an empty value for this parameter or none at all returns data by an individual asset. Supports multi-aggregation (aggregation of multiple categories) in a comma separated list, such as `aggregate=account,project`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="accumulate" type="boolean" required="false" %}
When set to `false`, this endpoint returns daily time series data vs cumulative data. Default value is `false`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="limit" type="int" required="false" %}
Refers to the number of line items per page. Pair with the `offset` parameter to filter your payload to specific sections of line items. You should also set `accumulate=true` to obtain a single list of line items, otherwise you will receive a group of line items per interval of time being sampled. Paginates by all five item types, and will provide lists for all five types.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="filter" type="string" required="false" %}
Filter your results by any category which you can aggregate by, can support multiple filterable items in the same category in a comma-separated list. For example, to filter results by clusters A and B, use `filter=cluster:clusterA,clusterB` See our [Filter Parameters](/apis/apis-overview/filters-api.md) doc for a complete explanation of how to use filters and what categories are supported.
{% endswagger-parameter %}