## Anomaly Detection

These APIs include calls to three entity types:

* Anomaly detection rules - The set of conditions to trigger alerts
* Anomalies - Actual anomalies found in the data
* Alerts - Alert instances that were sent to report on anomalies in the data, according to the detection rules 

<aside class="notice">
The <code>commonParams</code> header is relevant for resellers.</br>
It allows setting rules, or getting anomlies and alerts for customer data.</br>
As a reseller, set it to <code>--header 'commonParams: {"isPpApplied":true}</code> to control/view customer data
</aside>

### Get Alert Configuration Rules

> Request example: Get anomaly detection rules

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/anomaly-detection/rules' \
--header 'Authorization: YOUR_AUTH_TOKEN' \
--header 'apikey: YOUR_API_KEY' \
--header 'commonParams: {"isPpApplied":false}'
```

**Summary:** Retrieves anomaly-detection rules for the user

**Description:** The call is used to retrieve anomaly-detection rules defined by api-key of the defined user

### Create an Alert Configuration Rule

> Request example: POST anomaly detection rule

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/anomaly-detection/rule' \
--header 'Authorization: YOUR_AUTH_TOKEN' \
--header 'apikey: YOUR_API_KEY' \
--data-raw '{
    "alertRule": {
        "ruleName": "asdasd",
        "direction": "UP",
        "minAmountChange?": 5,
        "minPercentChange?": 0,
        "recipientsEmails?": "example@domain.com, example2@domain.com",
        "filters": {
            "include": {
                "service": [
                    "Amazon API Gateway"
                ],
                "division": [
                    "division1"
                ],
                "linkedaccid": [
                    "123456789123"
                ]
            },
            "exclude": {
                "service": [],
                "division": [],
                "linkedaccid": []
            }
        }
    }
}'
```

**Summary:** Creates an anomaly-detection rule

**Description:** The call is used to create an anomaly-detection rule

### Update Alert Configuration Rule

> Request example: PUT anomaly detection rule

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/anomaly-detection/rule' \
--header 'Authorization: YOUR_AUTH_TOKEN' \
--header 'apikey: YOUR_API_KEY' \
--data-raw '{
    "alertRule": {
        "uuid": EXISTING_ITEM_UUID,
        "ruleName": "abcdef",
        "direction": "UP",
        "minAmountChange?": 5,
        "minPercentChange?": 0,
        "recipientsEmails?": "example@domain.com, example2@domain.com",
        "filters": {
            "include": {
                "service": [
                    "Amazon API Gateway"
                ],
                "division": [
                    "division1"
                ],
                "linkedaccid": [
                    "123456789123"
                ]
            },
            "exclude": {
                "service": [],
                "division": [],
                "linkedaccid": []
            }
        }
    }
}'
```

**Summary:** Updates an anomaly-detection rule

**Description:** The call is used to update an anomaly-detection rule

### Delete Alert Configuration Rule

> Request example:

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/anomaly-detection/rule/:id' \
--header 'Authorization: YOUR_AUTH_TOKEN' \
--header 'apikey: YOUR_API_KEY' \
```

**Summary:** Delete the specified anomaly-detection rule by id

### Get Anomalies

> Request example:

```shell
curl --location --request GET 'http://api.mypileus.io/api/v1/anomaly-detection?startDate=2022-12-01&endDate=2023-02-28' \
--header 'Authorization: YOUR_AUTH_TOKEN' \
--header 'apikey: YOUR_API_KEY' \
```

**Summary:** Retrieve the anomalies by timestamps

### Get Alerts

> Request example:

```shell
curl --location --request GET 'http://api.mypileus.io/api/v1/anomaly-detection?alerted=true&startDate=2023-02-01&endDate=2023-02-28' \
--header 'Authorization: YOUR_AUTH_TOKEN' \
--header 'apikey: YOUR_API_KEY' \
```

**Summary:** Retrieve the alerts by timestamps
