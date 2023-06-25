## Anomaly Detection
### Get Anomaly Detection Rules

**Summary:** Retrieves anomaly-detection rules for given user

**Description:** The call is used to retrieve anomaly-detection created by api-key defined user

> Request example: Get anomaly detection rules

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/usage/anomaly-detections/rules' \
--header 'apikey: {{account-api-token}}' \
--header 'Authorization: {{bearer-token}}'
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

### Edit Anomaly Detection Rules

**Summary:** update anomaly-detection rule for given user

**Description:** The call is used to update anomaly detection rule created by api-key defined user

> Request Example: Editing an Anomaly Detection Rule

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/usage/anomaly-detections/rules' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
--data-raw '{
  "ruleName": "string",
  "minPercentChange": 0,
  "minAmountChange": 0,
  "includeFilters": {
    "filter_name": [
      "string"
    ]
  },
  "excludeFilters": {
    "filter_name": [
      "string"
    ]
  },
  "ruleId": "string"
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
| 200 | successful delete |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Create Anoamly Detection Rules

**Summary:** create anomaly-detection rule for given user

**Description:** The call is used to create anomaly detection rule created by api-key defined user

> Request Example: Creating an anomaly detection rule

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/usage/anomaly-detections/rules' \
--header 'apikey: {{account-api-token}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "ruleName": "string",
  "minPercentChange": 0,
  "minAmountChange": 0,
  "includeFilters": {
    "filter_name": [
      "string"
    ]
  },
  "excludeFilters": {
    "filter_name": [
      "string"
    ]
  },
  "ruleId": "string"
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

### Delete Anomaly Detection Rules 

**Summary:** update anomaly detection rule for given user

**Description:** The call is used to delete anomaly detection rule created by api-key defined user

> Request Example: Delete an anomaly detection rule

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/usage/anomaly-detections/rules?ruleId=sdfsd' \
--header 'apikey: {{account-api-token}}' \
--header 'Authorization: {{Bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| ruleId | query | The anomaly detection rule Id to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |
