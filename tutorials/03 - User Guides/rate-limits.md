---
title: Request rate limits
---

# Request rate limits

Requests sent to Microsoft DeepDev API are rate limited based on several time frames.

During private preview, the rate limits are as follows:

| Time frame | Maximum number of requests |
| ---------- | -------------------------- |
| 1 second   | 2                          |
| 1 minute   | 100                        |
| 10 minutes | 500                        |

## Response

In case you exceed your allowed rate limit, any further requests for the time period will fail with the status code 429.
