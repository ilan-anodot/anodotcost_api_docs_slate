## MSP Billing Rules

**Margin Type Names**

The following margin types are used in all Billing Rules requests.</br>
Use them to when creating billing rules and to understand existing billing rules.

| UI Name | API Value | Note |
| - | - | - |
| Percentage | update_cost_with_factor | |
| Fixed Cost | add_fixed_cost ||
| Fixed Rate | add_fixed_rate ||
| Custom Service Aggregated % | add_cost_with_factor ||
| AWS Support | add_aws_support | For AWS accounts only |
| Remove AWS Support | remove_aws_support | For AWS accounts only |
| Data Exclusion | remove_cost ||

### Get MSP Billing Rules

**Summary:** Retrieves list of MSP Billing Rules

**Description:** The call is used to retrieve list of billing rules for an MSP

> Request Example v2 (new): Get MSP Billing Relues

```shell
curl --location --request GET 'https://api.mypileus.io/api/v2/msp/billing-rules/v2' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

> Request Example v1 (Deprecated): Get MSP Billing Relues

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

> Request Example v2 (new): Create MSP Billing Rule

```shell
curl --location --request POST  
'https://api.mypileus.io/api/v2/msp/billing-rules/v2' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-Token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "scope":{
        "startMonth":"2024-11",
        "endMonth":"2024-11",
        "customerNames":["Customer5"],
        "level":"customer"
        },
    "filters":{
        "excludeFilters":{
            "costtype":["Credit"],
            "service":["AWS Support [Enterprise]"]
            },
        "excludeAwsMarketplace":true,
        "includeFilters":{
            "cloudfrontregion":["United States"],
            "region":["us-east-1"],
            "usagetype":["AP-DataTransfer-Out-OBytes"],
            "purchaseoption":["on demand","savings_plan"],
            "customtags":["Customer: no_tag"],
            "quantitytype":["Hours"],
            "chargetype":["Usage"],
            "familytype":["Compute"],
            "operation":["EC2 Compute"],
            "instancetype":["Amazon Elastic Compute Cloud"],
            "linkedaccid":["369369"]
            },
        "likeFilters":{
            "itemdescription":["aws"]
            }
        },
    "description":{
        "name":"rule name",
        "description":"rule description"
        },
    "margin":{
        "type":"add_aws_support",
        "resolution":"customer",
        "disableMinimumFee":true,
        "plan":"AWS Premium Support"
        }
    }
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

</br>

**Filtering Options**

| Filter Name | Type | Optional Operators | AWS | Azure | 
|-------------|------|--------------------|-----|-------|
| chargetype | list | is | Yes | | 
| cloudfrontregion | list | is | Yes | | 
| costtype | list | is | Yes | Yes | 
| customtags | ["key":"value"] | is | Yes | Yes | 
| Familytype | list | is | Yes | Yes (refers to 'meter category') | 
| instancetype | list | is | Yes | | 
| Linkedaccid | list | is | Yes | Yes (refers to 'subscription') | 
| Operation | list | is | Yes | Yes (refers to 'meter name') | 
| purchaseoption | list | is | Yes | Yes | 
| quantitytype | List - single value | is | Yes | Yes | 
| region | list | is | Yes | Yes | 
| service | list | is | Yes | Yes | 
| usagetype | list | is / Contains(LIKE) | Yes | Yes | 
| itemdescription | list | Contains (LIKE) | Yes || 
| excludeAwsMarketplace | Bool (true/false) | is | Yes | |
| Familytypeusage | list | is ||Yes (refers to 'sub meter category')| 


<aside class="notice">
<b>Note</b></br>
Depending on the marging type, filter availability might change. View it in the Anodot UI.
</aside>

> margin Example

```json
{
"margin":{
        "type":"add_aws_support",
        "resolution":"customer",
        "disableMinimumFee":true,
        "plan":"AWS Premium Support"
        }
}
```

**Margin type AWS support plan**

| Parameter | Description |
| ----------| ----------------|
| plan | Values are: </br>AWS Support [Developer]</br>AWS Support [Enterprise]</br>AWS Support [Business]</br>AWS Premium Support|
| disableMinimumFee | Boolean |
| resolution | Values are: Customer - total / link_account - Per linked account |
| Filters | See list of filters above |

</br>

**Margin type Remove AWS support**

| Parameter | Description |
| ----------| ----------------|
| plan | Values are: </br>AWS Support [Developer]</br>AWS Support [Enterprise]</br>AWS Support [Business]</br>AWS Premium Support|

</br>

**Margin type Fixed Cost**

| Parameter | Description |
| ----------| ----------------|
| customService | String. Represents the custom service name.|
| amount | Discount amount |

</br>

**Margin type Fixed Rate**

| Parameter | Description |
| ----------| ----------------|
| customService | String. Represents the custom service name.|
| amount | Discount amount |
| Filters | See list of filters above |

</br>

**Margin type Custom Service Aggregated %**

| Parameter | Description |
| ----------| ----------------|
| customService | String. Represents the custom service name.|
| factor | Decimal representation of a percentage discount/charge |
| Filters | See list of filters above |

</br>

**Margin type Percentage**

| Parameter | Description |
| ----------| ----------------|
| factor | Decimal representation of a percentage discount/charge |
| Filters | See list of filters above |

</br>

**Margin type Data Exclusion**

| Parameter | Description |
| ----------| ----------------|
| Filters | See list of filters above |


> Request Example v1 (Deprecated): Create MSP Billing Rule

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/msp/billing-rules' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-Token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
   "frequency": "recurring",
   "name": "Billing Rule 1",
   "linkedAccounts": "ALL",
   "customers": ["<customerNameId>"],
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

<aside class="notice">
Use the <i>customerNameId</i> field in the <i>customers</i> array.</br>
</aside>

**Responses**

> Response Example v2 (new)

```json
{
   "ruleId": "d2d71e2a-8e5c-4473-92f4-e0fecce30000",
   "orgId": "88",
   "accountKey": 19,
   "description": {
       "name": "rule name 1",
       "description": "rule description"
   },
   "margin": {
       "type": "add_aws_support",
       "disableMinimumFee": true,
       "plan": "AWS Premium Support",
       "resolution": "customer"
   },
   "filters": {
       "includeFilters": {
           "cloudfrontregion": [
               "United States"
           ],
           "region": [
               "us-east-1"
           ],
           "usagetype": [
               "AP-DataTransfer-Out-Bytes"
           ],
           "purchaseoption": [
               "on demand",
               "savings_plan"
           ],
           "customtags": [
               "Customer: no_tag"
           ],
           "quantitytype": [
               "Hours"
           ],
           "chargetype": [
               "Usage"
           ],
           "familytype": [
               "Compute"
           ],
           "operation": [
               "Windows"
           ],
           "instancetype": [
               "Amazon Elastic Compute Cloud"
           ],
           "linkedaccid": [
               "369369"
           ]
       },
       "excludeFilters": {
           "costtype": [
               "Credit"
           ],
           "service": [
               "AWS Support [Enterprise]",
               "AWS Premium Support"
           ]
       },
       "likeFilters": {
           "itemdescription": [
               "aws"
           ]
       },
       "excludeAwsMarketplace": true
   },
   "sourceTemplateId": null,
   "accountId": "369369",
   "scope": {
       "level": "customer",
       "customerNames": ["Customer5"],
       "linkedAccountIds": [],
       "startMonth": "2024-11",
       "endMonth": "2024-11"
   }
}
```

| Code | Description |
| ---- | ----------- |
| 201 | successful Creation |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Edit MSP Billing Rules

**Summary:** Update an MSP Billing Rule

**Description:** The call is used to update an existing MSP Billing Rule using its ID</br>
The billing rule ID is retrieved by the GET billing rules request.

> Request Example v2 (new) - Edit an MSP Billing Rule

```shell
curl --location --request PUT 'https://api-front.mypileus.io/api/v2/msp/billing-rules/v2' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-Token}}' \
--data '{
    "ruleId": "123123",
    "description": {
        "name": "test_v2",
        "description": null
    },
    "margin": {
        "type": "update_cost_with_factor",
        "factor": 0.14,
        "costType": "relative"
    },
    "scope": {
        "level": "customer",
        "customerNames": ["Customer5"],
        "startMonth": "2024-11",
        "endMonth": "2024-11"
    }
}'
```

> Request Example v1 (Deprecated) - Edit an MSP Billing Rule

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/msp/billing-rules/23454' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
   "frequency": "recurring",
   "name": "Billing Rule 1",
   "linkedAccounts": "ALL",
   "customers": ["<customer code>"],
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
| id | path | The billing rule ID to update | Yes |  |

**Responses**

> Response Example v2 (new)

```json
{
  "ruleId": "123123",
  "orgId": "432423",
  "accountKey": 432432,
  "description": { "name": "test_v2", "description": null },
  "margin": {
    "type": "update_cost_with_factor",
    "factor": 0.14,
    "costType": "relative"
  },
  "filters": {},
  "sourceTemplateId": null,
  "accountId": "***",
  "scope": {
    "level": "customer",
    "customerNames": ["Customer5"],
    "startMonth": "2024-11",
    "endMonth": "2024-11"
  }
}
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete MSP Billing Rules

**Summary:** Delete an MSP Billing Rule

**Description:** The call is used to delete an MSP Billing Rule using its ID</br>
The billing rule ID is retrieved by the GET billing rules request.

> Request Example v2 (New): Delete MSP Billing rule

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v2/msp/billing-rules/v2/{rule-id} \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
```

> Request Example v1 (Deprecated): Delete MSP Billing rule

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
| id | path | The billing rule Id to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful deletion |
| 400 | Invalid Parameters value |
| 500 | Server error |
