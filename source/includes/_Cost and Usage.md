## Cost & Usage
### Get Cost & Usage 

**Summary:** Retrieves cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve cloud invoice data grouped and filters by different values

> Request Example - Getting cost and usage

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/invoices/cost-and-usage?groupBy=service&startDate=2021-12-31&endDate=2022-07-01&periodGranLevel=month&costType=discount' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| groupBy | query | The group by value that will be used for the results | Yes |  |
| startDate | query | Start date of the cost and usage data examination | Yes |  |
| endDate | query | End date of the cost and usage data examination | Yes |  |
| periodGranLevel | query | Granularity level of the period in the output data | Yes |  |
| costType | query | Type of cost to get the report for. Possible options are: cost, discount, refund, credit | Yes |  |
| isNetUnblended | query |  | No |  |
| isAmortized | query |  | No |  |
| isNetAmortized | query |  | No |  |
| wheres | query | conditions of type if x equals y when retrieving data | No |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data | No |  |
| excludeFilters | query | conditions of type if x not in (y,z,w) when retrieving data | No |  |


### groupBy and filtering options

The table provides a few examples on the way to use the filters and groupBy parameters

Analysis | Filter | groupBy |
------------------|--------|---------|
Service |filters%5Bservice%5D |service|
Region  |filters%5Bregion%5D |region|
Linked Account|filters%5Blinkedaccid%5D|linkedaccid|
Tags|filters%5Bcustomtags%5D=Tag_Key3A%20Tag_Value&|customtags%3ATag_Key|
Cost Center|filters%5Bdivision%5D|division|
Sub Views|filters%5Bsubviewscustomtags%5D|viewscustomtags%3AViewName|
Virtual Tags|filters%5Bvirtualcustomtags%5D|virtualcustomtags%3AVirtualTagName|
Quantity Type|filters%5Bquantitytype%5D||
Operation|filters%5Boperation%5D|operation|
Purchase Option|filters%5Bpurchaseoption%5D|purchaseoption|
Family Type|filters%5Bfamilytype%5D|familytype|
Instance Type|filters%5Binstancetype%5D|instancetype|
Sub Service|filters%5Bsubservice%5D||
Charge Type|filters%5Bchargetype%5D||
Filter Group|filters%5Bcategories%5D|category|
Business Mapping|filters%5Bbusinessmapping%5D|businessmapping|
Usage Type|filters%5Busagetype%5D|usagetype|
Availability Zone|filters%5Bzonetag%5D|zonetag|
Resource|filters%5Bresourceid%5D|resourceid|


> Response Example 

```json
[
    {
        "group_by": "undefined",
        "usage_date": "2022-05",
        "account_id": "932213950603",
        "total_cost": 36564.952178,
        "total_usage_quantity": 1096079113.54,
        "resource_name": "",
        "resource_id": ""
    },
    {
        "group_by": "undefined",
        "usage_date": "2022-06",
        "account_id": "932213950603",
        "total_cost": 41454.924133,
        "total_usage_quantity": 1247146351.53,
        "resource_name": "",
        "resource_id": ""
    },
    {
        "group_by": "undefined",
        "usage_date": "2022-07",
        "account_id": "932213950603",
        "total_cost": 75231.838587,
        "total_usage_quantity": 2346800877.42,
        "resource_name": "",
        "resource_id": ""
    },
    {
        "group_by": "undefined",
        "usage_date": "2022-08",
        "account_id": "932213950603",
        "total_cost": 11307.24461,
        "total_usage_quantity": 141920221.13,
        "resource_name": "",
        "resource_id": ""
    }
]
```

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |
