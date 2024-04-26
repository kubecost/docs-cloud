# Kubecost Cloud Release Notes

Kubecost Cloud does not release updates on a scheduled timeline like our self-hosted product. Instead, Kubecost Cloud product updates will be documented here periodically.

## 4/26/24

### New Features:
* Cloud Costs page: performance improvements through pagination and streamlined API calls
* Cloud Costs page: autocomplete now available for filters and aggregations
* Clusters page: split Cluster into Cluster Name and Agent ID
* Clusters page: enhance "Add Cluster" and "Update Cluster" modals
* Improve Azure cost export downloads to be faster and more resilient to failures

### Bug Fixes:
* Fixed issue where costs from the Kubernetes domain could not be added to Collections
* Fixed issue where the agent version was incorrectly formatted on the Clusters page
## 4/16/24

### New Features:

* Allocations page: new window and filter selectors, autocomplete now available for filters and aggregations
* Improved performance for complex queries

### Bug Fixes:
* Fixed issue where Cluster Status Notifications could not be configured
* Fixed issue where Contains(Prefix/Suffix) filters for labels would not return the expected results

## 3/6/24

### Bug Fixes:
* Fixed issue where Cloud Costs without resource IDs would be under-counted within a Collection

## 2/26/24

### New Features:

* Collections page allows users to create a single report containing both Kubernetes and cloud Costs
* Updated left navigation includes color palette and moves dashboards under 'Monitor'
* Kubecost Cloud agent now leverages 3rd party verified TLS certificates instead of self-signed certificates
* Improved accuracy of Load Balancer and Persistent Volume costs by reconciling each individual entity against cloud costs.

## 12/21/23

### New Features:
* Cluster Status Alerts: get notified when a Kubecost Cloud agent stops reporting
* New filtering options added on the Allocations page: Department/Environment/Owner/Product/Team. Filtering by any of the properties listed above is equivalent to filtering by the following allocation label names: “department”, “env”, “owner”, “app”, “team”. The label names are not customizable at the moment.

### Bug Fixes:
* Fixed issue where Cloud Cost reports could not be created directly from the Reports page
* Fixed issue where Allocation aggregation by Service would not return the expected results
* Fixed issue where Allocation aggregation by Department/Environment/Owner/Product/Team would not return the expected results. The aggregation options listed above will now aggregate by the following allocation label names: “department”, “env”, “owner”, “app”, “team”. The label names are not customizable at the moment.

## 12/8/23

### New features:

* Alerts page
    * Get alerted when there is a significant change in your spending
* Auto-send reports
    * Schedule .csv or .pdf reports directly to your email
* Support for OpenShift Clusters

### Bug fixes:

* Fixed issue where applying multiple filters in the Cloud Cost Explorer Page resulted in only the last filter being applied
* Fixed issue where an error with one of the savings insights would block access to the entire Savings page
* Fixed issue where Cloud Cost Reports with certain properties could not be saved correctly
* Fixed issue where Azure non-compute cloud items were not returned on the Cloud Cost Explorer page
* Fixed issue where the Assets page would fail to load due to NaN costs
