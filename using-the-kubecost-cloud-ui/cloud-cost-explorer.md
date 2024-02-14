# Cloud Cost Explorer

The Cloud Cost Explorer is a dashboard which provides visualization and filtering of your cloud spending. This dashboard includes the costs for all assets in your connected cloud accounts by pulling from those providers' Cost and Usage Reports (CURs) or other cloud billing reports.

For help on integrating one or several cloud service providers (CSPs), see the corresponding documentation:

* [Kubecost Cloud AWS Integration](/cloud-billing-integrations/aws-cloud-integration.md)
* [Kubecost Cloud GCP Integration](/cloud-billing-integrations/gcp-cloud-integration.md)
* [Kubecost Cloud Azure Integration](/cloud-billing-integrations/azure-cloud-integration.md)

## UI Overview

### Date range

You can adjust your displayed metrics using the date range feature, represented by _Last 7 days_, the default range. This will control the time range of metrics that appear. Select the date range of the report by setting specific start and end dates, or by using one of the preset options.

### Aggregate filters

You can adjust your displayed metrics by aggregating your cost by category. Supported fields are _Account, Provider, Invoice Entity, Service_, _Item_, as well as custom labels. The Cloud Cost Explorer dashboard supports single and multi-aggregation. See the table below for descriptions of each field.

<table><thead><tr><th width="172" align="center">Aggregation</th><th>Description</th></tr></thead><tbody><tr><td align="center">Account</td><td>The ID of the billing account your cloud provider bill comes from. (ex: AWS Management/Payer Account ID, GCP Billing Account ID, Azure Billing Account ID)</td></tr><tr><td align="center">Provider</td><td>Cloud service provider (ex: AWS, Azure, GCP)</td></tr><tr><td align="center">Invoice Entity</td><td>Cloud provider account (ex: AWS Account, Azure Subscription, GCP Project)</td></tr><tr><td align="center">Service</td><td>Cloud provider services (ex: S3, microsoft.compute, BigQuery)</td></tr><tr><td align="center">Item</td><td>Individual items from your cloud billing report(s)</td></tr><tr><td align="center">Labels</td><td>Labels/tags on your cloud resources (ex: AWS tags, Azure tags, GCP labels)</td></tr></tbody></table>

### Edit

Selecting the _Edit_ button will allow for additional filtering and pricing display options for your cloud data.

#### Add filters

You can filter displayed dashboard metrics by selecting _Edit_, then adding a filter. Filters can be created for the following categories (see descriptions of each category in the Aggregate filters table above):

* Service
* Account
* Invoice Entity
* Provider
* Labels

**Cost Metric**

The Cost Metric dropdown allows you to adjust the displayed cost data based on different calculations. Cost Metric values are based on and calculated following standard FinOps dimensions and metrics, but may be calculated differently depending on your CSP. Learn more about how these metrics are calculated by each CSP in the [Cost metrics by CSP](cloud-cost-explorer.md#cost-metrics-by-csp) section below. The five available metrics supported by the Cloud Costs Explorer are:

<table><thead><tr><th width="201">Cost Metric</th><th>Description</th></tr></thead><tbody><tr><td>Amortized Net Cost</td><td>Net Cost with removed cash upfront fees and amortized (default)</td></tr><tr><td>Net Cost</td><td>Costs inclusive of discounts and credits. Will also include one-time and recurring charges.</td></tr><tr><td>List Cost</td><td>CSP pricing without any discounts</td></tr><tr><td>Invoiced Cost</td><td>Pricing based on usage during billing period</td></tr><tr><td>Amortized Cost</td><td>Effective/upfront cost across the billing period</td></tr></tbody></table>

### Additional options

Selecting _Save_ will save your current Cloud Costs query for access on the Reports page.

Select the three horizontal dots icon to view additional options. Select _Open Report_ to open any existing Cloud Costs reports. Selecting _Download CSV_ will download the current query as a CSV file.

## Cost metrics by CSP

The current Cloud Cost schema is optimistic in that it provides space for cost metrics that may not yet be available from some providers. As the FOCUS Spec gains more adoption among CSPs, all fields will be populated with values that match the definitions. For now, some values on certain providers are being populated with their nearest approximate. This section outlines how each value is populated on each CSP.

<details>

<summary>AWS cost metrics</summary>

Of all billing exports and APIs, the Cost and Usage Report (CUR) has the most robust set of cost metrics, and currently has the best support. Depending on what kind of discounts or resources a user has, the schema changes, therefore many of these columns are populated dynamically to support all users. In particular, any `_net_` column will only be available if the user has a discount that causes it to exist. Additionally, Kubecost currently only considers line items that have a `line_item_line_item_type` of `Usage`, `DiscountUsage`, `SavingsPlanCoveredUsage`, `EdpDiscount`, or `PrivateRateDiscount`.

More information on the columns and their definitions can be found in AWS' [Line item details](https://docs.aws.amazon.com/cur/latest/userguide/Lineitem-columns.html) documentation.

**List Cost**

To populate list price, Kubecost uses `pricing_public_on_demand_cost`.

**Net Cost**

Kubecost uses `line_item_net_unblended_cost` if available. If not, Kubecost uses `line_item_unblended_cost.`

**Amortized Net Cost**

If `_net_` is not available, Kubecost uses Amortized Cost

If `line_item_line_item_type` is `DiscountUsage`, Kubecost uses `reservation_net_effective_cost`.

If `line_item_line_item_type` is `SavingsPlanCoveredUsage`, Kubecost uses `savings_plan_net_savings_plan_effective_cost`.

Default to `line_item_net_unblended_cost`.

**Invoiced Cost**

Kubecost uses Net Cost.

**Amortized Cost**

If `line_item_line_item_type` is `DiscountUsage`, Kubecost uses `reservation_effective_cost`.

If `line_item_line_item_type` is `SavingsPlanCoveredUsage`, Kubecost uses `savings_plan_savings_plan_effective_cost`.

Default to `line_item_unblended_cost`.

</details>

<details>

<summary>GCP cost metrics</summary>

Cloud Cost uses a detailed billing export accessed via BigQuery to interface with GCP. This export provides Kubecost with a Cost column with a float value in addition to an array of credit objects per item. These credits are various discounts applied to the item being referenced.

More details about the export can be found in GCP's [Structure of Detailed data export](https://cloud.google.com/billing/docs/how-to/export-data-bigquery-tables/detailed-usage).

**List Cost**

The Cost column for the line item.

**Net Cost**

The Cost column plus the sum of all credit amounts.

**Amortized Net Cost**

Kubecost uses Net Cost.

**Invoiced Cost**

Kubecost uses Net Cost.

**Amortized Cost**

Kubecost uses Net Cost.

</details>

<details>

<summary>Azure cost metrics</summary>

The Azure billing export can be set to amortized or not amortized during creation. Depending on this, either the Net Cost Metric or Amortized Net Cost metric will be accurate. Additionally the Azure export has multiple schema depending on when it was created and what kind of account the user has. There are also localized versions of the headers.

**List Cost**

Kubecost uses`paygcostinbillingcurrency` if available, otherwise Kubecost uses Net Cost

**Net Cost**

Kubecost uses `costinbillingcurrency`. If not available, Kubecost uses `pretaxcost`, and if that isn't available, Kubecost uses `cost`.

**Amortized Net Cost**

Kubecost uses Net Cost.

**Invoiced Cost**

Kubecost uses Net Cost.

**Amortized Cost**

Kubecost uses Net Cost.

</details>