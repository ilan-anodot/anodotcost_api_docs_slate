## Assets 

### Get Cost Assets  

**Summary:** Retrieve the list of assets defined in the account

Notice that you can use this API to get 'regular' cost assets as well as Kubernetes assets. The possible combinations of the columns / cost types vary between the two use cases. 

> Request example: Getting assets 

```shell
curl --location --request GET 'https://api.mypileus.io/api/v2/usage/assets?startDate=2023-01-01&endDate=2023-01-15&isK8S=0&granLevel=day&columns=service&costType=cost&isUnblended=true' \
--header '{{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

> Request example: Getting K8S assets 

```shell
curl --location --request GET 'https://api.mypileus.io/api/v2/usage/assets?startDate=2022-12-15&endDate=2023-01-15&isK8S=1&granLevel=day&columns=region&costType=compute&costType=dataTransfer&costType=storage&isUnblended=true' \
--header '{{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| startDate | query | Start date for getting the data | Yes | date in YYYY-MM-DD format |
| endDate | query | Start date for getting the data | Yes | date in YYYY-MM-DD format |
| isK8S | query | A flag indicating whether to get 'regular' assets or Kubernets ones. | Yes | Bool. 1 = K8S assets, 0 = Regular Assets |
| granLevel | query | Granularity level of the period in the output data.  | Yes | enum. Possible values: month, week, day |
| columns | query | The different columns for the required assets | Yes |  
| costType | query | The different cost types for the required assets. Possible options are: cost, discount, refund, credit. If you are getting K8S assets, the possible options are: compute, dataTransfer, storage | Yes | list of values, e.g. costType=cost&costType=discount
| isUblended | query | Should the returned data be unblended or not | Yes | Bool (true / false)
| token | query | Pagination indicator. The next token to return in this call. This will be returned from the previous call to this API. | Optional | string

**Responses**

> Response Example - Cost Assets

```json
[
    {
        "usagedate": "2023-01-01",
        "payeraccount": "932213950603",
        "service": "Tax",
        "totalcost": 10644.48,
        "totalUsageQuantity": 51
    },
    {
        "usagedate": "2023-01-01",
        "payeraccount": "932213950603",
        "service": "Redshift-RI",
        "totalcost": 8291.4336,
        "totalUsageQuantity": 2976
    },
    {
        "usagedate": "2023-01-01",
        "payeraccount": "932213950603",
        "service": "AWS Support [Business]",
        "totalcost": 3539.337317,
        "totalUsageQuantity": 46276.2473789223
    },
    {
        "usagedate": "2023-01-15",
        "payeraccount": "932213950603",
        "service": "AWS Amplify",
        "totalcost": 0,
        "totalUsageQuantity": 3.2870904935
    }
]
```

The response is comprised of an array of assets, according to the following table:

| Name | Type | Description | 
| ---- | ---- | ----------- | 
| usageDate | date | ID of the cost center |
| payeraccount | String | Cost Center Name | 
| service | String | Cost Center Code (optional) |
| totalcost | Number | ID of the Anodot Cost Account | 
| totalUsageQuantity | Number | All the accounts linked to this cost center |

**Pagination information:**
</br>
If there is additional information to show, the *nextToken* field will be included in the response.</br>
The value should be used in the *token* parameter of the next call.



**Error Code**

| Code | Description |
| ---- | ----------- |
| 200 | User retrieval success |
| 404 | User not found |
| 500 | Server error |
