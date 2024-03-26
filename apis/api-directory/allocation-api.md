# Allocation API

{% swagger method="get" path="/allocation/query" baseUrl="http://app.kubecost.com/query" summary="Allocation API (Querying)" %}
{% swagger-description %}
The Allocation API is the preferred way to query for costs and resources allocated to Kubernetes workloads and optionally aggregated by Kubernetes concepts like `namespace`, `controller`, and `label`.
{% endswagger-description %}

{% swagger-parameter in="path" name="window" type="string" required="true" %}
Duration of time over which to query. Accepts multiple formats including units of time, relative time units, and unix timestamps. See this section on [using the `window` parameter](/apis/api-directory/api-directory.md#using-the-window-parameter) for more information.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="format" type="string" required="false" %}
File type when exporting query. Currently only supports `json`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="aggregate" type="string" required="false" %}
Field by which to aggregate the results. Accepts: `cluster`, `node`, `container`, `controller`, `controllerKind`, `namespace`, `pod`, `providerId`, `service`, `label:<name>`, `annotation:<name>`, `deployment`, `daemonset`, `statefulset`, `job`, `department`, `environment`, `owner`, or `product`. Also accepts comma-separated lists for multi-aggregation, like `namespace,label:app`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="accumulate" type="boolean" required="false" %}
If `true`, sum the entire range of time intervals into a single set. Default value is `false`. Does not accumulate idle costs, which must be configured separately.

{% endswagger-parameter %}

{% swagger-parameter in="path" name="idle" type="boolean" required="false" %}
If `true`, include idle cost (i.e. the cost of the un-allocated assets) as its own allocation. Default is `true.`
{% endswagger-parameter %}

{% swagger-parameter in="path" name="idleByNode" type="boolean" required="false" %}
If `true`, idle allocations are created on a per node basis. Otherwise, idle allocations are created on a per cluster basis. 

{% endswagger-parameter %}

{% swagger-parameter in="path" name="limit" type="int" required="false" %}
Refers to the number of line items per page. Currently, only supported together with `accumulate=true` to obtain a single list of line items.

{% endswagger-parameter %}

{% swagger-parameter in="path" name="filter" type="string" required="false" %}
Filter your results by any category which you can aggregate by, can support multiple filterable items in the same category in a comma-separated list. For example, to filter results by clusters A and B, use `filter=cluster:clusterA,clusterB` See our [Filter Parameters](/apis/filter-parameters.md) doc for a complete explanation of how to use filters and what categories are supported.
{% endswagger-parameter %}
{% endswagger %}



{% swagger method="get" path="/allocation/totals" baseUrl="http://app.kubecost.com/query" summary="Allocation API (Total)" %}
{% swagger-description %}
The Allocation API is the preferred way to query for costs and resources allocated to Kubernetes workloads. The `/totals` endpoint does not aggregate, and accepts only a window and an optional filter, returning a single total cost.

{% endswagger-description %}

{% swagger-parameter in="path" name="window" type="string" required="true" %}
Duration of time over which to query. Accepts multiple formats including units of time, relative time units, and unix timestamps. See this section on [using the `window` parameter](/apis/api-directory/api-directory.md#using-the-window-parameter) for more information.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="format" type="string" required="false" %}
File type when exporting query. Currently only supports `json`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="filter" type="string" required="false" %}
Filter your results by any category which you can aggregate by, can support multiple filterable items in the same category in a comma-separated list. For example, to filter results by clusters A and B, use `filter=cluster:clusterA,clusterB` See our [Filter Parameters](/apis/filter-parameters.md) doc for a complete explanation of how to use filters and what categories are supported.
{% endswagger-parameter %}
{% endswagger %}