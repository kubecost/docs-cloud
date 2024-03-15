# Abandoned Workloads

{% swagger method="get" path="/savings/abandonedWorkloads" baseUrl="http://app.kubecost.com/query" summary="Abandoned Workloads API" %}
{% swagger-description %}
The Abandoned Workloads API suggests cluster workloads that have been abandoned based on network traffic levels.
{% endswagger-description %}

{% swagger-parameter in="path" name="days" type="int" %}
Number of historical days over which network traffic should be measured.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="threshold" type="int" %}
The threshold of total traffic (bytes in/out per second) at which a workload is determined abandoned.
{% endswagger-parameter %}
