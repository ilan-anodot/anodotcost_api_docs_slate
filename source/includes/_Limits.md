# Rates and Limits

<aside class = "success">
The default rate limit for Anodot end-points is **500 calls per minute**.
Please adhere to the rate limitations to prevent data loss and error handling.
</aside>

Specific end-points have different limits, please see the table below:</br>

End Point | Rate Limit (RPM)
----------|-----------
Alert configuration | 500 RPM for all *alerts* API calls
Alert triggers | 500 RPM for all *triggers* API calls
Anomaly | 1000 RPM for all *anomaly* API calls
Feedback | GET - 50 RPM
Stream | POST/PUT - 1 RPM</br>GET - 10 RPM</br>DELETE - 15 RPM
Cardinality | 50 RPM for all *cardinality* API calls


The overall number of entities is limited according to the terms of use or your specific contract.