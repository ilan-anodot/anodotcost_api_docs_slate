## Budgets
### Get Budget 

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

**Summary:** Retrieves budgets for the given user

**Description:** The call is used to retrieve budgets created by api-key defined user

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

The response is an array of budgets defined in the account.</br>
V2 responses include additional fields that are available in the budget object.

> Response Example v1

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

> Response Example v2

```json
[{
  "endDate": "2025-01-29 00:00:00",
  "userKey": "82f99b85-e85a-436e-a8eb-d4ed98000000",
  "totalForecastedCost": "157404.18",
  "forcastedPrice": {
    "2024-02-21": "4641.44",
    "2024-02-20": "4641.26",
    "2024-02-23": "4641.74",
    "2024-02-22": "4641.6",
    "2024-02-14": "4639.87",
    "2024-02-25": "4641.97",
    "2024-02-13": "4639.58",
    "2024-02-24": "4641.86",
    "2024-02-16": "4640.4",
    "2024-02-27": "4642.14",
    "2024-02-15": "4640.14",
    "2024-02-26": "4642.07",
    "2024-02-18": "4640.87",
    "2024-02-29": "4642.26",
    "2024-02-17": "4640.64",
    "2024-02-28": "4642.21",
    "2024-02-19": "4641.07"
  },
  "rawFilters": {
    "excludefilters": [
      {
        "field": "chargetype",
        "values": "('Tax')",
        "forExclude": true
      }
    ],
    "includefilters": []
  },
  "budgetAmountType": "fixed",
  "budgetType": "recurring",
  "flexible": 1,
  "isAmortized": false,
  "lastCostDate": "2024-02-12",
  "forecastType": "ad-hoc",
  "filters": {
    "include": {},
    "exclude": {
      "chargetype": [
        "Tax"
      ]
    }
  },
  "accountId": "932213950603",
  "monthlyTotalCost": "78503.06",
  "dailyCalculatedAutomatically": true,
  "isPpApplied": false,
  "monthlyTotalForecastedCost": "157404.18",
  "startDate": "2024-01-29 00:00:00",
  "costType": "unblended",
  "divisionId": "0",
  "description": "frgrtgh",
  "warnThresholdDate": "None",
  "actualMonthlyCost": "78503.06",
  "creationTime": "2024-01-29 10:55:19",
  "budgetAmounts": [
   
  ],
  "period": 0,
  "costData": {
    "2024-02-03": "6059.29",
    "2024-02-02": "6692.01",
    "2024-02-05": "5361.55",
    "2024-02-04": "6123.41",
    "2024-02-07": "4722.88",
    "2024-02-06": "7081.14",
    "2024-02-09": "4945.8",
    "2024-02-08": "5503.73",
    "2024-02-10": "5638.67",
    "2024-02-01": "20540.51",
    "2024-02-12": "1077.2",
    "2024-02-11": "4756.87"
  },
  "forecastedWarnThresholdDate": null,
  "previousMonthCost": 0,
  "alerts": [
    {
      "budgetPercentToAlertFrom": 64,
      "alertGranularity": [
        "monthly"
      ],
      "whenToAlert": [
        "forecasted"
      ],
      "recipients": [
        "test@anodot.com"
      ]
    }
  ],
  "dailyBudgetAmount": "191570.86",
  "isNetCost": false,
  "monthlyBudgetAmount": "5555555.0",
  "updateTime": "2024-02-12",
  "overusePrice": {},
  "explicitCostType": true,
  "monthlyStartDate": "2024-02-01 00:00:00",
  "dailyForecastedCost": "5269.32",
  "previousBudgets": [
    "Here we will include a list of the previous Budgets per month in the structure of the current budget"
  ],
  "forecasting": {
    "2024": {
      "2": {
        "forcastedDailyCost": 5246.805963069843,
        "startOveruseDate": 0,
        "forecastTotalCost": 157404.17889209528,
        "forecastedWarnThresholdDate": null,
        "forcastedPrice": {
          "2024-02-21": "4641.44",
          "2024-02-20": "4641.26",
          "2024-02-23": "4641.74",
          "2024-02-22": "4641.6",
          "2024-02-14": "4639.87",
          "2024-02-25": "4641.97",
          "2024-02-13": "4639.58",
          "2024-02-24": "4641.86",
          "2024-02-16": "4640.4",
          "2024-02-27": "4642.14",
          "2024-02-15": "4640.14",
          "2024-02-26": "4642.07",
          "2024-02-18": "4640.87",
          "2024-02-29": "4642.26",
          "2024-02-17": "4640.64",
          "2024-02-28": "4642.21",
          "2024-02-19": "4641.07"
        },
        "overusePrice": { },
        "forecastedActualMonthlyCost": "157404.17889209528"
      },
    }
  },
  "totalCost": 78503,
  "uuid": "4d1ad4b171224e48820b33375a000000",
  "startOveruseDate": "0",
  "budgetAmount": 5555555,
  "isActive": 1,
  "accountKey": "21",
  "budgetName": "Unblended",
  "isValid": "1",
  "remainingBudget": "5477051.94",
  "updateTimestamp": "2024-02-12 16:49:28.033663",
  "preparedAccumCostsData": [
    {
      "date": "2024-02-01",
      "cost": 20540.51,
      "isWarnValue": false,
      "actualData": "20540.51",
      "accumilateNum": 20540.51
    },
    {
      "date": "2024-02-02",
      "cost": 27232.519999999997,
      "isWarnValue": false,
      "actualData": "6692.01",
      "accumilateNum": 27232.519999999997
    },…
  ],
  "totalForcasted": "157404.18",
  "preparedMonthForecastingData": [
    {
      "date": "2024-2-01",
      "forecastTotalCost": 157404.17889209528,
      "monthNum": 0
    },…
  ],
  "budgetAlerts": [
    {
      "budgetPercentToAlertFrom": 64,
      "alertGranularity": [
        "monthly"
      ],
      "whenToAlert": [
        "forecasted"
      ],
      "recipients": [
        "test@anodot.com"
      ]
    }
  ],
  "budgetId": "4d1ad4b171224e48820b33375a000000",
  "isFlexible": 1,
  "excludeFilters": {
    "chargetype": [
      "Tax"
    ]
  },
  "includeFilters": {}
}]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Create Budget

**Summary:** Create a budget

**Description:** The call is used to create budgets

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

**Summary:** Update a budget

**Description:** The call is used to update a budget



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

<aside class="notice">
The edit budget request enables editing the following sections of the budget:</br>
1. Name</br>
2. Description</br>
3. Alerts - Adding, editing and removing alerts for the budget</br>
</br>
Note that only active budgets can be updated
</aside>

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

**Summary:** Delete a budget by Id

**Description:** The call is used to delete a budget

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
