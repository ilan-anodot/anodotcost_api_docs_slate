## Alert Actions

> End Point prefix is **/api/v2/alert-actions**


Using this set of API calls you can manage the Alert Actions defined in your account. For more information about Alert actions, please click [here](https://support.anodot.com/hc/en-us/articles/360019505219-Actions).

Authentication type: [Access Token Authentication] (#access-tokens).

### List all alert actions

> Request Example: Get All Alert Actions

```shell
curl -X GET \
"https://app.anodot.com/api/v2/alert-actions" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer {{bearer-token}}"
```

Use this API to get All Alert actions and their respective ID's

> Response Example:

```json
[
    {
        "id": "61f8fad5b7eda06537d01ed2",
        "userId": "61f8f55360635d000f2c8e5d",
        "actionType": "OUTSIDE_LINK",
        "actionName": "Grafana Name",
        "btnName": "Grafana Button Name",
        "data": {
            "url": "https://acme.corp.com/grafana"
        },
        "created": 1643707093,
        "modified": 1643707093
    },
    {
        "id": "61f8fb01b7eda06537d01ed3",
        "userId": "61f8f55360635d000f2c8e5d",
        "actionType": "OUTSIDE_LINK",
        "actionName": "New Jira Name",
        "btnName": "New Jira Button",
        "data": {
            "url": "https://www.acme.com/jira"
        },
        "created": 1643707137,
        "modified": 1643728479
    }
]
```

**Response Fields**

This call will return an array of alert actions, each one with the following structure:

Field | Type | Description / Example
-|-|-
id | String | Alert action id. This id can be used in future calls for action modification / deletion.
userId | String | ID of the action owner (The user which created the action) 
actionType | Array | The type of the action defined. Currently - only "OUTSIDE_LINK" actions are supported, but in the future additional types will be added.
actionName | String | Name of the action as it shows up inside Anodot. Note that this is different than the text which is actually shown on the button when the alert recipient activates the action.
btnName | String | The text which appears 
data | Array | Relevant parameters for defining the action. For OUTSIDE_LINK actions, the only parameter is url.
created | epoch | timestamp of when the action was created
modified | epoch | timestamp of when the action was modified



### Get alert action by ID

> Request Example: Get alert action by ID

```shell
curl -X GET \
"https://app.anodot.com/api/v2/alert-actions/{{action-id}}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer {{bearer-token}}"
```

Use this API to get a specific alert action.

**Request Arguments**

Argument | Type | Description
---------|------|------------
id | String | Alert action id to retrieve.

> Response Example:

```json
[
    {
        "id": "61f9579eb7eda06537d01ed5",
        "userId": "61f8f55360635d000f2c8e5d",
        "actionType": "OUTSIDE_LINK",
        "actionName": "Postman Glory",
        "btnName": "Postman Button Name",
        "data": {
            "url": "https://acme.corp.com/postman"
        },
        "created": 1643730846,
        "modified": 1643730846
    }
]
```

**Response Fields**

Similar to the results returned by the [List all alert actions] (#list-all-alert-actions) call. 

### Create a new alert action

Create a new alert action.</br>

<aside class="success">
A Pro Tip:</br>
Get example alert action parameters by calling the GET alerts action API. You can then use the configuration you got as a jumping off point to creating your own alerts actions.
</aside>

> Request Example: Create a new alert action

```shell
curl -X POST \
curl --location --request POST 'https://app.anodot.com/api/v2/alert-actions/create' \
--header 'Authorization: Bearer {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '[
  {
    "userId": "61f8f55360635d000f2c8e5d",
    "actionType": "OUTSIDE_LINK",
        "actionName": "Postman Glory",
        "btnName": "Postman Button Name",
        "data": {
            "url": "https://acme.corp.com/postman"
        }
    }
]'
```

**Request Arguments**

Selected Request Arguments:

Argument | Type | Description
---------|------|------------
userId | String | ID of the action owner (The user which created the action) 
actionType | Array | The type of the action defined. Currently - only "OUTSIDE_LINK" actions are supported, but in the future additional types will be added.
actionName | String | Name of the action as it shows up inside Anodot. Note that this is different than the text which is actually shown on the button when the alert recipient activates the action.
btnName | String | The text which appears 
data | Array | Relevant parameters for defining the action. For OUTSIDE_LINK actions, the only parameter is url.

> Response Example:

```json
[
    {
        "id": "61fa747483efad63412d2ca3",
        "userId": "61f8f55360635d000f2c8e5d",
        "actionType": "OUTSIDE_LINK",
        "actionName": "Postman Glory",
        "btnName": "Postman Button Name",
        "data": {
            "url": "https://acme.corp.com/postman"
        },
        "created": 1643803764,
        "modified": 1643803764
    }
]
```

**Response Fields**

The response will show the parameters of the created alert action together with the newly created ID and the created/modified time. 

### Update Alert Action

Use this API to edit an alert action

>Request Example:

```shell
curl -X PUT \
"https://app.anodot.com/api/v2/alert-actions/{{actionID}}" \
--header 'Authorization: Bearer {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "id": "61fa747483efad63412d2ca3",
        "userId": "61f8f55360635d000f2c8e5d",
        "actionType": "OUTSIDE_LINK",
        "actionName": "What'\''s the story with",
        "btnName": "Morning Glory",
        "data": {
            "url": "https://www.acme.com/Oasis"
        }
    }
]'
```

**Request Arguments**

Argument | Type | Description
---------|------|------------
id | String | Alert action id to edit.

Notice that you need to pass the ID of the action on the URL itself as well as the in the *body* of the call the updated definition of the action. The definition is the same as the one used in the alert action creation API. 

**Response**

The updated configuration of the action (same structure as the Get Alert action call)


### Delete alert action

Use this API to delete an alert action

>Request Example:

```shell
curl --location --request DELETE 'https://app.anodot.com/api/v2/alert-actions/{{actionID}}' \
--header 'Authorization: Bearer {{bearer-token}}' \
--data-raw ''"
```

**Request Arguments**

Argument | Type | Description
---------|------|------------
id | String | Alert action id to delete.

> Response Example:

```json
{
    "numDeleted": 1
}
```

**Response Fields**

Assuming the operation was sucessfull you will recieve in the body of the reponse a confirmation how many alerts were deleted (supposed to be 1)
