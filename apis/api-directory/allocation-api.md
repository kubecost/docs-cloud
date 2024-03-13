# Allocation API

{% hint style="info" %}
Throughout our API documentation, we use `localhost:9090` as the default Kubecost URL, but your Kubecost instance may be exposed by a service or ingress. To reach Kubecost at port 9090, run: `kubectl port-forward deployment/kubecost-cost-analyzer -n kubecost 9090`. When querying the cost-model container directly (ex. localhost:9003), the `/model` part of the URI should be removed.
{% endhint %}

{% swagger method="get" path="/allocation" baseUrl="http://<your-kubecost-address>/model" summary="Allocation API" %}
{% swagger-description %}
The Allocation API is the preferred way to query for costs and resources allocated to Kubernetes workloads and optionally aggregated by Kubernetes concepts like `namespace`, `controller`, and `label`.

{% endswagger-description %}

{% swagger-parameter in="path" name="window" type="string" required="true" %}
Duration of time over which to query. Accepts words like `today`, `week`, `month`, `yesterday`, `lastweek`, `lastmonth`; durations like `30m`, `12h`, `7d`; comma-separated RFC3339 date pairs like `2021-01-02T15:04:05Z,2021-02-02T15:04:05Z`; comma-separated Unix timestamp (seconds) pairs like `1578002645,1580681045`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="aggregate" type="string" required="false" %}
Field by which to aggregate the results. Accepts: `cluster`, `namespace`, `controllerKind`, `controller`, `service`, `node`, `pod`, `label:<name>`, and `annotation:<name>`. Also accepts comma-separated lists for multi-aggregation, like `namespace,label:app`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="accumulate" type="boolean" required="false" %}
If `true`, sum the entire range of time intervals into a single set. Default value is `false`. Also supports accumulation by specific intervals of time including `hour`, `day`, and `week`. Does not accumulate idle costs, which must be configured separately.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="idle" type="boolean" required="false" %}
If `true`, include idle cost (i.e. the cost of the un-allocated assets) as its own allocation. (See [special types of allocation](#special-types-of-allocation).) Default is `true.`
{% endswagger-parameter %}

{% swagger-parameter in="path" name="external" type="boolean" required="false" %}
If `true`, include [external, or out-of-cluster costs](/install-and-configure/install/cloud-integration/README.md) in each allocation. Default is `false`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="offset" type="int" required="false" %}
Refers to the number of pages you are searching through which will increase by integers for the amount of pages you want to skip. Starting value is `0`, representing the first page of results.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="limit" type="int" required="false" %}
Refers to the number of line items per page. Pair with the `offset` parameter to filter your payload to specific sections of line items. You should also set `accumulate=true` to obtain a single list of line items, otherwise you will receive a group of line items per interval of time being sampled.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="filter" type="string" required="false" %}
Filter your results by any category which you can aggregate by, can support multiple filterable items in the same category in a comma-separated list. For example, to filter results by clusters A and B, use `filter=cluster:clusterA,clusterB` See our [Filter Parameters](/apis/apis-overview/filters-api.md) doc for a complete explanation of how to use filters and what categories are supported.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="format" type="string" required="false" %}
Set to `csv` to download an accumulated version of the allocation results in CSV format. Set to `pdf` to download an accumulated version of the allocation results in PDF format. By default, results will be in JSON format.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="format" type="string" required="false" %}
Cost metric format. Learn about cost metric calculations in our [Allocations Dashboard](/using-kubecost/navigating-the-kubecost-ui/cost-allocation/README.md) doc. Supports `cumulative`, `hourly`, `daily`, and `monthly`. Default is `cumulative`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="shareIdle" type="boolean" required="false" %}
If `true`, idle cost is allocated proportionally across all non-idle allocations, per-resource. That is, idle CPU cost is shared with each non-idle allocation's CPU cost, according to the percentage of the total CPU cost represented. Default is `false`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="splitIdle" type="boolean" required="false" %}
If `true`, and `shareIdle == false`, Idle Allocations are created on a per cluster or per node basis rather than being aggregated into a single "_idle_" allocation. Default is `false`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="idleByNode" type="boolean" required="false" %}
If `true`, idle allocations are created on a per node basis. Which will result in different values when shared and more idle allocations when split. Default is `false`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="includeSharedCostBreakdown" type="boolean" required="false" %}
Provides breakdown of cost metrics for any shared resources. Default is `true`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="reconcile" type="boolean" required="false" %}
If `true`, pulls data from the Assets cache and corrects prices of Allocations according to their related Assets. The corrections from this process are stored in each cost category's cost adjustment field. If the integration with your cloud provider's billing data has been set up, this will result in the most accurate costs for Allocations. Default is `true`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="shareTenancyCosts" type="boolean" required="false" %}
If `true`, share the cost of cluster overhead assets such as cluster management costs and node attached volumes across tenants of those resources. Results are added to the sharedCost field. Cluster management and attached volumes are shared by cluster. Default is `true`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="shareNamespaces" type="string" required="false" %}
Comma-separated list of namespaces to share; e.g. `kube-system, kubecost` will share the costs of those two namespaces with the remaining non-idle, unshared allocations.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="shareLabels" type="string" required="false" %}
Comma-separated list of labels to share; e.g. `env:staging, app:test` will share the costs of those two label values with the remaining non-idle, unshared allocations.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="shareCost" type="float" required="false" %}
Floating-point value representing a monthly cost to share with the remaining non-idle, unshared allocations; e.g. `30.42` ($1.00/day == $30.42/month) for the query `yesterday` (1 day) will split and distribute exactly $1.00 across the allocations. Default is `0.0.`
{% endswagger-parameter %}

{% swagger-parameter in="path" name="shareSplit" type="string" required="false" %}
Determines how to split shared costs among non-idle, unshared allocations. By default, the split will be `weighted`; i.e. proportional to the cost of the allocation, relative to the total. The other option is `even`; i.e. each allocation gets an equal portion of the shared cost. Default is `weighted`.
{% endswagger-parameter %}
