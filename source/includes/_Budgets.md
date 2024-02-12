## Budgets
### Get Budget 

**Summary:** Retrieves budgets for given user

**Description:** The call is used to retrieve budgets created by api-key defined user


> Request Example: Getting a budget (v1 Or v2)


```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/budgets' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

```shell
curl --location --request GET 'https://api.mypileus.io/api/v2/budgets' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**
The response is an array of budgets defined in the account. 

> Response Example 

```json
[
    {
        "budgetId": "f30de497a71f45b7a5dd5e0953bc43e6",
        "budget_amounts": [],
        "end_date": "2023-06-01 00:00:00",
        "creation_time": "2022-07-03 11:48:33",
        "user_key": "ff55ffaf-42d7-402d-95c8-bdd6488392f3",
        "budget_amount_type": "fixed",
        "budget_type": "recurring",
        "flexible": 1,
        "alerts": [
            {
                "budget_percent_to_alert_from": 80,
                "alert_granularity": [
                    "daily"
                ],
                "recipients": [
                    "yariv@anodot.com"
                ],
                "when_to_alert": [
                    "forecasted"
                ]
            }
        ],
        "filters": {
            "include": {
                "linkedaccname": [
                    "Anodot Customer Success"
                ]
            },
            "exclude": {}
        },
        "account_id": "340481513670",
        "start_date": "2022-06-01 00:00:00",
        "division_id": "0",
        "budget_amount": 50000,
        "is_active": 1,
        "account_key": "649",
        "budget_name": "Customer Success",
        "is_valid": "1"
    }
]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Create Budget

**Summary:** create a budget for a given user

**Description:** The call is used to create budgets created by api-key defined user

> Example: Create budget v1

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/budgets' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "budgetName": "string",
  "budgetAmount": 0,
  "budgetAmounts": [
    {
      "date": "string",
      "amount": 0
    }
  ],
  "budgetAlerts": [
    {
      "alertPercent": 0,
      "alertEmail": "string",
      "whenToAlert": [
        "actualUsage"
      ],
      "alertGranularity": [
        "daily"
      ]
    }
  ],
  "includeFilters": {
    "filter_name": [
      "linkedaccid"
    ]
  },
  "excludeFilters": {
    "filter_name": [
      "linkedaccid"
    ]
  },
  "budgetType": "expiring",
  "budgetAmountType": "fixed",
  "startDate": "string",
  "endDate": "string",
  "isFlexible": true,
  "budgetId": "string"
}'
```

> Example: Create Budget v2

```shell
curl --location --request POST 'https://api.mypileus.io/api/v2/budgets' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "budgetName": "test plan type",
  "budgetAmount": 999999,
  "budgetAmounts": [
    {
      "amount": 76923,
      "date": "2023-12"
    },
    {
      "amount": 76923,
      "date": "2024-1"
    },
    {
      "amount": 76923,
      "date": "2024-2"
    },
    {
      "amount": 76923,
      "date": "2024-3"
    },
    {
      "amount": 76923,
      "date": "2024-4"
    },
    {
      "amount": 76923,
      "date": "2024-5"
    },
    {
      "amount": 76923,
      "date": "2024-6"
    },
    {
      "amount": 76923,
      "date": "2024-7"
    },
    {
      "amount": 76923,
      "date": "2024-8"
    },
    {
      "amount": 76923,
      "date": "2024-9"
    },
    {
      "amount": 76923,
      "date": "2024-10"
    },
    {
      "amount": 76923,
      "date": "2024-11"
    },
    {
      "amount": 76923,
      "date": "2024-12"
    }
  ],
  "endDate": "2024-12-26 00:00:00",
  "startDate": "2023-12-26 00:00:00",
  "isRelativeAlerts": true,
  "budgetAlerts": [
    {
      "whenToAlert": [
        "forecasted"
      ],
      "alertGranularity": [
        "monthly"
      ],
      "alertPercent": "88",
      "alertEmail": "demo@pileuscloud.com"
    }
  ],
  "costType": "net_amortized",
  "budgetId": "8b3c7f804dbe4638b057d3dc0b000000",
  "description": "",
  "isFlexible": 1,
  "excludeFilters": {},
  "includeFilters": {},
  "budgetType": "expiring",
  "budgetAmountType": "planned",
  "period": 0,
  "userKey": "82f99b85-e85a-436e-a8eb-d4ed98000000"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Budget Fields**

| Name | Type | Description |
|------|------|-------------|
| budgetName | String | The name in the Anodot Cost UI |
| costType | ENUM | Valid values: “amortized”,”net_amortized”,”unblended”|
| budgetType | ENUM | Valid values: “recurring”, “expiring” |
| budgetAmountType | ENUM | Valid values: “fixed”,”planned” |
| budgetAmount |  Number | The total budget |
| budgetAmounts | Object Array |  An array of amount and month pairs. Example: {"amount": 1234 ,"date": "2024-2"},{"amount": 5000 ,"date": "2024-3"} | 
| isFlexible | Depcrecated | Do not use this parameter |  
| isRelativeAlerts | Deprecated | Do not use this parameter |
| period | ENUM | Valid values: 0-"Monthly", 1-"Quarterly", 2-"Yearly" |
| startDate | date in string format | Example: “2024-02-11T09:25:29.800Z” |
| endDate | date in string format| Example: "2025-02-11T09:25:29.809Z" |
| includeFilters | object of filter keys (Optional) | Example: {"region": ["af-south-1"]} |
| excludeFilters | Object of filter keys (Optional) | Example: {"chargetype": ["Tax"]} |
| budgetAlerts | Array (Optional) | An array of alerts defined over the budget |

**budgetAlerts fields**

| Name | Type | Description |
|------|------|-------------|
| whenToAlert | Array | Valid values: “forecasted", ”actualUsage” of both |
| alertGranularity | Array | Valid values: ”monthly”, “daily”, “total” or combinations of them |
| alertPercent | String holding a number | Budget percent utilization thereshold |
| alertEmail | String | valid email addresses, separated by commas |



<aside class="notice">
<code>budgetType</code> and <code>budgetAmountType</code> create 3 valid combinations:</br>
1. budgetType: “recurring” and budgetAmountType: “fixed” = “Fixed monthly”</br>
2. budgetType: “expiring” and budgetAmountType: “planned” = “Planned monthly”</br>
3. budgetType: “expiring” and budgetAmountType: “fixed” = “Fixed period”</br>
</br>

Option 2 (budgetType = “expiring” and budgetAmountType=”planned”)  will include an array of budgetAmounts for each month, otherwise this object will be empty.
</aside>


**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Edit Budget 

**Summary:** update budget for a given user

**Description:** The call is used to update budget created by the api-key defined user

> Request Example: Update a budget v1

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/budgets?budgetId={{budgetId}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "budgetName": "string",
  "budgetAmount": 0,
  "budgetAmounts": [
    {
      "date": "string",
      "amount": 0
    }
  ],
  "budgetAlerts": [
    {
      "alertPercent": 0,
      "alertEmail": "string",
      "whenToAlert": [
        "actualUsage"
      ],
      "alertGranularity": [
        "daily"
      ]
    }
  ],
  "includeFilters": {
    "filter_name": [
      "linkedaccid"
    ]
  },
  "excludeFilters": {
    "filter_name": [
      "linkedaccid"
    ]
  },
  "budgetType": "expiring",
  "budgetAmountType": "fixed",
  "startDate": "string",
  "endDate": "string",
  "isFlexible": true,
  "budgetId": "string"
}'
```

> Request Example: Update a budget v2

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v2/budgets?budgetId={{budgetId}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "budgetName": "test plan type",
  "budgetAmount": 999999,
  "budgetAmounts": [
    {
      "amount": 76923,
      "date": "2023-12"
    },
    {
      "amount": 76923,
      "date": "2024-1"
    },
    {
      "amount": 76923,
      "date": "2024-2"
    },
    {
      "amount": 76923,
      "date": "2024-3"
    },
    {
      "amount": 76923,
      "date": "2024-4"
    },
    {
      "amount": 76923,
      "date": "2024-5"
    },
    {
      "amount": 76923,
      "date": "2024-6"
    },
    {
      "amount": 76923,
      "date": "2024-7"
    },
    {
      "amount": 76923,
      "date": "2024-8"
    },
    {
      "amount": 76923,
      "date": "2024-9"
    },
    {
      "amount": 76923,
      "date": "2024-10"
    },
    {
      "amount": 76923,
      "date": "2024-11"
    },
    {
      "amount": 76923,
      "date": "2024-12"
    }
  ],
  "endDate": "2024-12-26 00:00:00",
  "startDate": "2023-12-26 00:00:00",
  "isRelativeAlerts": true,
  "budgetAlerts": [
    {
      "whenToAlert": [
        "forecasted"
      ],
      "alertGranularity": [
        "monthly"
      ],
      "alertPercent": "88",
      "alertEmail": "demo@pileuscloud.com"
    }
  ],
  "costType": "net_amortized",
  "budgetId": "{budgetId}",
  "description": "",
  "isFlexible": 1,
  "excludeFilters": {},
  "includeFilters": {},
  "budgetType": "expiring",
  "budgetAmountType": "planned",
  "period": 0,
  "userKey": "82f99b85-e85a-436e-a8eb-d4ed989b9456"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |


### Delete Budget

**Summary:** Delete budget for a given user

**Description:** The call is used to delete budget created by api-key defined user

> Request Example: Delete a budget v1

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/budgets?budgetId={{budget-id}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw ''
```

> Request Example: Delete a budget v2

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v2/budgets?budgetId={{budget-id}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw ''
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| budgetId | query | The budget Id to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |
