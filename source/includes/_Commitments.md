## Commitments 
### Get Utilization 

**Summary:** Retrieves the utilization of an RI (Reserved Instance) or an SP (Savings Plan)

**Description:** The call is used to retrieve the utilization of RIs/SPs for a specific date.

<aside class="notice">
This request is applicable in AWS and Microsoft Azure accounts
</aside>

> Request Example: Getting a utilization


```shell
curl --location -g --request GET 'https://api.mypileus.io/api/v1/commitment/utilization/?date=2022-07-29&linkedAccount={{linked-account-id}}?commitmentService=ec2&commitmentType=ri' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| date | query | Report date (YYYY-MM-01) format | Yes | String |
| commitmentType | query | Reserved Instance or Savings Plan. Values: ri, sp | Yes | String |
| commitmentService | query | The service which the commitment is applied on. See the list of values in the note below. | Yes (only for AWS) | String |
| linkedAccount | query | Linked account ID only (Do not use the linked account name) | No | String |

<aside class="notice">
<b>CommitmentService</b> options:</br>
AWS RI: ec2, rds, elasticache, redshift, os (OpenSearch) </br>
AWS SP: EC2InstanceSavingsPlans, ComputeSavingsPlans</br>
Azure: The parameter is not required
</aside>

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
