# Kubecost Cloud: Cloud Billing Integrations

Integration with cloud service providers (CSPs) via their respective billing APIs allows Kubecost to display out-of-cluster (OOC) costs (e.g. AWS S3, Google Cloud Storage, Azure Storage Account). Additionally, it allows Kubecost to reconcile Kubecost's in-cluster estimates with actual billing data to improve accuracy.

To learn more about how Kubecost provides accurate cost data or how to manage existing cloud integrations, read below. Otherwise, see any of the following articles to get started on integrating a specific CSP:

* [Kubecost Cloud AWS Integration](/cloud-billing-integrations/aws-cloud-integration.md)
* [Kubecost Cloud GCP Integration](/cloud-billing-integrations/gcp-cloud-integration.md)
* [Kubecost Cloud Azure Integration](/cloud-billing-integrations/azure-cloud-integration.md)

## Reconciliation

Reconciliation matches in-cluster assets with items found in the billing data pulled from the CSP. This allows Kubecost to display the most accurate depiction of your in-cluster spending. Additionally, the reconciliation process creates `Network` assets for in-cluster nodes based on the information in the billing data. The main drawback of this process is that the CSPs have between a 6 to 24-hour delay in releasing billing data, and reconciliation requires a complete day of cost data to reconcile with the in-cluster assets. This requires a 48-hour window between resource usage and reconciliation. If reconciliation is performed within this window, asset cost is deflated to the partially complete cost shown in the billing data.

Cost-based metrics are based on on-demand pricing unless there is definitive data from a CSP that the node is not on-demand. This way estimates are as accurate as possible. If a new reserved instance is provisioned or a node joins a savings plan:

1. Kubecost continues to emit on-demand pricing until the node is added to the cloud bill.
2. Once the node is added to the cloud bill, Kubecost starts emitting something closer to the actual price.
3. For the time period where Kubecost assumed the node was on-demand but it was actually reserved, reconciliation adjusts the price to reflect the actual cost.

{% hint style="info" %}
The reconciled assets will inherit the labels from the corresponding items in the billing data. If there exist identical label keys between the original assets and those of the billing data items, the label value of the original asset will take precedence.
{% endhint %}

## Managing cloud integrations

You can view your existing cloud integrations and their success status in the Kubecost UI by visiting _Settings_, then scrolling to Cloud Integrations. To create a new integration or learn more about existing integrations, select _View additional details_ to go to the Cloud Integrations page.

Here, you can view your existing integrations. For non-successful integrations, Kubecost will display a diagnostic error message in the Status column to contextualize steps toward successful integration.

![Example successful and failed cloud integrations](/images/integrations.png)

Select an individual integration to view a side panel that contains details about that integration. You will be able to select _Edit_ to adjust any details about your integration that were configured during the original setup. You can also delete an integration by selecting the red trash can icon next to the name of the integration.
