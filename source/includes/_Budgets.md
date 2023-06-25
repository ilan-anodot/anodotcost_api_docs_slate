## Budgets
### Get Budget 

**Summary:** Retrieves budgets for given user

**Description:** The call is used to retrieve budgets created by api-key defined user


> Request Example: Getting a budget


```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/budgets' \
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

### Edit Budget 

**Summary:** update budget for given user

**Description:** The call is used to update budget created by api-key defined user

> Request Example: Update a budget 

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

### Create Budget

**Summary:** create budget for given user

**Description:** The call is used to create budgets created by api-key defined user

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

**Summary:** Delette budget for given user

**Description:** The call is used to delete budget created by api-key defined user

> Request Example: Delete a budget

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/budgets?budgetId={{budget-id}}' \
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
