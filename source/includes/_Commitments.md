## Commitments 
### Get Utilization 

**Summary:** Retrieves the utilization of an RI (Reserved Instance)

**Description:** The call is used to retrieve the utilization of RIs for a specific date.

Note: Currently this is only supported in Microsoft Azure accounts.

> Request Example: Getting a utilization


```shell
curl --location -g --request GET 'https://api.mypileus.io/api/v1/commitment/utilization/?date=2022-07-29&linkedAccount={{linked-account-id}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| date | query | Report date (YYYY-MM-DD) format | Yes | String |
| linkedAccount | query | Linked account ID only (No linked account name) | No | String |

**Responses**
The response is a utilization object showing the utilization for each RI. 

> Response Example 

```json
[
    {
        "minUtilizationPercentage": "0.0",
        "maxUtilizationPercentage": "100.0",
        "usageDate": "2022-08-01T00:00:00",
        "accountId": "ABC-123",
        "usedHours": "560.3",
        "reservedHours": "650.0",
        "linkedAccountName": "LINKED_ACC_NAME",
        "linkedAccountId": "LINKED_ACC_ID",
        "avgUtilizationPercentage": "86.2",
        "reservationOrderId": "asxc1234526fssz",
        "reservationId": "1234-5676-23243-12123",
        "skuName": "Test_acv2_sa"
    },
]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |
