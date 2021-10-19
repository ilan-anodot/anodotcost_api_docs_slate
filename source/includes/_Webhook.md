# Anodot Webhook Channel



The Anodot Webhook channel is used by customers to create custom handling of Alert triggers. The trigger is sent to a webhook URL the customer specifies and then you can create your own handling of the payload you get from Anodot. Instructions on setting up the webhook from the Anodot application can be found  - [here](https://support.anodot.com/hc/en-us/articles/360006494374-Anomaly-Alert-Webhook-Formats)

The Anodot Webhook format has four variants, based on the Alert type:

* [Anomaly Alert](#anomaly-alert)
* [Anomaly Alert - Extended](#anomaly-alert-extended)
* [Static Alert](#static-alert)
* [No Data Alert](#no-data-alert)

Below you can see the format for each alert and sample payloads. 

<aside class="notice">
Epoch time is always in seconds (UTC time).</br>
The alert templates show the structure (with iterators if a number of metrics and alerts descriptions are combined to the same alert message).
</aside>

### Main Fields
Field | Description
------|------------
subject | Alert subject - same as alert email. Taken from Alert title
severity | Determined by the alert with the highest severity amongst the alerts included in the webhook. Possible values: info, low, medium, high, critical
description | Description of the alert who’s title was used in the subject
investigationUrl	| Link to Anoboard within Anodot for investigation purposes
startTime	| Human-readable start time of 1st alert in the anomaly
startTimeEpoch	| startTime in Epoch
timescale	| Alert timescale values: 1min, 5min, 1 hour, 1 day, 1 week
type	| Alert type: Anomaly / Static / No Data
mergedAnomalies	| An array of all merged anomalies
alerts | An array of all alerts included in this triggered instance
metrics	| An array of all metrics in each alert
direction	| Up/down or both. If both, then the upper/lower are relevant
delta | For backward compatibility
name	| Metric name (including tags and user given names for composite)
id	| Metric id
state	| Specific metric state. Values: OPEN, CLOSED
season | Values: "None" if no season, or season value (2w, 1w, 1d, 12h, 6h, 5h, 4h, 3h, 2h, 1h, 30m, 15m, 10m, 5m).
events	| If correlated in the alert - User events within the time of the alert metrics
buckets	| An array of the events grouped by dates
topEvents	| Top X events for the alert
alertid	| Specific alert id
AlertSettingsURl	| Link to specific alert configuration
description	| Specific alert description
severity | Specific alert severity. Possible values: info, low, medium, high, critical
alertGroupId | A unique identifier for the entire incident. This is useful if you would like to collate the different notifications of a specific incidents together - i.e. getting the 'open', 'update' and 'close' messages of the same incident together. They will all have the same alertGroupId.

<aside class="notice">
Note on seasonality: If additional seasons are added, the string representation remains the same:</br>
w - week</br>
d - day</br>
h - hour</br>
m - minute</br>
</aside>

## Anomaly Alert 

Anomaly alerts are the main type of Anodot alerts. Notice that Anomaly alerts have two versions for the webhook format - standard and extended. The extended version is open to select customers upon request. Please send a mail to [support@anodot.com](mailto:support@anodot.com) if you would like to start using the extended version. 
On the right you can see the template and a sample of the standard Anomaly alert trigger. 

> Anomaly Alert Template: 

```json
{
"subject": "{{subject}}",
"severity": "{{severity}}",
"description": "{{description}}",
"investigationUrl": "{{The link to the Anoboard}}",
"startTime": "{{startTime}} (UTC)",
"startTimeEpoch": "{{startTimeEpoch}}",
"anomalyId": "{{anomalyId}}",
"alertGroupId": "{{alertGroupId}}",
"mergedAnomalies":"[{{anomalyId}}]",
"timeScale": "timeScale",
"alerts": [
{
    "title": "{{title}}",
    "metrics": [
        {
        "closeReasonPhrase": "{{closeReasonPhrase}}",
        "duration": "{{duration}}",
        "durationInSeconds": "{{durationInSeconds}}",
        "startTime": "{{startTime}} (UTC)",
        "startTimeEpoch": "{{startTimeEpoch}}",
        "imageUrl": "{{imageUrl}}",
        "peak": "{{peak}}",
        "direction": "{{direction}}",
        "delta": "{{delta}}",
        "significance" : "{{score}}",
        "name": "{{name}}",
        "id":"{{id}}",
        "state": "{{state}}",
        "season":"{{season}}"
        }
    ],
    "events":{
        "Total":"{{total Events}}",
        "buckets":[
        {
        "date":"{{date}}",
        "Total":"{{total events in aggregation}}",
        "topEvents":[
            {
            "title" :"{{title}}",
            "description" :"{{description}}",
            "source" :"{{source}}",
            "category" : "{{category}}",
            "startDate" :"{{startDate}}",
            "endDate" : "{{endDate}}"
            }
        ]
    }
    ]
},
    "alertId": "{{alertId}}",
    "alertSettingsUrl": "{{The link to the alert’s setting}}",
    "description": "{{description}}",
    "severity": "{{severity}}"
    }
]
}
```

> Anomaly Alert Response Example (With Multiple Metrics):

```json
 {
  "subject": "(2 Alerts) Close: [6-1. Automation DAL1] and [6-2. Automation DAL2] [critical][a42ba]",
  "severity": "critical",
 "description": "should generate one alert story with DAL1 contains metrics m1(up) and DAL2 contains metrics m4(down)",   
 "investigationUrl": "https://yourdomain.anodot.com/#!/anomalies?tabs=main;0&activeTab=1&anomalies=;0(a42bab2f70f44ec8a420cc2130cc47bb)&duration=;1(1)&durationScale=;minutes(minutes)&delta=;1(1)&deltaType=;percentage(percentage)&resolution=;short(short)&score=;0(0)&state=;both(both)&direction=;both(both)&bookmark=;()&alertId=;(4fd71678-cd35-4248-ae2b-7578201cde78,33187cca-680c-4a42-a067-1d817ddb28dc)&sort=;significance(significance)&q=;()&constRange=;1h(c)&startDate=;0(0)&endDate=;0(0)",
  "startTime": "14 Nov, 2018 9:17AM (UTC)",
  "startTimeEpoch": "1542187020",
  "anomalyId": "2aefdac1-1a07-4b7b-bf84-09bf5b34f917",
  "alertGroupId": "2aefdac1-1a07-4b7b-bf84-09bf5b34f917",
  "mergedAnomalies": "[a42bab2f70f44ec8a420cc2130cc47bb]",
  "timeScale": "1m",
  "type": "Anomaly",
  "alerts": [
    {
      "title": "6-2. Automation DAL2 1542267963737",
      "metrics": [
        {
          "duration": "20m",
          "durationInSeconds": "1200",
          "startTime": "14 Nov, 2018 9:17AM (UTC)",
          "startTimeEpoch": "1542187020",
          "imageUrl": "https://alert-images-staging.s3.amazonaws.com/i:33187cca-680c
-4a42-a067-1d817ddb28dce:f855728768f0a8d979581e8fb3dd5320s:close.png",
          "peak": "-500.0",
          "lowerPeak": "-500",
          "direction": "DOWN",
          "delta": 1000,
          "lowerPercentageDelta": 1000,
          "significance": 0,
          "name": "automation.alert.m4.1542267963737",
          "id": "automation.alert.m4.1542267963737",
          "state": "CLOSED",
          "season": "None"
        }
      ],
      "events": {
        "total": "0",
        "buckets": []
      },
      "alertId": "dc23e0ad-5561-4742-b123-af7046659d4e",
      "alertSettingsUrl": "https://yourdomain.anodot.com/#!/alerts/dc23e0ad-5561-4742-
b123-af7046659d4e",
      "description": "should generate one alert story with DAL1 contains metrics m1(up)
 and DAL2 contains metrics m4(down)",
      "severity": "critical"
    },
    {
      "title": "6-1. Automation DAL1 1542267963737",
      "metrics": [
        {
          "duration": "20m",
          "durationInSeconds": "1200",
          "startTime": "14 Nov, 2018 9:17AM (UTC)",
          "startTimeEpoch": "1542187020",
          "imageUrl": "https://alert-images-staging.s3.amazonaws.com/i:4fd71678-cd35-4248-ae2b-7578201cde78e:14e50f7a6cd06fc2efa0b542c73dc3fcs:close.png",
          "peak": "500.0",
          "upperPeak": "500",
          "direction": "UP",
          "delta": 1000,
          "upperPercentageDelta": 1000,
          "significance": 0,
          "name": "automation.alert.m1.1542267963737",
          "id": "automation.alert.m1.1542267963737",
          "state": "CLOSED",
          "season": "None"
        }
      ],
      "events": {
        "total": "0",
        "buckets": []
      },
      "alertId": "04d7af9d-f892-4d50-8a1a-eb5297247f4f",
      "alertSettingsUrl": "https://yourdomain.anodot.com/#!/alerts/04d7af9d-f892-4d50-8a1a-eb5297247f4f",
      "description": "should generate one alert story with DAL1 contains metrics m1(up)
 and DAL2 contains metrics m4(down)",
      "severity": "critical"
    }
  ]
} 
```

## Anomaly Alert - Extended 
In the extended version, the following fields are added to the webhook payload (not necessarily at the end of the 'standard' payload, so please pay attention to the template and example on the right)

Field | Description
------|------------
upperAbsoluteDelta</br>lowerAbsoluteDelta | The upper/lower absolute delta value (the difference in the "from" and "to" values below) 
from | The initial delta value
to | The final delta value
closeReasonPhrase | Whenever an alert is closed, this field will specify the reason that alert is closed (field is per metric).
impact | Business impact data. An object containing the impact, currency and effect of an anomaly. For a deeper explanation on business impact, please read [here](https://support.anodot.com/hc/en-us/articles/360016317859-Measuring-Business-Impact)
actions | An array of actions defined by the alert owner. Each element in the array has a name, URL, buttonName and type. For a deeper explanation on actions, please read [here](https://support.anodot.com/hc/en-us/articles/360019505219-Actions). 
alertGroupStatus | An inidicator on the status of the alert group - it can be either 'open' or 'close'. It will turn to 'close' after *all* metrics in the alert gruop return to their normal baseline.   

> Anomaly Alert Extended Template: 

```json
{
"subject": "{{subject}}",
"severity": "{{severity}}",
"description": "{{description}}",
"investigationUrl": "{{appUrl}}/anomalies?tabs=main;0&activeTab=1&anomalies=;0({{alertGroupId}})&duration=;1(1)&durationScale=;minutes(minutes)&delta=;0(0)&deltaType=;percentage(percentage)&resolution=;{{rollup}}({{rollup}})&score=;0(0)&state=;both(both)&direction=;both(both)&bookmark=;()&alertId=;({{alertTriggerIds}})&sort=;significance(significance)&q=;()&constRange=;1h(c)&startDate=;0(0)&endDate=;0(0)",
"startTime": "{{startTime}} ({{timeZone}})",
"startTimeEpoch": "{{startTimeEpoch}}",
"anomalyId": "{{alertGroupId}}",
"alertGroupId": "{{alertGroupId}}",
"alertGroupStatus": "{{alertgGroupStatus}}"
"mergedAnomalies": "{{mergedAnomalies}}",
"timeScale": "{{timeScale}}",
"type" : "Anomaly",
"alerts": [
   {
   "title": "{{title}}",
   "alertStatus": "{{alertStatus}}",
   "metrics": [
     {
      "closeReasonPhrase": "This metric was last seen 50 minutes ago and has now timed out.",
      "duration": "{{duration}}",
      "durationInSeconds": "{{durationInSeconds}}",
      "startTime": "{{startTime}} ({{timeZone}})",
      "startTimeEpoch": "{{startTimeEpoch}}",
      "imageUrl": "{{imageUrl}}",
      "peak": "{{peak}}",
      "upperPeak": "{{upperPeak}}",
      "lowerPeak": "{{lowerPeak}}",
      "direction": "{{direction}}",
      "delta": "{{delta}}",
      "upperPercentageDelta": "{{upperPercentageDelta}}",
      "upperAbsoluteDelta": "{{upperAbsoluteDelta}}",
      "from" : "{{upperComparisonValue}}",
      "to" : "{{upperPeak}}",
      "lowerPercentageDelta": "{{lowerPercentageDelta}}",
      "lowerAbsoluteDelta": "{{lowerAbsoluteDelta}}",
      "from" : "{{lowerComparisonValue}}",
      "to" : "{{lowerPeak}}",
      "significance" : "{{score}}",
      "name": "{{name}}",
      "id": "{{id}}",
      "state": "{{state}}",
      "impactInfo": {
            "impact": "{{impact value}}",
            "currency": "{{impact currency}}",
            "effect": "{{impact direction}}"
          },
      "season": "{{season}}"
      }
   ],
   "triggereIds":"{{alertTriggerIds}}",
   "events":{
   "total":"{{totalUserEvents}}",
   "buckets":[
        {
       "date":"{{date}}",
       "total":"{{totalEvents}}",
       "topEvents":[
          {
          "title" :"{{title}}",
          "description" :"{{description}}",
          "source" :"{{source}}",
          "category" : "{{category}}",
          "startDate" :"{{startDate}}",
          "endDate" : "{{endDate}}"
          }
     ]
     }
  ]
  },
  "alertId": "{{alertConfigurationId}}",
  "alertSettingsUrl": "{{appUrl}}/alerts/{{alertConfigurationId}}",
  "description": "{{description}}",
  "severity": "{{severity}}"
  }
]
}
```

> Anomaly Alert Extended Response Example:

```json
 {
  "subject": "Alert Close: Smarttags alert Revenue daily stream per: 20365[high][bb9e7]",
  "severity": "high",
  "description": "",
  "investigationUrl": "http://yourdomain.anodot.com/#!/anomalies?tabs=main;0&activeTab=1&anomalies=;0(bb9e7e1e5cb64c468e11cdf95756c272)&duration=;1(1)&durationScale=;minutes(minutes)&delta=;0(0)&deltaType=;percentage(percentage)&resolution=;longlong(longlong)&score=;0(0)&state=;both(both)&direction=;both(both)&bookmark=;()&alertId=;(62cf1755-cc90-4994-b8e4-86e308455622)&sort=;significance(significance)&q=;()&constRange=;1h(c)&startDate=;0(0)&endDate=;0(0)",
  "startTime": "31 Aug, 2020 12:00AM (UTC)",
  "startTimeEpoch": "1598832000",
  "anomalyId": "2aefdac1-1a07-4b7b-bf84-09bf5b34f917",
  "alertGroupId": "2aefdac1-1a07-4b7b-bf84-09bf5b34f917",
  "alertGroupStatus": "OPEN",
  "mergedAnomalies": "[bb9e7e1e5cb64c468e11cdf95756b100]",
  "timeScale": "1d",
  "type": "Anomaly",
  "alerts": [
   { 
     "title": "Smarttags alert Revenue daily stream per: 20365",
     "alertStatus": "open",
     "metrics": [
    {
     "duration": "1d",
     "durationInSeconds": "86400",
     "startTime": "31 Aug, 2020 12:00AM (UTC)",
     "startTimeEpoch": "1598832000",
     "imageUrl": "",
     "peak": "17245.6",
     "upperPeak": "17245.6",
     "direction": "UP",
     "delta": 76,
     "upperPercentageDelta": 76,
     "upperAbsoluteDelta": 7442.942363620727,
     "from": 9802.65,
     "to": 17245.6,
     "significance": 66,
     "name": "what=commission_daily.agency_id=8.campaign_id=20365.channel_name=Google.currency_code=USD.ksname=KS1456.profile_id=14",
     "id": "bc.SSSKY4zhyR456.14.20365.8.Google.USD.KS1456.commission_daily",
     "state": "CLOSED",
     "impactInfo": {
            "impact": "0.2941702200631637",
            "currency": "USD",
            "effect": "bad"
      },
     "season": "None"
    }
  ],
  "triggereIds": [
  "62cf1755-cc90-4994-b8e4-86e308311425"
  ],
  "events": {
  "total": "0",
  "buckets": []
  },
  "actions": [
      {
        "name": "Run Remediation Script",
        "url": "jenkins://whatever",
        "buttonName": "Fix",
        "type": "OUTSIDE_LINK"
      },
      {
        "name": "Call 911",
        "url": "Someone please call 911",
        "buttonName": "Call 911",
        "type": "OUTSIDE_LINK"
      }
    ],
  "alertId": "7feaee91-0197-4278-a840-6f3fe832a7da",
  "alertSettingsUrl": "http://yourdomain.anodot.com/#!/alerts/7feaee91-0197-4211-a840-4fdfe832a7da",
  "description": "",
  "severity": "high"
  }
 ]
}
```
## Static Alert 

A static alert trigger is fired when a specified metric crosses a designated threshold. For details on Static alerts please see [here](https://support.anodot.com/hc/en-us/articles/360015467439-Creating-Static-and-No-Data-Alerts).

> Static Alert Template: 

```json
{
"subject": "{{subject}}",
"severity": "{{severity}}",
"description": "{{description}}",
"startTime": "{{startTime}} (UTC)",
"startTimeEpoch": "{{startTimeEpoch}}",
"alertGroupId": "{{alertGroupId}}",
"alertGroupStatus": "{{alertgGroupStatus}}",
"type": "{{type}}",
"alerts": [
   {
    "title": "{{title}}",
    "alertStatus": "{{alertStatus}}",
    "metrics": [
    {
        "duration": "{{duration}}",
        "durationInSeconds": "{{durationInSeconds}}",
        "startTime": "{{startTime}} (UTC)",
        "startTimeEpoch": "{{startTimeEpoch}}",
        "imageUrl": "{{imageUrl}}",
        "peak": "{{peak}}",
        "direction": "{{direction}}",
        "name": "{{name}}",
        "state": "{{state}}",
        "threshold": "{{threshold}}"
        }
    ],
   }
],
"events": { 
  "total":"{{total Events}}",
  "buckets": [
  {
    "date":"{{date}}",
    "Total":"{{total events in aggregation}}",
    "topEvents": [
      {
        "title" :"{{title}}",
        "description" :"{{description}}",
        "source" :"{{source}}",
        "category" : "{{category}}",
        "startDate" :"{{startDate}}",
        "endDate" : "{{endDate}}"
      }
    ]
  },
  ]
},
"alertId": "{{alertId}}",
"alertSettingsUrl": "{{Link to the alerts setting}}", 
"description" :"{{description}}",
"severity":"{{severity}}"
}
]
}
```


> Static Alert Response Example (With Multiple Metrics):

```json
 {
  "subject": "Alert Close: Static Alert Sample 1479763487178[critical][fcc7a]",
  "severity": "critical",
  "description": "Sample Static Alert", 
  "startTime": "11/19/2016 22:27:00 (UTC)",
  "startTimeEpoch": "1479594420",
  "alertGroupId": "2aefdac1-1a07-4b7b-bf84-09bf5b34f917",
  "alertGroupStatus": "OPEN",
  "type": "static",
  "alerts": [
    {
      "title": "“Static Alert Sample 1479763487178",
      "alertStatus": "open",
      "metrics": [
        {
          "duration": "16h 42m",
          "durationInSeconds": "60120",
          "startTime": "11/19/2016 22:27:00 (UTC)",
          "startTimeEpoch": "1479594420",
          "imageUrl": "https://alert-images-staging.s3.amazonaws.com/i:04410c3e-9364-46d2-9db5-c71777429254e:7d70fa50f1f0060f2e1176c5d207eb1b.png",
          "peak": "1000.0000",
          "direction": "UP",
          "name": "what=total_sales.country=us.device=mobile.source=db.state=ca.1448141087178",
          "state": "CLOSED",
          "threshold": 500
        }
      ],
      "events": {
        "total": "2",
        "buckets": [
          {
            "date": "1472626800",
            "total": "1",
            "topEvents": [
              {
                "title": "Sample event 1",
                "description": "Event 1",
                "source": "jenkins",
                "category": "deployments",
                "startDate": "1472626597",
                "endDate": "1472626597"
              }
            ]
          },
          {
            "date": "1472626200",
            "total": "1",
            "topEvents": [
              {
                "title": "Sample Event 2",
                "description": "Event 2",
                "source": "jenkins",
                "category": "deployments",
                "startDate": "1472625982",
                "endDate": "1472625982"
              }
            ]
          }
        ]
      },
      "alertId": "fcc7a04e-b9ac-413f-841d-8c69ba24b384",
      "alertSettingsUrl": "https://yourdomain.anodot.com/#!/alert/fcc7a04e-b9ac-413f-841d-8c69ba24b384",      
      "description": "Sample Static Alert",
      "severity": "critical"
    }
  ]
} 
```

## No Data Alert 

A No Data alert trigger is fired when a specified metric ceases to send data points. For details on No Data alerts please see [here](https://support.anodot.com/hc/en-us/articles/360015467439-Creating-Static-and-No-Data-Alerts).

> No Data Alert Template: 

```json
{
"subject": "{{subject}}",
"severity": "{{severity}}",
"description": "{{description}}",
"startTime": "{{startTime}} (UTC)",
"startTimeEpoch": "{{startTimeEpoch}}",
"alertGroupId": "{{alertGroupId}}",
"alertGroupStatus": "{{alertgGroupStatus}}",
"type": "{{type}}",
"alerts": [
   {
    "title": "{{title}}",
    "alertStatus": "{{alertStatus}}",
    "metrics": [
    {
        "duration": "{{duration}}",
        "durationInSeconds": "{{durationInSeconds}}",
        "startTime": "{{startTime}} (UTC)",
        "startTimeEpoch": "{{startTimeEpoch}}",
        "imageUrl": "{{imageUrl}}",
        "peak": "{{peak}}",
        "direction": "{{direction}}",
        "name": "{{name}}",
        "state": "{{state}}",
        "threshold": "{{threshold}}"
        }
    ],
   }
],
"events": { 
  "total":"{{total Events}}",
  "buckets": [
  {
    "date":"{{date}}",
    "Total":"{{total events in aggregation}}",
    "topEvents": [
      {
        "title" :"{{title}}",
        "description" :"{{description}}",
        "source" :"{{source}}",
        "category" : "{{category}}",
        "startDate" :"{{startDate}}",
        "endDate" : "{{endDate}}"
      }
    ]
  },
  ]
},
"alertId": "{{alertId}}",
"alertSettingsUrl": "{{Link to the alerts setting}}", 
"description" :"{{description}}",
"severity":"{{severity}}"
}
]
}
```

> No Data Alert Response Example (With Multiple Metrics):

```json
{
"subject": "Alert Open: No Data Reported Sample No Data Alert 1479763791301[critical][576ff]",
"severity": "critical",
"description": "should generate one alert on no data",
"startTime": "11/21/2016 21:32:12 (UTC)",
"startTimeEpoch": "1479763932",
"alertGroupId": "2aefdac1-1a07-4b7b-bf84-09bf5b34f917",
"alertGroupStatus": "open",
"type": "No Data",
"alerts": [
    {
    "title": "Sample No Data Alert 1479763791301",
    "alertStatus": "open",
    "metrics": [   
        {
        "lastSeen":"11/21/2016 20:50:12",
        "lastSeenEpoch":"1479761412",
        "duration": "2m",
        "durationInSeconds": "120",
        "startTime": "11/21/2016 21:32:12 (UTC)",
        "startTimeEpoch": "1479763932",
        "name": "what=total_sales.country=us.device=mobile.source=db.state=ca.1479763791301",
        "state": "OPEN"
        } 
    ],
    "alertId": "576ff33f-3a7a-4396-888f-16edc20b98ad",
    "alertSettingsUrl": "https://app.staging.anodot.com/#!/alert/576ff33f-3a7a-4396-888f-16edc20b98ad",    "description": "should generate one alert on no data",
    "severity": "critical"
    }
]
} 
```

