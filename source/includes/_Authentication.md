# Authentication

## Basic Authentication

> Using the basic token in a REST call:

```shell
POST 'https://{{app-url}}/api/v1/metrics?token=${DC_Key}&protocol=anodot20'
```

Basic authentication is used for:

* Sending metrics to Anodot.
* Calling Anodot legacy APIs. 

Anodot basic authentication uses a **Data Collection Key**.
Copy the **Data Collection Key** from the [Token Management](https://support.anodot.com/hc/en-us/articles/360002631114-Token-Management) page and use it in your REST calls.

<aside class="warning">
We recommend using Access Token based Authentication for all purposes other than posting metrics.
See details below.
</aside>

## Access Tokens

To use access token authentication, follow these 3 simple steps:

**Step 1:** Create an access-key in the [Token Management](https://support.anodot.com/hc/en-us/articles/360002631114-Token-Management) page within Anodot.

> Endpoint: **/access-token** :

> Request example:

```shell
POST 'https://{{app-url}}/api/v2/access-token/?responseformat=JSON' \
--header 'Content-Type: application/json' \
--data-raw '{
	"refreshToken" : "{{data-token}}"
}
'
```
### Request Arguments

Argument | Description
---------|------|------------
refreshToken | Copied from Anodot app, see step 1
responseformat [**Optional**] | Can be set to JSON (case sensitive!) if you would like to get the response in a json format.

> The Response contains the access-token. Option 1 is in plain-text:

```shell
"{{bearer-token-string}}"
```

> Second option is in a JSON format:

```json
{
    "token" : "{{bearer-token-string}}"
}
```


**Step 2:** Use the access-key's token to request an access-token.</br>The retrieved access-token is valid for 24 hours.

*Response Codes:*

Code | Description
---- | -----------
200 | OK - The response includes a token
401 | NOK - Token Mismatch

> Using the token you get in subsequent calls:

```shell
curl -X GET \
https://{{app-url}}.anodot.com/api/v2/feedbacks \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer {{bearer-token}}"
-d '{"startTime" : 1578391000, "endTime": 1578392000}'
```

**Step 3:** Use the retrieved access-token in your subsequent API calls<br/>
Set the Authorization header of your calls with *Bearer* and the retrieved access-token.

<aside class="notice">
Remember:<br/>Access token expiration is controlled via the 'User session timeout' configuration in the settings page, under the Authentication tab.
</aside>

## Get CustomerID

Each customer has a unique customerID which can be useful for some scenarios. 

> End Point prefix: **/api/v2/customers/id**

> Request example:

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/customers/id' \
--header 'Authorization: Bearer {{Bearer-Token}}' \
--data-raw ''
'
```
### Request Arguments

None.

> The Response contains the customerID. 

```json
"6062047f0f6585000e1a7107"
```
