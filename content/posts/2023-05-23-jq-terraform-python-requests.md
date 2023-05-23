---
date: 2023-05-23T00:00:00-05:00
title: 'python base64 requests jq and terraform'
draft: false
categories: [ daily ]
tags : [ python, jq]
---

## base64 user auth in requests
use the base64 for the first header and then reuse the bearer token that you get back.
``` 
    headers = {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
    }
    
    base_url = 'https://www.something.com'
    token_url = "/tauth/1.0/token/"
    url=base_url + token_url
    response = requests.post(url, headers=headers, auth=HTTPBasicAuth(USER, PASSWORD))
    data = response.json()
    
    #reusing that bearer token from the repsonse
    headers = {
        'Authorization': f"Bearer {data['token']}",
        'Content-Type': 'application/json',
        'Accept': 'application/json'
    }
    url="/something/"
    response=requests.get( url, headers=headers)
    data = response.json()

```

## using jq with terraform
taking a terraform plan and outputting only the things that will change on the plan.   such as only the resource names.  
this is useful when you need to do some targeted deploys.  
```
terraform plan -input=false -compact-warnings -out plan.tfplan;  
terraform show -no-color -json plan.tfplan | jq -r '.resource_changes[] | select(.change.actions[0]=="update" or .change.actions[0]=="create" or .change.actions[0]=="create") | .address'
```