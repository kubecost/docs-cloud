# Container Request Right Sizing Recommendation API

{% swagger method="get" path="savings/requestSizingV2" baseUrl="http://<kubecost-address>/model/" summary="Container Request Right Sizing Recommendation API" %}
{% swagger-description %}
The container request right sizing recommendation API provides recommendations for [container resource requests](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) based on configurable parameters and estimates the savings from implementing those recommendations on a per-container, per-controller level. If the cluster-level resources stay static, then there may not be significant savings from applying Kubecost's recommendations until you reduce your cluster resources. Instead, your idle allocation will increase.
{% endswagger-description %}

{% swagger-parameter in="query" name="algorithmCPU" type="string" required="false" %}
The algorithm to be used to calculate CPU recommendations based on historical CPU usage data. Options are `max` and `quantile`. Max recommendations are based on the maximum-observed usage in `window`. Quantile recommendations are based on a quantile of observed usage in `window` (requires the `qCPU` parameter to set the desired quantile). Defaults to `max`. To use the `quantile` algorithm, the [ContainerStats Pipeline](/architecture/containerstats-pipeline.md) must be enabled.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="algorithmRAM" type="string" required="false" %}
Like `algorithmCPU`, but for RAM recommendations.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="qCPU" type="float in the range (0, 1]" required="false" %}
The desired quantile to base CPU recommendations on. Only used if `algorithmCPU=quantile`. **Note**: a quantile of `0.95` is the same as a 95th percentile.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="qRAM" type="float in the range (0, 1]" required="false" %}
Like `qCPU`, but for RAM recommendations.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="targetCPUUtilization" type="float in the range (0, 1]" required="false" %}
A ratio of headroom on the base recommended CPU request. If the base recommendation is 100 mCPU and this parameter is `0.8`, the recommended CPU request will be `100 / 0.8 = 125` mCPU. Defaults to `0.7`. Inputs that fail to parse (see [Go docs here](https://pkg.go.dev/strconv#ParseFloat)) will default to `0.7`.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="targetRAMUtilization" type="float in the range (0, 1]" required="false" %}
Calculated like `targetCPUUtilization`.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="minRecCPUMillicores" type="float" required="false" %}
Lower bound, in millicores, of the CPU recommendation. Defaults to 10. Be careful when modifying below 10 for the following reason. Kubernetes currently recommends a maximum of 110 pods per node. A 10m minimum recommendation allows close to that (if all nodes are single core) while also being a round number.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="minRecRAMBytes" type="float" required="false" %}
Lower bound, in bytes, of the RAM recommendation. Defaults to 20MiB (20 \* 1024 \* 1024).
{% endswagger-parameter %}

{% swagger-parameter in="query" name="window" required="true" type="string" %}
Required parameter. Duration of time over which to calculate usage. Supports days before the current time in the following format:

`3d`. Hourly windows are not currently supported. It's recommended to provide a window greater than `2d`. See the [API Directory](/apis/api-directory/api-directory.md) for more a more detailed explanation of valid inputs to `window`.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="filter" type="string" required="false" %}
A filter to reduce the set of workloads for which recommendations will be calculated.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="sortBy" type="string" required="false" %}
Column to sort the response by. Defaults to `totalSavings`. Options are `totalSavings`, `currentEfficiency`, `cpuRecommended`, `cpuLatest`, `memoryRecommended`, and `memoryLatest`.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="sortByOrder" type="string" required="false" %}
Order to sort by. Defaults to `descending`. Options are `descending` and `ascending`.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="includeLabelsAndAnnotations" type="boolean" required="false" %}
Displays all labels and annotations associated with each container request when set to `true`. Default is `false`.
{% endswagger-parameter %}