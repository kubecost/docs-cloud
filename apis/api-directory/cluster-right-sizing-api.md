# Cluster Right-Sizing Recommendation API

Kubecost's Cluster Right-Sizing Recommendation API can monitor the resource utilization of your clusters and offer cost-effective right-sizing solutions.

{% swagger method="get" path="/clusterSizingETL" baseUrl="http://<your-kubecost-address>/model/savings" summary="Cluster Right-Sizing Recommendation API" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter required="false" in="path" name="range" type="string" %}
Duration of time to sample for cluster activity in order to provide recommendations. Accepts a numeral value with a unit of time `m` (minutes), `h`, `d`, or `w`. For example, to sample activity for 2 days, use `range=2d`. Also accepts comma-separated RFC3339 date pairs like `2021-01-02T15:04:05Z,2021-02-02T15:04:05Z`; and comma-separated Unix timestamp (seconds) pairs like `1578002645,1580681045`.
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