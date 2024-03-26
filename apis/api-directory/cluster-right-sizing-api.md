# Cluster Right-Sizing Recommendation API

Kubecost's Cluster Right-Sizing Recommendation API can monitor the resource utilization of your clusters and offer cost-effective right-sizing solutions.

{% swagger method="get" path="/savings/clusterSizingETL" baseUrl="http://app.kubecost.com/query" summary="Cluster Right-Sizing Recommendation API" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter required="false" in="path" name="window" type="string" %}
Duration of time over which to query. Accepts multiple formats including units of time, relative time units, and unix timestamps. See this section on [using the `window` parameter](/apis/api-directory/api-directory.md#using-the-window-parameter) for more information.
{% endswagger-parameter %}

{% swagger-parameter in="path" type="float in the range (0, 1]" name="targetUtilization" %}
Target CPU/RAM utilization which parallels environment profiles. For reference, Development should equal `.80`, Production should equal `.65`, and High Availability should equal `.5`. Also supports custom values within the range.
{% endswagger-parameter %}

{% swagger-parameter in="path" type="int" name="minNodeCount" %}
Minimum node count to be recommended which parallels environment profiles. For reference, Development should equal `1`, Production should equal `2`, and High Availability should equal `3`. Also supports custom values within the range.
{% endswagger-parameter %}

{% swagger-parameter in="path" type="boolean" name="allowSharedCore" %}
Whether you want to allow shared core node types to be included in your recommendation. Accepts `true` or `false`.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="architecture" type="string" %}
Accepts `x86` or `ARM`. Currently, `ARM` is only supported on AWS clusters.
{% endswagger-parameter %}

{% swagger-parameter in="path" type="boolean" name="spotNodes" %}
Whether you want to allow Spot node types to be included in your recommendation. Default is `false`.
{% endswagger-parameter %}
{% endswagger %}