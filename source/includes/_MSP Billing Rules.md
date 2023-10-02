## MSP Billing Rules

### Get MSP Billing Rules

**Summary:** Retrieves list of MSP Billing Rules

**Description:** The call is used to retrieve list of billing rules for an MSP

> Request Example: Get MSP Billing Relues

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/msp/billing-rules' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

> Response Example: Get MSP Billing Rules

```json
[
   {
       "id": "2eee54d8",
       "name": "test1",
       "accountId": "932213950603",
       "creationDate": "2022-07-07",
       "frequency": "one time",
       "customers": "ALL",
       "effectiveMonth": "2022-06",
       "marginType": "custom-service-calc-cost",
       "margin": "20",
       "disableMinimumFee": false,
       "excludeAwsMarketplace": true,
       "customService": "Custom Service Name",
       "regions": [
           "af-south-1"
       ],
       "excludeService": true,
       "usageTypes": "ALL",
       "usageTypeOperator": "IS",
       "cloudFrontRegions": [],
       "services": [
           "Amazon Athena",
           "Amazon CloudFront",
           "Amazon Detective"
       ]
   }
]
```


| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

The response is an array of rules for the MSP.

**Response Fields**

| Field | Description |
| ----- | ----------- |
| account_id | The payer account id |
| uuid | The event id |

### Create MSP Billing Rules

**Summary:** Create an MSP Billing Rule

**Description:** The call is used to create a new billing rule for an MSP

> Request Example: Create MSP Billing Rule

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/msp/billing-rules' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-Token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
   "frequency": "recurring",
   "name": "Billing Rule 1",
   "linkedAccounts": "ALL",
   "startMonth": "2022-06",
   "endMonth": "2022-07",
   "marginType": "calc-cost",
   "margin": -50,
   "excludeAwsMarketplace": false,
   "regions": "ALL",
   "disableMinimumFee": false,
   "excludeService": false,
   "usageTypes": "ALL",
   "usageTypeOperator": "IS",
   "cloudFrontRegions": [],
   "services": ["AWS Premium Support"]
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

> Response Example


| Code | Description |
| ---- | ----------- |
| 201 | successful Creation |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Edit MSP Billing Rules

**Summary:** Update an MSP Billing Rule

**Description:** The call is used to update an existing MSP Billing Rule

> Request Example - Edit an MSP Billing Rule

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/msp/billing-rules/23454' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
   "frequency": "recurring",
   "name": "Billing Rule 1",
   "linkedAccounts": "ALL",
   "startMonth": "2022-06",
   "endMonth": "2022-07",
   "marginType": "calc-cost",
   "margin": -50,
   "excludeAwsMarketplace": false,
   "regions": "ALL",
   "disableMinimumFee": false,
   "excludeService": false,
   "usageTypes": "ALL",
   "usageTypeOperator": "IS",
   "cloudFrontRegions": [],
   "services": ["AWS Premium Support"]
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The customer ID to update | Yes |  |

**Responses**


| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete MSP Billing Rules

**Summary:** Delete an MSP Billing Rule

**Description:** The call is used to delete an MSP Billing Rule

> Request Example: Delete MSP Billing rule

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/msp/billing-rules/234254' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw ''
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The customer Id to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful deletion |
| 400 | Invalid Parameters value |
| 500 | Server error |
