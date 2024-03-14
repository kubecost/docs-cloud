# Filter Parameters

Kubecost Cloud supports an advanced filtering language for all APIs.

## Filter operators

The supported filter operators are:

* `:` Equality
  * For string fields (e.g. namespace): equality
  * For slice/array fields (e.g. services): slice contains at least one value equal (equivalent to `~:`)
  * For map fields (e.g. labels): map contains key (equivalent to `~:`)
* `!:` Inequality, or "not contains" if an array type
* `~:` Contains
  * For string fields: contains
  * For slice fields: slice contains at least one value equal (equivalent to `:`)
  * For map fields: map contains key (equivalent to `:`)
* `!~:` NotContains, inverse of `~:`
* `<~:` ContainsPrefix
  * For string fields: string starts with
  * For slice fields: slice contains at least one value that starts with
  * For map fields: map contains at least one key that starts with
* `!<~:` NotContainsPrefix, inverse of `<~:`
* `~>:` ContainsSuffix
  * For string fields: strings ends with
  * For slice fields: slice contains at least one value that ends with
  * For map fields: map contains at least one key that ends with
* `!~>:` NotContainsSuffix, inverse of `~>:`

Filter values are strings surrounded by `"`. Multiple values can be separated by commas `,`.

Individual filters can be joined by `+` (representing logical AND) or `|` (representing logical OR). To use `+` and `|` in the same filter expression, scope _must_ be denoted via `(` and `)`. See examples.

## Examples

Here are some example filters to see how the filtering language works:

* `namespace:"kubecost"+container:"cost-model"` Return only results that are in the `kubecost` namespace and are for the `cost-model` container.
* `cluster:"cluster-one"+label[app]:"cost-analyzer"` Return only results in cluster `cluster-one` that are labeled with `app=cost-analyzer`.
* `cluster!:"cluster-one"` Ignore results from cluster `cluster-one`
* `namespace:"kubecost","kube-system"` Return only results from namespaces `kubecost` and `kube-system`.
* `namespace!:"kubecost","kube-system"` Return results for all namespaces except `kubecost` and `kube-system`.

For example, in an Allocation query:

{% code overflow="wrap" %}
```
http://app.kubecost.com/query/allocation/query?window=1d&accumulate=true&aggregate=namespace&filter=cluster!:%22cluster-one%22
```
{% endcode %}

The format is essentially: `<filter field> <filter op> <filter value>`

```sh
curl -G 'app.kubecost.com/query/assets/query' \
    -d 'window=3d' \
    --data-urlencode 'filter=assetType:"disk"'
```

## Formal grammar and implementation

To see the filter language's formal grammar and lexer/parser implementation, check out OpenCost's [`pkg/filter21/ast`](https://github.com/opencost/opencost/tree/develop/core/pkg/filter/ast).