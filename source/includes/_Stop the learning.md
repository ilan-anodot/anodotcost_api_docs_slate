# Stop the Learning

> End Point prefix is **/api/v1/metrics**

Authentication type: [Access Token Authentication] (#access-tokens).

When should you use the *STL API* ?

On occassions, metrics become very noisy, or out of their regular bounds. This may create several effects:

* The number of alerts sent on account of such metrics might increase, resulting in alert overflow, and possibly hiding alerts occuring on other metrics.
* The baseline learnt by Anodot may be convoluted due to the different values the metric gets.

To handle these scenarios, the *STL API* allows you to:

* Snooze the learning of noisy metrics, thus prevening the side effects.
* Resume the learning when the metrics resume close to normal values.

## Snooze Learning

> Request Example: POST /stl/add

```json
{
  "userId": "5b7552b68a9d6b000d624bdb",
  "timeScale": "5m",
  "duration": "3600",
  "metricIds": "[m1,m2]"
}
```

Use this request to snooze the learning of the metrics to a designated time.

### Request Arguments

Argument | Type | Description
---------|------|------------
userId | string | The user requesting to snooze the learning
timescale | Enum | The timescale to be snoozed.</br>This will be the timescale used in the noisy alert that we want to silence.</br>Valid values:
duration | number | The requested snooze duration in seconds.
metricIds | Array of strings | The array of metric ids we wish to snooze. 

### Response

## Resume Learning

> Request Example: POST /stl/remove

```json
{
  "userId": "5b7552b68a9d6b000d624bdb",
  "timeScale": "5m",
  "duration": "3600",
  "metricIds": "[m1,m2]"
}
```

Use this request to resume the learning of the metrics you have snoozed earlier.

### Request Arguments

Argument | Type | Description
---------|------|------------
userId | string | The user requesting to resume the learning
timescale | Enum | The timescale to be resumed.</br>This will be the timescale used in the metric we have snoozed earlier.</br>Valid values:
duration | number | The requested resume duration in seconds.
metricIds | Array of strings | The array of metric ids we wish to resume. 