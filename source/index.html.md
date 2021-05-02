---
title: Anodot API Reference 

#language_tabs: # must be one of https://git.io/vQNgJ
#  - json: JSON
#  - shell: cURL
  
toc_footers:
  - <a href='https://www.anodot.com'>Anodot Business Monitoring</a>
  - <a href='https://support.anodot.com/hc/en-us'>Anodot Help Center</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>
  - <a href='mailto:support@anodot.com'>Contact us</a>

includes:
  # - Resources
  - Authentication
  - Post Metrics
  - Tags
  - Alert configuration
  - Alert Triggers
  - Anomaly
  - Feedback
  - Streams
  - Lookup Tables
  - Dynamic Routing Tables
  - Channels
  - User Events
  - Users
  - Webhook
  - Schema
  - Metrics Operations
 # - Forecast
  - Need to know
  - Limits
  - Response Codes
  - Anodot Error Codes
  
search: true
---

# API Reference

> **API Base URLs**

> * Asia Pacific customers, Use [https://ap.anodot.com](https://ap.anodot.com)<br/>
> * India customers, Use [https://in.anodot.com](https://in.anodot.com)<br/>
> * US customers, Use [https://api.anodot.com](https://api.anodot.com)<br/>
> * Personalized URL scheme? Use [https://my-org.anodot.com](https://my-org.anodot.com)

Welcome to the Anodot API Reference Site.
Use the Anodot APIs to access Anodot Objects.

The Anodot API endpoints can be used to:

* Post metrics to Anodot using Metric 2.0 and Metric 3.0 protocols.
* Post Events and Tags to Anodot.
* Get information on metrics, alerts, feedback, anomalies and other entities

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/10969159-293c8cc8-9926-4189-8f7b-accfe91bf88c?action=collection%2Ffork&collection-url=entityId%3D10969159-293c8cc8-9926-4189-8f7b-accfe91bf88c%26entityType%3Dcollection#?env%5BAnodot%5D=W3sia2V5IjoibWV0cmljX3ZhbHVlIiwidmFsdWUiOiIxIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJlcG9jaCIsInZhbHVlIjoiMTYwNjMwNTc0OCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiY2FtcGFpZ25JRCIsInZhbHVlIjoiMSIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoibWV0cmljX2NvdW50ZXIiLCJ2YWx1ZSI6IiIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYWNjZXNzLXRva2VuIiwidmFsdWUiOiJBY2Nlc3MgVG9rZW4gdXNlZCBmb3IgdGhlIEF1dGhlbnRpY2F0ZSBDYWxsXG4iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImFwaS10b2tlbiIsInZhbHVlIjoiVG9rZW4gR2VuZXJhdGVkIGJ5IEF1dGhlbnRpY2F0aW9uIEFQSVxuIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJkYXRhLXRva2VuIiwidmFsdWUiOiJHZXQgeW91ciB0b2tlbiEgIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJhcHAtdXJsIiwidmFsdWUiOiJBbm9kb3QgQXBwbGljYXRpb24gVVJMIiwiZW5hYmxlZCI6dHJ1ZX1d)

<aside class="notice">
We keep updating this site with new endpoints and APIs.<br/>
Need help? Reach out at <code>support@anodot.com</code> and our support team will be happy to assist you.
</aside>
