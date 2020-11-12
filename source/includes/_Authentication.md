# Authentication

## Basic Authentication

> Using the basic token in a REST call:

```shell
POST https://api.anodot.com/api/v1/metrics?token=${DC_Key}&protocol=anodot20
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

> Step 1: Create an access key in Anodot App

**Step 1:** Create an access-key in the [Token Management](https://support.anodot.com/hc/en-us/articles/360002631114-Token-Management) page within Anodot.

> Step 2: Request the access-token

> Endpoint: **/access-token** : </br>
> Request argument: **refreshToken** Copied from Anodot app, see step 1</br>

```shell
curl -X POST \
https://app.anodot.com/api/v2/access-token \
-H 'Content-Type: application/json' \
-d '{"refreshToken": "722354b89ae60761f9fcxcxca613315c"}'
```

> The Response contains the access-token</br>

```shell
"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkYXRhIjoiMDgwNjVlYjA3ZTI4ZTRiZGVlZjkxZGJlMTJiNWUyYjU1MjNjMjA4KTAwNjBjODI1YjYwM2M0MGJkOThlMThlYjQ2ZDMyMWIxZDIiLCJpYXQiOjE1NTM2ODU4OTksImV4cCI6MTU1NjI3Nzg5OX0.TOE5ZNr6yL0wyLsB1RQEA8CV-S6zzUEafkXLwHIY76k"
```

**Step 2:** Use the access-key's token to request an access-token.</br>The retrieved access-token is valid for 24 hours.

*Response Codes:*

Code | Description
---- | -----------
200 | OK - The response includes a token
401 | NOK - Token Mismatch

> Step 3: Use the access-token in REST calls

```shell
curl -X GET \
https://app.anodot.com/api/v2/feedbacks \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkYXRhIjoiMDgwNjVlYjA3ZTI4ZTRiZGVlZjkxZGJlMTJiNWUyYjU1MjNjMjA4KTAwNjBjODI1YjYwM2M0MGJkOThlMThlYjQ2ZDMyMWIxZDIiLCJpYXQiOjE1NTM2ODU4OTksImV4cCI6MTU1NjI3Nzg5OX0.TOE5ZNr6yL0wyLsB1RQEA8CV-S6zzUEafkXLwHIY76k"
-d '{"startTime" : 1578391000, "endTime": 1578392000}'
```

**Step 3:** Use the retrieved access-token in your REST calls<br/>
Set the Authorization header of your calls with *Bearer* and the retrieved access-token.

<aside class="notice">
Remember:<br/>Access tokens are valid for 24 hours. Include periodic calls in your code to get an updated token.
</aside>