## Kubernetes

### Get Kubernetes cost 

**Summary:** Retrieves kubernetes cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve kubernetes data grouped and filtered by different values

> Request Example: Get Kubernetes Cost

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/kubernetes/cost-and-usage?groupBy=cluster&startDate=2022-05-01&endDate=2022-08-01&periodGranLevel=month&costUsageType=compute' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| groupBy | query | The group by value that will be used for the results. Possible values: Available values : cluster, region, linkedaccname, linkedaccid, namespace, node, instancetype, labels, customtags | Yes |  |
| startDate | query | Start date of the cost and usage data examination | Yes |  |
| endDate | query | End date of the cost and usage data examination | Yes |  |
| periodGranLevel | query | Granularity level of the period in the output data. Available values : month, week, day | Yes |  |
| costUsageType | query | Available valuye: Available values : compute, dataTransfer, storage | Yes |  |
| usageType | query | Available values : CPU, Memory | No |  |
| metricType | query | Available values : actual, requirements | No |  |
| isNetUnblended | query | Available values : true, false | No |  |
| isAmortized | query | Available values : true, false | No |  |
| isNetAmortized | query | Available values : true, false | No |  |
| wheres | query | conditions of type if x equals y when retrieving data. Available values : cluster, region, linkedaccname, linkedaccid, namespace, node, instancetype, labels, customtags | No |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data. Available values : cluster, region, linkedaccname, linkedaccid, namespace, node, instancetype, labels, customtags | No |  |
| excludeFilters | query | conditions of type if x not in (y,z,w) when retrieving data. Available values : cluster, region, linkedaccname, linkedaccid, namespace, node, instancetype, labels, customtags | No |  |

**Responses**

> Response example: Getting Kubernetes Cost

```json
{
    "data": [
        {
            "groupBy": "us-east-1:932213950603:cw-test-2",
            "usageDate": "2022-05",
            "accountId": "932213950603",
            "clusterName": "us-east-1:932213950603:cw-test-2",
            "totalCost": 61.9,
            "totalUsage": 253883145259.66
        },
        {
            "groupBy": "us-east-1:932213950603:my-cluster-name",
            "usageDate": "2022-05",
            "accountId": "932213950603",
            "clusterName": "us-east-1:932213950603:my-cluster-name",
            "totalCost": 7.74,
            "totalUsage": 31338272009.87
        }
    ],
    "nextPage": null
}
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Get Kubernetes cost per pod

**Summary:** Retrieves kubernetes cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve kubernetes Pod cost and usage data grouped

> Request Example: Getting Kubernetes cost per pod / cluster 

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/kubernetes/cost-and-usage/pod?startDate=2022-05-01&endDate=2022-08-01&periodGranLevel=month&clusterName=us-east-1:932213950603:cw-test-2' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| startDate | query | Start date of the cost and usage data examination | Yes |  |
| endDate | query | End date of the cost and usage data examination | No |  |
| clusterName | query | The cluster name that the pod running on. cluster name formatting should be in following format- [CLUSTER_REGION]:[CLUSTER_ACCOUNT_ID]:[CLUSTER_NAME] for example us-east-1:123456789012:prod-cluster | No |  |
| nodeName | query |  | No |  |
| podName | query | pod name formatting should be in following format- [POD_NAMESPACE]:[POD_NAME] for example kube-system:aws-node-k5jgh | Yes |  |

> Response Example: 

```json
{
    "data": [
        {
            "groupBy": "us-east-1:932213950603:cw-test-2",
            "usageDate": "2022-05",
            "accountId": "932213950603",
            "clusterName": "us-east-1:932213950603:cw-test-2",
            "totalCost": 61.9,
            "totalUsage": 253883145259.66
        },
        {
            "groupBy": "us-east-1:932213950603:my-cluster-name",
            "usageDate": "2022-05",
            "accountId": "932213950603",
            "clusterName": "us-east-1:932213950603:my-cluster-name",
            "totalCost": 7.74,
            "totalUsage": 31338272009.87
        }
    ]
}
```

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |
