# Channels

> End Point **GET /api/v2/channels**

Channels are the outgoing integrations used to send alerts to their destinations.
Anodot supports multiple channel types.

Use the channel API endpoint to get the channels available in your account and link them to alerts you create using the alerts API.

Authentication type: [Access Token Authentication] (#access-tokens).

## Get channels

> Request Example: GET All channels in the account

```shell
curl -X GET \
https://app.anodot.com/api/v2/channels \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

> Request Example: GET All channels of type slack

```shell
curl -X GET \
https://app.anodot.com/api/v2/channels?type=slack \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

> Request Example: GET All channels of type slack containing "alert" in the name

```shell
curl -X GET \
https://app.anodot.com/api/v2/channels?type=slack&name=alert \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

### Request Arguments

Argument | Type | Description
---------|------|------------
type [**Optional**] | string | Limit the response to this channel type.<br/>Use single, lowercase word. Possible values are listed in the [channel list] (#channel-list) below.
name [**Optional**] | string | Limit the response to anodot channel names containing this string

### Channel List

Type | Description
-----| -----------
email | Create an email and send to participants in the email distribution list
webhook | Send the alert as a JSON object to a webhook server
slack | Post the alert as a slack message on a slack channel
pagerduty | Create an event in a PagerDuty service
jira | Open the alert as ticket in a JIRA project
opsgenie | Create an OpsGenie alert
msteams | Post the alert as an MS Teams message on an MS Teams channel
tamtam | Send the alert as a TamTam message

> Response Example:

```json
[
    {
        "id": "2b7465e5-dbda-46f5-ae67-xxxxxyyyyy",
        "name": "Test",
        "type": "email"
    },
    {
        "id": "558ef7a2-7064-49be-873d-xxxxxyyyyy",
        "name": "test slack",
        "type": "slack"
    }
] 
```

### Response Fields

The response is a list of channels

Field | Type | Description / Example
-|-|-
id | String ($uuid) | channel id. Use this id when you create alerts via API and need to provide a destination channel.
type | String | Channel type. See possible values in [channel list] (#channel-list)
name | String | Channel name.

