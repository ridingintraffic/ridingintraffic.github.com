---
date: 2023-01-31T00:00:00-05:00
draft: false
title: 'fun with cloudwatch logs'
categories: [ daily ]
tags : [ life ]
---

# writing directly to cloudwatch logs
sometimes you just need to write to a cloudwatch log  and you want to do that from the terminal?
You can do this with jq bash and aws cli   yey
```
timestamp=$(date +%s000); \
json_data=$(jq --null-input --arg timestamp $timestamp '{"timestamp": ($timestamp|tonumber), "message": "ssn: 123-12-1234"}'); \
aws logs put-log-events --log-group-name /dev/ridigintraffic --log-stream-name something-special --log-events "$json_data"
```