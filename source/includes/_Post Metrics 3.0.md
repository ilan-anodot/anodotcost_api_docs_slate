# Post Metrics 3.0

Send data samples to Anodot using Metric 3.0 protocol.
Anodot metric 3.0 protocol works with a structured schema. The protocol expects key-value pairs of properties instead of one single-name string.
This allows Anodot to understand the metric dimensions and the measured KPI (the *measure* property).
It is possible to post timestamp watermarks to indicate flushing directives to Anodot.

The protocol is based on **schemas** which define

* The required **measures** and their attributes: *Units*, *Aggregation*, *Counting method*
* The required **dimensions** and attributes: *Name*, *Fill policy*

**Process Steps**

1. [Creating a Schema](#schema)
2. Sending samples matching the schema
3. Sending watermarks timestamp to direct Anodot to process data samples up to the timestamp

## Send Data Samples

## Send Stream Watermark