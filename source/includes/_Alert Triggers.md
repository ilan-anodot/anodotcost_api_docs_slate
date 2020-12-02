# Alert Triggers

## The Alert Trigger Object

> End Points:

> https://api.anodot.com/v1/alerts

The Alert Trigger object is sent when an anomaly / static / no-data alert matches all its conditions.
Alert Triggers can be sent in groups based on correlations, proximity in time and recipients.
The Alert Triggers API manages and queries these triggers and groups.

## Get Triggered Alerts

> Request Structure: GET /triggers/{triggerId}

### Request Arguments

Argument | Type | Description
---------|------|------------
triggerId **Required** | string | Id of the alert trigger to fetch
metricQuery | string | Filter alerts that include the metrics specified by the given search query
sort | string | Sorting by one of the given fields: 'startTime', 'updatedTime'.<br/>Default value is 'updatedTime'.<br/>Available values : startTime, updatedTime.
order | string | Sorting order, one of: 'asc', 'desc'.<br/>Default value is 'desc'.<br/>Available values : asc, desc

> Response Example:

```json
{
  "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
  "timeScale": "5m",
  "type": "anomaly",
  "status": "open",
  "groupId": "20262fb3-00ba-412c-a515-aec74c4824ca",
  "alertConfigurationId": "20262fb3-00ba-412c-a515-aec74c4824ca",
  "title": "Jetty servers availability",
  "description": "This alerts monitors the availablity of the jetty servers",
  "severity": "critical",
  "startTime": 1512882919,
  "updatedTime": 1512882919,
  "endTime": 1512982919,
  "channels": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
      "type": "email",
      "name": "NOC Channel"
    }
  ],
  "metrics": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
      "name": "what=cpu.metric=cpuIdle.server=anodot.dc=ny",
      "what": "cpu",
      "properties": [
        {
          "key": "server",
          "value": "anodot32"
        }
      ],
      "tags": [
        {
          "key": "server",
          "value": "anodot32"
        }
      ],
      "snooze": {
        "userId": "20262fb3-00ba-412c-a515-aec74c4824ca",
        "snoozeTime": 1525586116,
        "resumeTime": 1525586116
      },
      "origin": {
        "type": "alert",
        "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
        "title": "GA sessions of my application"
      },
      "status": "closed",
      "updatedTime": 1512982919,
      "displayStartTime": 1512982919,
      "displayEndTime": 1512982919,
      "intervals": [
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "anomalyId": "20262fb3-00ba-412c-a515-aec74c4824ca",
          "direction": "up",
          "score": 0.99,
          "peak": 45.99,
          "deltaAbsolute": 3.76,
          "deltaPercentage": 10.42
        },
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "lastSeenTime": 1512382919
        },
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "threshold": 40.3,
          "direction": "up",
          "peak": 45.99
        }
      ]
    }
  ]
}
```

### Alert Response Fields

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
startTime|integer(int32)|example: 1512882919 <br/>Start timestamp of the alert (epoch time in seconds).
updatedTime|integer(int32) |example: 1512882919<br/>Last timestamp the alert was updated (epoch time in seconds).<br/> Update means that an open metric was closed or new metric was added to the alert.
endTime|integer(int32)|example: 1512982919<br/>End timestamp of the alert (epoch time in seconds). May be empty for non 'closed' alerts.
channels| object array | Array of channels associated with the alert.<br/>See details below
metrics | object array | Array of metrics different types of alerts hold different metric interval objects.<br/>See details below

### Channels Array

Field | Type | Description / Example
-|-|-
id | string | example: 20262fb3-00ba-412c-a515-aec74c4824ca. <br/>Channel Id.
type | string(Enum - user - email - webhook - slack)|example: email.
name |string |example: NOC Channel. Channel name.

### Metrics Array

Field | Type | Description / Example
-|-|-
id |string |example: 20262fb3-00ba-412c-a515-aec74c4824ca. Metric Id.
name | string | example: what=cpu.metric=cpuIdle.server=anodot.dc=ny. Metric name.
what |string | example: cpu. Metric measurment.
properties| Array | Metric properties.
tags | Array | Tags Array.
snooze | Object | Metric snooze Object.
origin | Object | Metric origin Object.
status | string(Enum - open - closed) | example: closed
updatedTime | integer(int32) |example: 1512982919<br/>Last updated timestamp of the metric (epoch time in seconds).
displayStartTime | integer |example: 1512982919<br/>The recommend time to display the metrics from (epoch time in seconds).
displayEndTime |integer |example: 1512982919<br/>The recommend time to display the metrics until (epoch time in seconds).
intervals | Array | Array of metric interval breaches.<br/>One of:<br/>* AnomalyMetricInterval<br>* NoDataMetricInterval<br/>* StaticTHMetricInterval

### Properties/Tags Array

Field | Type | Description / Example
-|-|-
key | string | example: server. Property key.
value| string |example: anodot32. Property value.

### Metric Snooze Object

Field | Type | Description / Example
-|-|-
userId | string |example: 20262fb3-00ba-412c-a515-aec74c4824ca.<br/>The Id of the user that snoozed the metric
snoozeTime | integer |example: 1525586116.<br/>Epoch time (seconds) of when the metric was snoozed
resumeTime | integer |example: 1525586116<br/>Epoch time (seconds) of when the metric should resume after snooze

### Metric Origin Object

Field | Type | Description / Example
-|-|-
type | string(Enum - alert - composite - stream) | example: alert. Origin type.
id | string | example: 20262fb3-00ba-412c-a515-aec74c4824ca<br/>Origin Id.
title | string | example: GA sessions of my application. Origin title.

### AnomalyMetricInterval Object

Represents a metric anomaly caused by a baseline breach.
Metric peak, deltaAbsolute, deltaPercentage are computed based on the first breach direction that occured.

Field | Type | Description / Example
-|-|-
status|string(Enum - open - closed)| example: closed
startTime | integer(int32) | example: 1512882919.<br/>Start timestamp of the metric alert (epoch time in seconds).
endTime	| integer(int32) | example: 1512982919 | End timestamp of the metric alert (epoch time in seconds).<br/>null when metric is open.
duration | integer(int32) | example: 300. Duration of the metric alert in seconds.
anomalyId | string | example: 20262fb3-00ba-412c-a515-aec74c4824ca. Anomaly Id.
direction | string(Enum - up - down) | example: up
score | number(double) | example: 0.99. Anomaly score between 0 to 1. Represents 0-100 in the UI.
peak | number(double)| example: 45.99. The highest or lowest value of the metric.
deltaAbsolute | number(double) |example: 3.76. The absolute delta between the metric value and the baseline sleeve.
deltaPercentage | number(double) |example: 10.42. The percentage delta between the metric value and the baseline sleeve.

### NoDataMetricInterval Object

Field | Type | Description / Example
-|-|-
status | string(Enum - open - closed) |example: closed
startTime| integer(int32) |example: 1512882919. Start timestamp of the metric alert (epoch time in seconds).
endTime| integer(int32) |example: 1512982919. End timestamp of the metric alert (epoch time in seconds). null when metric is open.
duration | integer(int32) |example: 300. Duration of the metric alert in seconds.
lastSeenTime| integer(int32) |example: 1512382919. Last timestamp (epoch in seconds) the metric reported data at.

### StaticTHMetricInterval Object

Represents a metric static threshold breach.
Metric peak is computed based on the first breach direction that occured.

Field | Type | Description / Example
-|-|-
status | string(Enum - open - closed) |example: closed
startTime |integer(int32) |example: 1512882919. Start timestamp of the metric alert (epoch time in seconds).
endTime	| integer(int32) |example: 1512982919. End timestamp of the metric alert (epoch time in seconds). null when metric is open.
duration |integer(int32) | example: 300. Duration of the metric alert in seconds.
threshold |number(double) | example: 40.3. The static threshold crossed by the metric.
direction |string(Enum - up - down) | example: up
peak |number(double) |example: 45.99. The highest or lowest value of the metric.

## Add an Acknoledgement to an Alert Trigger

> Request Structure: POST /triggers/ack/add

> Example Request Body:

```json
{
  "userId": "20262fb3-00ba-412c-a515-aec74c4824ca"
}
```
Acknowledge the Alert Trigger

### Request Arguments

Argument | Type | Description
---------|------|------------
groupId | string | ID of the triggered alert group **Required**
Request body | application/json | The object that will be added **Required**. This Id specifies which user performed the Acknowledgement.

### Respones

Code | Description | Details/Next steps
-|-|-
200 | Successful Operation | .
400 | Invalid ID supplied | The user ID provided in the request was not found.<br/>Check the user ID and retry
401 | Unauthorized request | Check the token used in the request and retry.
404 | Alert trigger not found | The groupId provided in the request was not found.<br/>Check the group ID and retry.
500 | Unexpected server failure | Try again later.

## Remove an Acknoledgement from an Alert Trigger

> Request Structure: POST /triggers/ack/remove

> Example Request Body:

```json
{
  "userId": "20262fb3-00ba-412c-a515-aec74c4824ca"
}
```

Remove the Ack from the alert trigger

### Request Arguments

Argument | Type | Description
---------|------|------------
groupId | string | ID of the triggered alert group **Required**
Request body | application/json | The object that will be removed **Required**. This Id specifies which user is removed from the Acknowledgement.

### Respones

Code | Description | Details/Next steps
-|-|-
200 | Successful Operation | .
400 | Invalid ID supplied | The user ID provided in the request was not found.<br/>Check the user ID and retry
401 | Unauthorized request | Check the token used in the request and retry.
404 | Alert trigger not found | The groupId provided in the request was not found.<br/>Check the group ID and retry.
500 | Unexpected server failure | Try again later.

# Alert Trigger Group Requests

These requests get alert triggers by filtering the groups they belong to.

## Get Triggered Alert Groups

> Request Structure: GET /trigger-group

> Example Request:

```json
{"status": "This example it yet to Be Added"}
```

Fetch alert triggers for a specific time frame based on a given set of filters.
Arrays can be passed as comma seperated list of strings.

### Request Arguments

Argument | Type | Description
---------|------|------------
channels | array string | Channel Id of the alert.<br/>Only alerts that were triggered to the specified list of channels are included in the result. User Ids may be specified as well to filter alerts that have specific subscribers.
searchQuery | string | Filter alerts that include the given search query in the alert title or description.
metricQuery | string | Filter alerts that include the metrics specified by the given search query.
startTime | integer | Time the alert was opened (epoch in seconds). Only alerts that occured after this given time are included in the result. **required**
endTime | integer | Time the alert was closed (epoch in seconds). Only alerts that started before this given time are included in the result.
snoozed | boolean | Include alerts that contain at least one snoozed metric. Default is snoozed.
updatedStartTime | integer | Time the alert was last updated (epoch in seconds). Only alerts that were updated after the given time are included in the result.
updatedEndTime | integer | Time the alert was last updated (epoch in seconds). Only alerts that were updated before the given time are included in the result.
ackType | integer | TBD akcnowlage
type | array string | Array of the following strings 'no_data', 'static', 'anomaly'. By default alert of all types are returned. Available values : no_data, static, anomaly
status | array string | Array of the following strings: 'open', 'closed'. By default alert of all statuses are returned. Available values : open, closed
severity | array string | Array of the following strings 'critical', 'high', 'medium', 'low', 'info'.<br/>By default alert of all serverities are returned. Available values : critical, high, medium, low, info.
sort | string | Sorting by one of the given fields: 'startTime', 'updatedTime', 'severity'.<br/>Default value is 'updatedTime'. When non time-based field is selected a secondary sorting by updatedTime is performed.<br/> When using grouping, the sorting of the group is perforned accordingly:<br/>* startTime - min of startTime cross all alerts in the group.<br/>* updatedTime - max of updatedTime cross all alerts in the group.<br/>* severity - highest severity cross all alerts in the group.<br/>Available values : startTime, updatedTime, severity
order | string | Sorting order, one of: 'asc', 'desc'.<br/>Default value is 'desc'.<br/>Available values : asc, desc
groupBy | string | Group the anomaly instances. There are few supported groupings:<br/>* relatedAlerts - group related alerts together.<br/>* alertConfiguration - group alert instances by the alert configuration.<br/>* none - no grouping. When none is selected the grouping level of the response is ommited.<br/>Default value is 'relatedAlerts'.<br/>Available values : relatedAlerts, alertConfiguration, none.
index | integer | Number of records to skip for pagination
size | integer | Maximum number of records to return per page.

> Example Response:

```json
{
  "total": 121,
  "alertGroups": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
      "name": "Jetty servers availability",
      "alerts": [
        {
          "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
          "timeScale": "5m",
          "type": "anomaly",
          "status": "open",
          "groupId": "20262fb3-00ba-412c-a515-aec74c4824ca",
          "alertConfigurationId": "20262fb3-00ba-412c-a515-aec74c4824ca",
          "title": "Jetty servers availability",
          "description": "This alerts monitors the availablity of the jetty servers",
          "severity": "critical",
          "startTime": 1512882919,
          "updatedTime": 1512882919,
          "endTime": 1512982919,
          "channels": [
            {
              "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
              "type": "email",
              "name": "NOC Channel"
            }
          ],
          "metrics": [
            {
              "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
              "name": "what=cpu.metric=cpuIdle.server=anodot.dc=ny",
              "what": "cpu",
              "properties": [
                {
                  "key": "server",
                  "value": "anodot32"
                }
              ],
              "tags": [
                {
                  "key": "server",
                  "value": "anodot32"
                }
              ],
              "snooze": {
                "userId": "20262fb3-00ba-412c-a515-aec74c4824ca",
                "snoozeTime": 1525586116,
                "resumeTime": 1525586116
              },
              "origin": {
                "type": "alert",
                "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
                "title": "GA sessions of my application"
              },
              "status": "closed",
              "updatedTime": 1512982919,
              "displayStartTime": 1512982919,
              "displayEndTime": 1512982919,
              "intervals": [
                {
                  "status": "closed",
                  "startTime": 1512882919,
                  "endTime": 1512982919,
                  "duration": 300,
                  "anomalyId": "20262fb3-00ba-412c-a515-aec74c4824ca",
                  "direction": "up",
                  "score": 0.99,
                  "peak": 45.99,
                  "deltaAbsolute": 3.76,
                  "deltaPercentage": 10.42
                },
                {
                  "status": "closed",
                  "startTime": 1512882919,
                  "endTime": 1512982919,
                  "duration": 300,
                  "lastSeenTime": 1512382919
                },
                {
                  "status": "closed",
                  "startTime": 1512882919,
                  "endTime": 1512982919,
                  "duration": 300,
                  "threshold": 40.3,
                  "direction": "up",
                  "peak": 45.99
                }
              ]
            }
          ]
        }
      ],
      "stars": [
        "string"
      ]
    }
  ]
}
```

### Response Structure

Field | Type | Description / Example
-|-|-
total | integer | example:121. Total items in group.
alertGroups | Array | Array of alert groups

### Alert Group

Field | Type | Description / Example
-|-|-
id | string(uuid) |example: 20262fb3-00ba-412c-a515-aec74c4824cd.<br/>Group Id.
name | string | example: Jetty servers availability. Alert name when grouping by alert configuration, empty otherwise.
alerts | Array | Array of Alert objects
stars | String Array | The ids of all the users that starred the alert group

### Alert 

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
startTime|integer(int32)|example: 1512882919 <br/>Start timestamp of the alert (epoch time in seconds).
updatedTime|integer(int32) |example: 1512882919<br/>Last timestamp the alert was updated (epoch time in seconds).<br/> Update means that an open metric was closed or new metric was added to the alert.
endTime|integer(int32)|example: 1512982919<br/>End timestamp of the alert (epoch time in seconds). May be empty for non 'closed' alerts.
channels| object array | Array of channels associated with the alert.
metrics | object array | Array of metrics different types of alerts hold different metric interval objects.

## Get Alert Trigger by Group Id

> Request Structure: GET /trigger-group/{groupId}

Find trigger by group Id
Fetch a single alert grouptrigger instance.

### Request Arguments

Argument | Type | Description
---------|------|------------
groupId | string | ID of the alert group **required**

>Example Response:

```json
{
  "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
  "timeScale": "5m",
  "type": "anomaly",
  "status": "open",
  "groupId": "20262fb3-00ba-412c-a515-aec74c4824ca",
  "alertConfigurationId": "20262fb3-00ba-412c-a515-aec74c4824ca",
  "title": "Jetty servers availability",
  "description": "This alerts monitors the availablity of the jetty servers",
  "severity": "critical",
  "startTime": 1512882919,
  "updatedTime": 1512882919,
  "endTime": 1512982919,
  "channels": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
      "type": "email",
      "name": "NOC Channel"
    }
  ],
  "metrics": [
    {
      "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
      "name": "what=cpu.metric=cpuIdle.server=anodot.dc=ny",
      "what": "cpu",
      "properties": [
        {
          "key": "server",
          "value": "anodot32"
        }
      ],
      "tags": [
        {
          "key": "server",
          "value": "anodot32"
        }
      ],
      "snooze": {
        "userId": "20262fb3-00ba-412c-a515-aec74c4824ca",
        "snoozeTime": 1525586116,
        "resumeTime": 1525586116
      },
      "origin": {
        "type": "alert",
        "id": "20262fb3-00ba-412c-a515-aec74c4824ca",
        "title": "GA sessions of my application"
      },
      "status": "closed",
      "updatedTime": 1512982919,
      "displayStartTime": 1512982919,
      "displayEndTime": 1512982919,
      "intervals": [
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "anomalyId": "20262fb3-00ba-412c-a515-aec74c4824ca",
          "direction": "up",
          "score": 0.99,
          "peak": 45.99,
          "deltaAbsolute": 3.76,
          "deltaPercentage": 10.42
        },
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "lastSeenTime": 1512382919
        },
        {
          "status": "closed",
          "startTime": 1512882919,
          "endTime": 1512982919,
          "duration": 300,
          "threshold": 40.3,
          "direction": "up",
          "peak": 45.99
        }
      ]
    }
  ]
}
```

### Alert Response Fields

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
startTime|integer(int32)|example: 1512882919 <br/>Start timestamp of the alert (epoch time in seconds).
updatedTime|integer(int32) |example: 1512882919<br/>Last timestamp the alert was updated (epoch time in seconds).<br/> Update means that an open metric was closed or new metric was added to the alert.
endTime|integer(int32)|example: 1512982919<br/>End timestamp of the alert (epoch time in seconds). May be empty for non 'closed' alerts.
channels| object array | Array of channels associated with the alert.
metrics | object array | Array of metrics different types of alerts hold different metric interval objects.
