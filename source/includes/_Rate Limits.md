## Rate Limits

The Anodot Cost APIs are limited according to the guidelines below.</br>

<aside class="notice">
Request rate limits are based on requests per apikey and API end point<br/>
Each apikey + API end point combo has its own limit.</br>
</aside>


| Processing Type | Values | Behavior |
|-----------------|--------|----------|
| Standard | Up to 500 reqeusts in a 10 minute window AND</br> Up to 250 requests in a 3 minute window | Standard processing |
| Delayed | Over 500 requests in a 10 minute window | Requests are delayed |
| Blocked | Over 250 requests in a 3 minute window  | Requests are blocked |
