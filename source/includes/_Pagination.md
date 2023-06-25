## Pagination

Today, Each API call is limited to returning **5000** rows. In case you need to get more results, you can use the pagination capability of the APIs


> Pagination Call Example: First Request

```json
{ 
  "data": "[item1, item2, … , item5000]",
   "nextToken": 2 
}
```


> Pagination Call Example: Calling with token=2

```json
{ 
  "data": "[item5001, item5002, … , item6000]",
   "nextToken": null 
}
```

In order to get the full date (in case **next_token** is not null), you need to make the same API request with **token=next_token_value** until **nexToken** returns with a null value.

The following APIs currently support pagination:

* [Cost and Usage](#get-cost-amp-usage)
* [Kubernetes Cost and Usage](#get-kubernetes-cost)
* [Assets](#get-cost-assets)

<aside class="warning">
Important - In order to use pagination, you need to use /v2 on the API calls instead of /v1.
</aside>

To use pagination the user needs to add an additional parameter called **token** to the query parameter. You do not need to attach **token** for the first call. 
However - you will see in the response of the relevant calls a field called **nextToken** (see example on the right)
