--- 
includes: 
   - errors 

--- 

## Introduction 

Pileus API 

**Version:** 1.0.0-oas3 

## /USERS
### ***GET*** 

**Summary:** retrieve users

#### HTTP Request 
`***GET*** /users` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | User retrival success |
| 404 | User not found |
| 500 | Server error |

## /DIVISIONS
### ***GET*** 

**Summary:** retrieve divisions

#### HTTP Request 
`***GET*** /divisions` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Divisions retrival success |
| 404 | Divisions not found |
| 500 | Server error |

## /INVOICES/COST-AND-USAGE
### ***GET*** 

**Summary:** Retrieves cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve cloud invoice data grouped and filters by different values

#### HTTP Request 
`***GET*** /invoices/cost-and-usage` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| groupBy | query | The group by value that will be used for the results | Yes |  |
| startDate | query | Start date of the cost and usage data examination | Yes |  |
| endDate | query | End date of the cost and usage data examination | Yes |  |
| periodGranLevel | query | Granularity level of the period in the output data | Yes |  |
| costType | query |  | No |  |
| isNetUnblended | query |  | No |  |
| isAmortized | query |  | No |  |
| isNetAmortized | query |  | No |  |
| wheres | query | conditions of type if x equals y when retrieving data | No |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data | No |  |
| excludeFilters | query | conditions of type if x not in (y,z,w) when retrieving data | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## /RECOMMENDATIONS
### ***GET*** 

**Summary:** Retrieves current live recommendations

**Description:** The call is used to retrieve the current live recommendations

#### HTTP Request 
`***GET*** /recommendations` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## /RECOMMENDATIONS/TYPES
### ***GET*** 

**Summary:** Retrieves current live recommendations types

**Description:** The call is used to retrieve the current live recommendations types

#### HTTP Request 
`***GET*** /recommendations/types` 

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

## /RECOMMENDATIONS/HISTORY
### ***GET*** 

**Summary:** Retrieves history of recommendations

**Description:** The call is used to retrieve history recommendations

#### HTTP Request 
`***GET*** /recommendations/history` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## /RECOMMENDATIONS/HISTORY/TYPES
### ***GET*** 

**Summary:** Retrieves current history recommendations types

**Description:** The call is used to retrieve the history recommendations types

#### HTTP Request 
`***GET*** /recommendations/history/types` 

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

## /BUDGETS
### ***GET*** 

**Summary:** Retrieves budgets for given user

**Description:** The call is used to retrieve budgets created by api-key defined user

#### HTTP Request 
`***GET*** /budgets` 

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

### ***PUT*** 

**Summary:** update budget for given user

**Description:** The call is used to update budget created by api-key defined user

#### HTTP Request 
`***PUT*** /budgets` 

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

### ***POST*** 

**Summary:** create budget for given user

**Description:** The call is used to create budgets created by api-key defined user

#### HTTP Request 
`***POST*** /budgets` 

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

### ***DELETE*** 

**Summary:** update budget for given user

**Description:** The call is used to delete budget created by api-key defined user

#### HTTP Request 
`***DELETE*** /budgets` 

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

## /USAGE/ANOMALY-DETECTIONS/RULES
### ***GET*** 

**Summary:** Retrieves anomaly-detection rules for given user

**Description:** The call is used to retrieve anomaly-detection created by api-key defined user

#### HTTP Request 
`***GET*** /usage/anomaly-detections/rules` 

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

### ***PUT*** 

**Summary:** update anomaly-detection rule for given user

**Description:** The call is used to update anomaly detection rule created by api-key defined user

#### HTTP Request 
`***PUT*** /usage/anomaly-detections/rules` 

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

### ***POST*** 

**Summary:** create anomaly-detection rule for given user

**Description:** The call is used to create anomaly detection rule created by api-key defined user

#### HTTP Request 
`***POST*** /usage/anomaly-detections/rules` 

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

### ***DELETE*** 

**Summary:** update anomaly detection rule for given user

**Description:** The call is used to delete anomaly detection rule created by api-key defined user

#### HTTP Request 
`***DELETE*** /usage/anomaly-detections/rules` 

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

## /KUBERNETES/COST-AND-USAGE
### ***GET*** 

**Summary:** Retrieves kubernetes cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve kubernetes data grouped and filtered by different values

#### HTTP Request 
`***GET*** /kubernetes/cost-and-usage` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| groupBy | query | The group by value that will be used for the results | Yes |  |
| startDate | query | Start date of the cost and usage data examination | Yes |  |
| endDate | query | End date of the cost and usage data examination | Yes |  |
| periodGranLevel | query | Granularity level of the period in the output data | Yes |  |
| costUsageType | query |  | No |  |
| usageType | query |  | No |  |
| metricType | query |  | No |  |
| isNetUnblended | query |  | No |  |
| isAmortized | query |  | No |  |
| isNetAmortized | query |  | No |  |
| wheres | query | conditions of type if x equals y when retrieving data | No |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data | No |  |
| excludeFilters | query | conditions of type if x not in (y,z,w) when retrieving data | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## /KUBERNETES/COST-AND-USAGE/POD
### ***GET*** 

**Summary:** Retrieves kubernetes cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve kubernetes Pod cost and usage data grouped

#### HTTP Request 
`***GET*** /kubernetes/cost-and-usage/pod` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| startDate | query | Start date of the cost and usage data examination | Yes |  |
| endDate | query | End date of the cost and usage data examination | No |  |
| clusterName | query | The cluster name that the pod running on. cluster name formatting should be in following format- [CLUSTER_REGION]:[CLUSTER_ACCOUNT_ID]:[CLUSTER_NAME] for example us-east-1:123456789012:prod-cluster | No |  |
| nodeName | query |  | No |  |
| podName | query | pod name formatting should be in following format- [POD_NAMESPACE]:[POD_NAME] for example kube-system:aws-node-k5jgh | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## /EVENTS
### ***GET*** 

**Summary:** Retrieves events for given user

**Description:** The call is used to retrieve events created by api-key defined user

#### HTTP Request 
`***GET*** /events` 

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

### ***POST*** 

**Summary:** create event for given user

**Description:** The call is used to create events created by api-key defined user

#### HTTP Request 
`***POST*** /events` 

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

## /EVENTS/{ID}
### ***PUT*** 

**Summary:** update event for given user

**Description:** The call is used to update budget created by api-key defined user

#### HTTP Request 
`***PUT*** /events/{id}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The event ID to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### ***DELETE*** 

**Summary:** delete event for given user

**Description:** The call is used to delete event created by api-key defined user

#### HTTP Request 
`***DELETE*** /events/{id}` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The event ID to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

<!-- Converted with the swagger-to-slate https://github.com/lavkumarv/swagger-to-slate -->
