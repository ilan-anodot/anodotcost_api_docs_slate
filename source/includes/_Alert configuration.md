# Alert Configuration

> End Point prefix is **/api/v2/alerts**


Use the *Alert Config API* to:

* Fetch all alert configurations
* Create a new alert
* Edit an existing alert
* Pause / Resume an alert
* Delete an existing alert


Authentication type: [Access Token Authentication] (#access-tokens).

## GET alerts

> Request Example: Get All Alert Configurations

```shell
curl -X GET \
"https://app.anodot.com/api/v2/alerts" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to get All Alert Configurations and their respective ID's

<aside class="success">
A Pro Tip:</br>
Use this API to retrieve the alert ID's that are used to delete, edit, pause/resume alerts.
</aside>

### Response Fields

Selected Response Fields:

Field | Type | Description / Example
-|-|-
id | String | Alert id. This id can be used in future calls for alert creation.
Meta | Array | Alert meta deta such as creation details (time, owner, etc.). 
Configuration | Array | The configuration details for the alert. For a detailed breakdown, see [POST alerts] (#post-alerts).
State | Array | The current state of the alert (paused/live)

> Response Example:

```json
[
  {
    "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
    "meta": {
      "createdTime": 1513760272,
      "modifiedTime": 1513760272,
      "createdBy": "string",
      "modifiedBy": "string",
      "creatorId": "20262fb3-00ba-412c-a515-aec74c4824cd",
      "modifierId": "20262fb3-00ba-412c-a515-aec74c4824cd",
      "associatedDashboardTile": {
        "dashboardId": "20262fb3-00ba-412c-a515-aec74c4824cd",
        "tileId": "30262fb3-00ba-412c-a515-aec74c4824cd"
      }
    },
    "configuration": {
      "timeScale": "5m",
      "type": [
        "anomaly"
      ],
      "title": "Jetty servers availability",
      "owner": "user@domain.com",
      "description": "This alerts monitors the availablity of the jetty servers",
      "search": {
        "type": "expression",
        "expression": {
          "expression": [
            {
              "type": "string",
              "key": "host",
              "value": "anodot13",
              "isExact": true
            }
          ]
        },
        "compositeId": "20262fb3-00ba-412c-a515-aec74c4824cd"
      },
      "severity": "critical",
      "isOnlyOpen": false,
      "channels": [
        "20262fb3-00ba-412c-a515-aec74c4824cd"
      ],
      "subscribers": [
        "20262fb3-00ba-412c-a515-aec74c4824cd"
      ],
      "conditions": [
        {
          "type": "delta",
          "duration": 1200
        },
        {
          "type": "delta",
          "duration": 300
        },
        {
          "type": "delta",
          "absolute": 170.3,
          "percentage": 20
        },
        {
          "type": "delta",
          "direction": "up"
        },
        {
          "type": "delta",
          "upperBound": "greaterThan",
          "upperValue": 827.12,
          "lowerBound": "lessThan",
          "lowerValue": 342.89
        },
        {
          "type": "delta",
          "absolute": 4267,
          "percentage": 30.7
        },
        {
          "type": "delta",
          "absolute": 4267,
          "percentage": 30.7
        },
        {
          "type": "delta",
          "significance": 0.35
        }
      ],
      "correlatedEvents": {
        "query": {
          "expression": [
            {
              "type": "string",
              "key": "host",
              "value": "anodot13",
              "isExact": true
            }
          ]
        }
      }
    },
    "state": {
      "state": "string",
      "resumeTime": 1513760272,
      "pausedTime": 1513760272,
      "pausedBy": "string",
      "pausedId": "string"
    },
    "validation": {
      "isValid": true,
      "general": [
        {
          "id": 2987,
          "message": "Alert excceded maximum amount of allowed metrics."
        }
      ]
    }
  }
]
```

## POST alerts

> Request Example: Create a new alert

```shell
curl -X POST \
"https://app.anodot.com/api/v2/alerts" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

>Request JSON Example:

```json
{
  "timeScale": "5m",
  "type": [
    "anomaly"
  ],
  "title": "Jetty servers availability",
  "description": "This alerts monitors the availablity of the jetty servers",
  "search": {
    "type": "expression",
    "expression": {
      "expression": [
        {
          "type": "string",
          "key": "host",
          "value": "anodot13",
          "isExact": true
        }
      ]
    },
    "compositeId": "20262fb3-00ba-412c-a515-aec74c4824cd"
  },
  "severity": "critical",
  "isOnlyOpen": false,
  "channels": [
    "20262fb3-00ba-412c-a515-aec74c4824cd"
  ],
  "subscribers": [
    "20262fb3-00ba-412c-a515-aec74c4824cd"
  ],
  "conditions": [
    {
      "type": "delta",
      "duration": 1200
    },
    {
      "type": "delta",
      "duration": 300
    },
    {
      "type": "delta",
      "absolute": 170.3,
      "percentage": 20
    },
    {
      "type": "delta",
      "direction": "up"
    },
    {
      "type": "delta",
      "upperBound": "greaterThan",
      "upperValue": 827.12,
      "lowerBound": "lessThan",
      "lowerValue": 342.89
    },
    {
      "type": "delta",
      "absolute": 4267,
      "percentage": 30.7
    },
    {
      "type": "delta",
      "absolute": 4267,
      "percentage": 30.7
    },
    {
      "type": "delta",
      "significance": 0.35
    }
  ],
  "correlatedEvents": {
    "query": {
      "expression": [
        {
          "type": "string",
          "key": "host",
          "value": "anodot13",
          "isExact": true
        }
      ]
    }
  }
}
```

Create a new alert configuration.</br>
Please note, the new alert owner will be the token owner.

<aside class="success">
A Pro Tip:</br>
Get example alert configurations by calling the GET alerts API. You can then use the "configuration": {} section as a jumping off point to creating your own alerts.
</aside>

### Request Arguments

Selected Request Arguments:

Argument | Type | Description
---------|------|------------
timescale | String | The timescale of the alert, valid options include (1m,5m,1h,1d,1w)
type | String | The type of anomaly (anomaly, static, no data)
title | String | The title that will appear when an alert is fired
description | String | The description that will appear when an alert is fired
search | Array | The alert query. Search by a key/value pair or via a stream ID. To get stream IDs, please see [GET data streams] (#get-data-streams).
severity | String | Add additional meta data (critical,high,medium,low and info)
isOnlyOpen | Boolean | Enable to get notifications on alert start and alert close
channels | String | Use this to determine which channels the alert will fire to. To get channel IDs, please see [GET channels] (#get-channels).
subscribers | String | Use this to determine which user's email the alert will fire to. To get subscriber IDs, use the GET alerts API to see an example of an alert that has already been set up with the relevant subscriber ID.
conditions | Array | Anomaly alerts require: direction, significance and duration conditions. Static alers require duration and threshold conditions. No data alerts require the no data duration condition. 
correlatedEvents | Array | Determines if static events that are sent via the events API are included in the alert. Filter out events based on a key:value pair.

## GET alerts/{alertId}

>Request Example:

```shell
curl -X GET \
"https://app.anodot.com/api/alerts/{alertID}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to get a single alert configuration

### Request Arguments

Argument | Type | Description
---------|------|------------
id | String | Alert id to retrieve.


## PUT alerts/{alertId}

>Request Example:

```shell
curl -X PUT \
"https://app.anodot.com/api/v2/alerts/{alertID}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to edit an alert

### Request Arguments

Argument | Type | Description
---------|------|------------
id | String | Alert id to edit.

## DELETE alerts/{alertId}

>Request Example:

```shell
curl -X DELETE \
"https://app.anodot.com/api/v2/alerts/{alertID}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to delete an alert

### Request Arguments

Argument | Type | Description
---------|------|------------
id | String | Alert id to delete.

## POST alerts/{alertId}/pause

>Request Example:

```shell
curl -X POST \
"https://app.anodot.com/api/v2/alerts/{alertID}/pause" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to pause an alert.

### Request Arguments

Argument | Type | Description
---------|------|------------
id | String | Alert id to pause.

## POST alerts/{alertId}/resume

>Request Example:

```shell
curl -X POST \
"https://app.anodot.com/api/v2/alerts/{alertID}/resume" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to resume an alert.

### Request Arguments

Argument | Type | Description
---------|------|------------
id | String | Alert id to resume.
