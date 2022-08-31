## Authentication

### Basic Authentication

> Using the basic token in a REST call:

```shell
POST 'https://{{app-url}}/api/v1/metrics?token={{DC_Key}}&protocol=anodot20'
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

### Access Tokens

> Endpoint: **/access-token** :

> Step 2 - Request example:

```shell
POST 'https://{{app-url}}/api/v2/access-token/?responseformat=JSON' \
--header 'Content-Type: application/json' \
--data-raw '{
	"refreshToken" : "{{access-key}}"
}
'
```

> Step 2 response - contains the access-token
> Option 1 is in plain-text:

```shell
"{{bearer-token-string}}"
```

> Option 2 is in JSON format:

```json
{
    "token" : "{{bearer-token-string}}"
}
```

> Step 3 - Using the token you get in subsequent calls:

```shell
curl -X GET \
https://{{app-url}}.anodot.com/api/v2/feedbacks \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer {{bearer-token}}"
-d '{"startTime" : 1578391000, "endTime": 1578392000}'
```

To use access token authentication, follow these 3 simple steps:

**Step 1:** Create an access-key in the [Token Management](https://support.anodot.com/hc/en-us/articles/360002631114-Token-Management) page within Anodot.

**Step 2:** Use the access-key's token to request a bearer-token.</br>The retrieved bearer-token expiration is controlled via the 'User session timeout' configuration in the settings page, under the Authentication tab.

**Step 3:** Use the retrieved bearer-token in your subsequent API calls<br/>
Set the Authorization header of your calls with *Bearer* and the retrieved bearer-token.

<aside class="notice">
Checking the token expiration:<br/>
The token made up of 3 sections, divided by dots.</br> 
Extract the second section. Decode it from Base64</br>
The object you get contains an "exp" part, this is a UNIX formatted time indicating the token's expiration time</br>
Create the logic in your code to support token refresh before you reach the expiration time.</br>
</aside>

*Step 2 Request Arguments*

Argument | Description
---------|------|------------
refreshToken | Copied from Anodot app, see step 1
responseformat [**Optional**] | Can be set to JSON (case sensitive!) if you would like to get the response in a json format.

*Response Codes:*

Code | Description
---- | -----------
200 | OK - The response includes a token
401 | NOK - Token Mismatch

### Get CustomerID

Each customer has a unique customerID which can be useful for some scenarios. 

> End Point prefix: **/api/v2/customers/id**

> Request example:

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/customers/id' \
--header 'Authorization: Bearer {{Bearer-Token}}' \
--data-raw ''
'
```
**Request Arguments**

None.

> The Response contains the customerID. 

```json
"6062047f0f6585000e1a7107"
```
