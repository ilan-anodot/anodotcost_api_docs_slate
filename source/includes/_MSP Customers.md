## MSP Customers

### Get MSP Customers

**Summary:** Retrieves list of customers for an MSP

**Description:** The call is used to retrieve list of customers for an MSP

> Request Example: Get MSP Customers

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/msp/customers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| startDate | query | Start date of events | Yes | date |
| endDate | query | Start date of events | Yes | date |

**Responses**

> Response Example: Events

```json
[
    {
        "date": "2022-07-07",
        "creationTime": "2022-07-07 12:46:08",
        "createdBy": "PileusUser",
        "uuid": "eventId",
        "divisionId": "0",
        "accountKey": "649",
        "userKey": "userKey",
        "accountId": "accountID",
        "description": "Its time to get the API going!",
        "title": "The Big API Palooza"
    }
]
```


| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

The response is an array of events within the given time frame.

**Event Structure**

| Field | Description |
| ----- | ----------- |
| account_id | The payer account id |
| uuid | The event id |
| creation_time | Time and date when this event was created in format YYYY-MM-DD hh:mm:ss |
| date | Date of the event in format YYYY-MM-DD |
| description | The description of the event |
| title | The title of the event |
| user_key | Identifier of user, who created this event |
| updating_time | The last time and date when this event was updated in format YYYY-MM-DD hh:mm:ss |

### Create MSP Customer

**Summary:** Create MSP customer

**Description:** The call is used to create a new customer for an MSP

> Request Example: Create MSP Customer

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/msp/customers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "customerName",
    "linkedAccountsIds": [“19923818823”, “342134123”],
    "code": "customerCode"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| title | body | Event title | Yes | String |
| description | body | Event description | Yes | String |
| date | body | Event date | Yes | date |
| createdBy | body | Name of user creating the event | Yes | String |

**Responses**

> Response Example - Event Creation

```json
{
    "account_id": "{{acccountID}}",
    "uuid": "fd50e8f0-823a-456e-9df8-7e14f8df3fb7",
    "account_key": "{{accountKey}}",
    "division_id": "0",
    "user_key": "{{user-Key}}",
    "creation_time": "2022-07-07 13:00:50",
    "created_by": "Anodot User",
    "title": "The Big API Palooza II",
    "description": "Its time to get the API going!",
    "date": "2022-07-07"
}
```

| Code | Description |
| ---- | ----------- |
| 201 | successful Creation |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Edit MSP Customer

**Summary:** Update an MSP customer

**Description:** The call is used to update an MSP customer

> Request Example - Edit an MSP customer

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/msp/customers/fd50e8f0-823a-456e-9df8-7e14f8df3fb7' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "customerName",
    "linkedAccountsIds": [“19923818823”, “342134123”],
    "code": "customerCode"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The customer ID to update | Yes |  |

**Responses**

The response will contain the updated customer in the regular structure.

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete MSP Customers

**Summary:** delete an MSP customer

**Description:** The call is used to delete an MSP customer

> Request Example: Delete MSP Customer

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/msp/customers/fd50e8f0-823a-456e-9df8-7e14f8df3fb7' \
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