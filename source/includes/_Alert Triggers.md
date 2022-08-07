## Alert Triggers

### The Alert Trigger Object

The Alert Trigger object is sent when an anomaly / static / no-data alert matches all its conditions.
Alert Triggers can be sent in groups based on correlations, proximity in time and recipients.
The Alert Triggers API manages and queries these triggers and groups. 

* [List all triggered alerts](#get-triggered-alerts)
* [Cound all triggered alerts](#count-triggered-alerts)
* [Acknowledge an Alert](#add-an-acknowledgement-to-an-alert-trigger)
* [Remove Acknowledgment from an alert](#remove-an-acknowledgement-from-an-alert-trigger)
* [Assign an Alert](#assign-an-alert-trigger)
* [Remove Assignment from an alert](#remove-an-assignment-from-an-alert-trigger)
* [Response Objects](#response-objects)



### Get Triggered Alerts

> Request Example: Get Triggered Alerts

```shell
curl -X GET 'https://app.anodot.com/api/v2/alerts/triggered?startTime=1616929887&order=asc&sort=updatedTime&labels=aws%20monitoring&types=anomaly&channels=645aa58b-16d5-4945-a95a-9fc83af55ae6&severities=HIGH&status=OPEN' \
--header 'Authorization: Bearer {{bearer-token}}' 
```

> Request Example: Get Triggered Alerts by alert configuration ID

```shell
curl -X GET 'https://app.anodot.com/api/v2/alerts/triggered?alertConfigurationIds={{alertConfigurationID}}' \
--header 'Authorization: Bearer {{bearer-token}}' 
```

**Request Arguments**

<aside class="success">
A Performance Tip:</br>
All parameters are optional, however calling this API with no parameters will return ALL triggered alerts of the past year which is a lot of data and not recommended. So please use at least the 'startTime' as a parameter.
</aside>  

Field | Type | Description / Example
-|-|-
startTime | integer | Retrieve alerts which started after this time (epoch time) 
order | enum | Can be either {asc, desc}
sort | enum | Can be either {startTime, updateTime, duration, score, metrics}
labels | string | Labels according to which the alerts are filtered.
types | enum | Can be one (or more) of the following: {anomaly, static, noData}
channels | string | Comma seperated list of channel IDs to which these alerts have been sent. 
severities | enum | Can be one (or more) of the following: {INFO,LOW,MEDIUM,HIGH,CRITICAL}
status | enum | Can be either "OPEN" or "CLOSE"
size | integer | You can specify the max number of triggers to return. The default is 20
alertConfigurationIds | string | Alert configuration ID, limit the call to fetch triggers for a specific alert configuration

> Response Example:

```json
{
    "total": 1,
    "alertGroups": [
        {
            "id": "e23c3444be71438ba8d6a0fcd32bcef7",
            "name": "Anomaly on BlendedCost by ap-south-1 and eu-west-1, Hrs",
            "alerts": [
                {
                    "id": "a1af9f23-1052-4458-abb2-28e797920bcf",
                    "timeScale": "1d",
                    "type": "anomaly",
                    "status": "OPEN",
                    "groupId": "e23c3444be71438ba8d6a0fcd32bcef7",
                    "alertConfigurationId": "5218023a-13e7-4442-9d14-40c1e3ff8aaa",
                    "title": "Anomaly on BlendedCost by ap-south-1 and eu-west-1, Hrs",
                    "description": "",
                    "severity": "high",
                    "startTime": 1617148800,
                    "updateTime": 1617580800,
                    "endTime": 1617580800,
                    "duration": 432000,
                    "channels": [
                        {
                            "id": "645aa58b-16d5-4945-a95a-9fc83af55ae6",
                            "type": "webhook",
                            "name": "slack-test-spectory2"
                        },
                        {
                            "id": "b99aa3ad-c3ad-46e9-be31-11f5da16bc2f",
                            "type": "webhook",
                            "name": "Trello"
                        },
                        {
                            "id": "85bb3cab-d9e2-4b2c-870c-0335a73a930a",
                            "type": "webhook",
                            "name": "Twilio - SMS"
                        }
                    ],
                    "subscribers": [],
                    "metrics": [
                        {
                            "id": "a2c28da9-0b19-4e7e-b13e-36bb20c701f3.Hrs.ap-south-1",
                            "name": "what=BlendedCost.func=Sum.pricing_unit=Hrs.region=ap-south-1.mtype=alert",
                            "what": "BlendedCost",
                            "properties": [
                                {
                                    "key": "pricing_unit",
                                    "value": "Hrs"
                                },
                                {
                                    "key": "region",
                                    "value": "ap-south-1"
                                },
                                {
                                    "key": "func",
                                    "value": "Sum"
                                },
                                {
                                    "key": "mtype",
                                    "value": "alert"
                                }
                            ],
                            "tags": [],
                            "dataPoints": [],
                            "baseline": [],
                            "origin": {
                                "type": "ALERT",
                                "id": "5218023a-13e7-4442-9d14-40c1e3ff8aaa",
                                "title": "Anomaly_on_BlendedCost_by_{{region}},_{{pricing_unit}}_BlendedCost"
                            },
                            "status": "close",
                            "updatedTime": 1617580800,
                            "startTime": 1617148800,
                            "displayStartTime": 1615939200,
                            "displayEndTime": 1618140542,
                            "snooze": null,
                            "stopLearning": null,
                            "intervals": [
                                {
                                    "status": "CLOSE",
                                    "startTime": 1617148800,
                                    "endTime": 1617494400,
                                    "duration": 345600,
                                    "anomalyId": "e23c3444be71438ba8d6a0fcd32bcef7",
                                    "direction": "UP",
                                    "peak": 134.79038581359998,
                                    "score": 0.8065615790125528,
                                    "deltaAbsolute": 62.12345294310754,
                                    "deltaPercentage": 85.49067710594642,
                                    "sumDeltas": 194.84395965044826
                                }
                            ],
                            "upperAbsoluteDelta": 62.12345294310754,
                            "upperPercentageDelta": 85.49067710594642,
                            "lowerAbsoluteDelta": null,
                            "lowerPercentageDelta": null,
                            "impact": null
                        },
                        {
                            "id": "a2c28da9-0b19-4e7e-b13e-36bb20c701f3.Hrs.eu-west-1",
                            "name": "what=BlendedCost.func=Sum.pricing_unit=Hrs.region=eu-west-1.mtype=alert",
                            "what": "BlendedCost",
                            "properties": [
                                {
                                    "key": "pricing_unit",
                                    "value": "Hrs"
                                },
                                {
                                    "key": "region",
                                    "value": "eu-west-1"
                                },
                                {
                                    "key": "func",
                                    "value": "Sum"
                                },
                                {
                                    "key": "mtype",
                                    "value": "alert"
                                }
                            ],
                            "tags": [],
                            "dataPoints": [],
                            "baseline": [],
                            "origin": {
                                "type": "ALERT",
                                "id": "5218023a-13e7-4442-9d14-40c1e3ff8aaa",
                                "title": "Anomaly_on_BlendedCost_by_{{region}},_{{pricing_unit}}_BlendedCost"
                            },
                            "status": "open",
                            "updatedTime": 1617235200,
                            "startTime": 1617148800,
                            "displayStartTime": 1615939200,
                            "displayEndTime": 1617667201,
                            "snooze": null,
                            "stopLearning": null,
                            "intervals": [
                                {
                                    "status": "OPEN",
                                    "startTime": 1617148800,
                                    "endTime": 1617580800,
                                    "duration": 432000,
                                    "anomalyId": "e23c3444be71438ba8d6a0fcd32bcef7",
                                    "direction": "UP",
                                    "peak": 2.155577299200001,
                                    "score": 0.8899891304643669,
                                    "deltaAbsolute": 1.7445812570728427,
                                    "deltaPercentage": 425.2623051375808,
                                    "sumDeltas": 7.026737624452241
                                }
                            ],
                            "upperAbsoluteDelta": 1.7445812570728427,
                            "upperPercentageDelta": 425.2623051375808,
                            "lowerAbsoluteDelta": null,
                            "lowerPercentageDelta": null,
                            "impact": null
                        }
                    ],
                    "summary": {
                        "totalMetrics": 2,
                        "snoozedMetrics": 0,
                        "stlMetrics": 0,
                        "snoozeSummary": null,
                        "stlSummary": null
                    },
                    "stars": [],
                    "reads": [],
                    "feedback": [],
                    "impactEligible": false,
                    "impact": null,
                    "alertActions": null
                }
            ],
            "startTime": 1617148800,
            "updateTime": 1617580800
        }
    ]
}
```

**Alert Response Fields**

Field | Type | Description / Example
-|-|-
total | number | Number of alert groups which meet the criteria.
alertGroups | Array | Array of [Alert Group](#alert-group) objects. 

### Count Triggered Alerts

> Request Example: Count Triggered Alerts

```shell
curl -X GET 'https://app.anodot.com/api/v2/alerts/triggered/count?startTime=1616929887&types=anomaly' \
--header 'Authorization: Bearer {{bearer-token}}' 
```
**Request Arguments**

Request arguments are the same as the [List all triggered alerts](#get-triggered-alerts) API Call and will enable you to filter the triggered alerts count you will get.

> Response Example:

```json
{
    "total": 1
}
```

**Alert Response Fields**

Field | Type | Description / Example
-|-|-
total | number | Number of alert triggers which meet the criteria.

### Acknowledge an Alert Trigger

> Request Structure: POST /triggered/{alert-id}/acknowledge/add

```shell

curl --location --request POST 'https://app.anodot.com/api/v2/alerts/triggered/{{alert-id}}/acknowledge/add' \
--header 'Authorization: Bearer {{bearer-token}} \
--header 'Content-Type: application/json' \
--data-raw '{
    "userId": "{{user-id}}"
}'
```

Acknowledge the Alert Trigger

**Request Arguments**

Argument | Type | Description
---------|------|------------
groupId | string | ID of the triggered alert group **Required**
userId | application/json | **Required**. This Id specifies which user performed the Acknowledgement.

**Response**

None.

### Remove an Acknowledgement from an Alert Trigger

> Request Structure: POST /triggered/{alert-id}/acknowledge/remove

```shell

curl --location --request POST 'https://app.anodot.com/api/v2/alerts/triggered/{{alert-id}}/acknowledge/remove' \
--header 'Authorization: Bearer {{bearer-token}} \
--header 'Content-Type: application/json' \
--data-raw '{
    "userId": "{{user-id}}"
}'
```

Remove the Ack from the alert trigger

**Request Arguments**

Argument | Type | Description
---------|------|------------
groupId | string | ID of the triggered alert group **Required**
userId | application/json | The object that will be removed **Required**. This Id specifies which user is removed from the Acknowledgement.

**Response**

None.

### Assign an Alert Trigger

> Request Structure: PUT /triggered/{alert-id}/assignee

```shell

curl --location --request PUT 'https://app.anodot.com/api/v2/alerts/triggered/{{alert-id}}/assignee' \
--header 'Authorization: Bearer {{bearer-token}} \
--header 'Content-Type: application/json' \
--data-raw '{
    "assigneeId": "{{user-id}}",
    "assignerId": "{{user-id}}"
}'
```

Assign an Alert Trigger

**Request Arguments**

Argument | Type | Description
---------|------|------------
alertID | string | ID of the triggered alert group **Required**
assigneeId | unique user identifier | **Required**. This Id specifies to which user the alert trigger will be assigned.
assignerId | unique user identifier | **Required**. This Id specifies which user performed the Assignment.


**Response**

None.

### Remove an Assignment from an Alert Trigger

> Request Structure: DELETE /triggered/{alert-id}/assignee

```shell

curl --location --request DELETE 'https://app.anodot.com/api/v2/alerts/triggered/{{alert-id}}/assignee' \
--header 'Authorization: Bearer {{bearer-token}} \
--header 'Content-Type: application/json' \
--data-raw '{
    "assigneeId": "{{user-id}}",
    "assignerId": "{{user-id}}"
}'
```

Remove assignment from an Alert Trigger

**Request Arguments**

Argument | Type | Description
---------|------|------------
alertID | string | ID of the triggered alert group **Required**
assigneeId | unique user identifier | **Required**. This Id specifies to which user the alert trigger will be assigned.
assignerId | unique user identifier | **Required**. This Id specifies which user performed the Assignment.

**Response**

None.

### Common Objects

#### Alert Group

Field | Type | Description / Example
-|-|-
id | string(uuid) |example: 20262fb3-00ba-412c-a515-aec74c4824cd.<br/>Group Id.
name | string | example: Jetty servers availability. Alert name when grouping by alert configuration, empty otherwise.
alerts | Array | Array of [Alert objects](#alert)
stars | String Array | The ids of all the users that acknowledged the alert group

#### Alert 

Field | Type | Description / Example
-|-|-
id | string (uuid)| Alert Trigger ID.<br/>example: 20262fb3-00ba-412c-a515-aec74c4824cd
timeScale | string(Enum - 1m - 5m - 1h - 1d - 1w) | example: 5m
type | string(Enum - no_data - static - anomaly) | example: anomaly
status | string(Enum - open - closed) | example: open
groupId| string(uuid) | example: 20262fb3-00ba-412c-a515-aec74c4824ca.<br/>Anodot automaticly groups related alert together.<br/>This Id represents the group.
alertConfigurationId | string(uuid) | example: 20262fb3-00ba-412c-a515-aec74c4824ca. Alert configuration Id.
title | string | example: Jetty servers availability.<br/>Alert title.
description	| string| example: This alerts monitors the availablity of the jetty servers.<br/> Alert description.
severity |string(Enum - critical - high - medium - low - info)| example: critical <br/>Alert severity.
startTime|integer(int32)|example: 1512882919 <br/>Start time of the anomaly (epoch time in seconds).
updatedTime|integer(int32) |example: 1512882919<br/>Last timestamp the alert was updated (epoch time in seconds).<br/> Update means that an open metric was closed or new metric was added to the alert.
endTime|integer(int32)|example: 1512982919<br/>End timestamp of the alert (epoch time in seconds). May be empty for non 'closed' alerts.
channels| object array | Array of [channels](#channels-array) associated with the alert.
metrics | object array | Array of [metrics](#metrics-array) different types of alerts hold different metric interval objects.
#### Channels Array

Field | Type | Description / Example
-|-|-
id | string | example: 20262fb3-00ba-412c-a515-aec74c4824ca. <br/>Channel Id.
type | string(Enum - user - email - webhook - slack)|example: email.
name |string |example: NOC Channel. Channel name.
#### Metrics Array

Field | Type | Description / Example
-|-|-
id |string |example: 20262fb3-00ba-412c-a515-aec74c4824ca. Metric Id.
name | string | example: what=cpu.metric=cpuIdle.server=anodot.dc=ny. Metric name.
what |string | example: cpu. Metric measurment.
properties| Array | Metric [properties](#properties-tags-array).
tags | Array | Metric [Tags](#properties-tags-array).
snooze | Object | [Metric snooze Object](#metric-snooze-object).
origin | Object | [Metric origin Object](#metric-origin-object).
status | string(Enum - open - closed) | example: closed
updatedTime | integer(int32) |example: 1512982919<br/>Last updated timestamp of the metric (epoch time in seconds).
displayStartTime | integer |example: 1512982919<br/>The recommend time to display the metrics from (epoch time in seconds).
displayEndTime |integer |example: 1512982919<br/>The recommend time to display the metrics until (epoch time in seconds).
intervals | Array | Array of metric interval breaches.<br/>One of:<br/>* [Anomaly Metric Interval](#anomaly-metric-interval-object)<br>* [NoData Metric Interval](#no-data-metric-interval-object)<br/>* [StaticTHMetricInterval](#static-th-metric-interval-object)

#### Properties/Tags Array

Field | Type | Description / Example
-|-|-
key | string | example: server. Property key.
value| string | example: anodot32. Property value.

#### Metric Snooze Object

Field | Type | Description / Example
-|-|-
userId | string |example: 20262fb3-00ba-412c-a515-aec74c4824ca.<br/>The Id of the user that snoozed the metric
snoozeTime | integer |example: 1525586116.<br/>Epoch time (seconds) of when the metric was snoozed
resumeTime | integer |example: 1525586116<br/>Epoch time (seconds) of when the metric should resume after snooze

#### Metric Origin Object

Field | Type | Description / Example
-|-|-
type | string(Enum - alert - composite - stream) | example: alert. Origin type.
id | string | example: 20262fb3-00ba-412c-a515-aec74c4824ca<br/>Origin Id.
title | string | example: GA sessions of my application. Origin title.

#### Anomaly Metric Interval Object

Represents a metric anomaly caused by a baseline breach.
Metric peak, deltaAbsolute, deltaPercentage are computed based on the first breach direction that occured.

Field | Type | Description / Example
-|-|-
status|string(Enum - open - closed)| example: closed
startTime | integer(int32) | example: 1512882919.<br/>The anomaly start (epoch time in seconds).
endTime	| integer(int32) | example: 1512982919 | End timestamp of the metric alert (epoch time in seconds).<br/>null when metric is open.
duration | integer(int32) | example: 300. Duration of the metric alert in seconds.
anomalyId | string | example: 20262fb3-00ba-412c-a515-aec74c4824ca. Anomaly Id.
direction | string(Enum - up - down) | example: up
score | number(double) | example: 0.99. Anomaly score between 0 to 1. Represents 0-100 in the UI.
peak | number(double)| example: 45.99. The highest or lowest value of the metric.
deltaAbsolute | number(double) |example: 3.76. The absolute delta between the metric value and the baseline sleeve.
deltaPercentage | number(double) |example: 10.42. The percentage delta between the metric value and the baseline sleeve.

#### No Data Metric Interval Object

Field | Type | Description / Example
-|-|-
status | string(Enum - open - closed) |example: closed
startTime| integer(int32) |example: 1512882919. The last time the metric was seen (epoch time in seconds).
endTime| integer(int32) |example: 1512982919. End timestamp of the metric alert (epoch time in seconds). null when metric is open.
duration | integer(int32) |example: 300. Duration of the metric alert in seconds.
lastSeenTime| integer(int32) |example: 1512382919. Last timestamp (epoch in seconds) the metric reported data at.

#### Static TH Metric Interval Object

Represents a metric static threshold breach.
Metric peak is computed based on the first breach direction that occured.

Field | Type | Description / Example
-|-|-
status | string(Enum - open - closed) |example: closed
startTime |integer(int32) |example: 1512882919. Breach start time (epoch time in seconds).
endTime	| integer(int32) |example: 1512982919. End timestamp of the metric alert (epoch time in seconds). null when metric is open.
duration |integer(int32) | example: 300. Duration of the metric alert in seconds.
threshold |number(double) | example: 40.3. The static threshold crossed by the metric.
direction |string(Enum - up - down) | example: up
peak |number(double) |example: 45.99. The highest or lowest value of the metric.
