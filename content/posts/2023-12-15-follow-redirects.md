---
title: "tracing redirects"
date: 2023-12-15T00:00:00-05:00
draft: false
categories: [ daily ]
tags : [ blog ]
---

# Follow the redirect road
Sometimes you want to have a curl command that traces the redirects and lets you see hops that it makes along the way  
long story short   do this  
```
curl -sLD - http://test.chrislatta.org/myredirect.html -o /dev/null 
```
it will look like this  
```
$ curl -sLD - http://test.chrislatta.org/myredirect.html -o /dev/null 
HTTP/1.1 301 Moved Permanently
Content-Type: text/html; charset=iso-8859-1
Content-Length: 244
Connection: keep-alive
Location: http://test.chrislatta.org/hop1.html

HTTP/1.1 301 Moved Permanently
Content-Type: text/html; charset=iso-8859-1
Content-Length: 244
Connection: keep-alive
Location: http://test.chrislatta.org/hop2.html

HTTP/1.1 301 Moved Permanently
Content-Type: text/html; charset=iso-8859-1
Content-Length: 244
Connection: keep-alive
Location: http://test.chrislatta.org/lost.html

HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 287
Connection: keep-alive
Host-Header: 192fc2e7e50945beb8231a492d6a8024
Accept-Ranges: bytes
This output shows that that there are three redirects, or "hops", occurring:

http://test.chrislatta.org/hop1.html
http://test.chrislatta.org/hop2.html
http://test.chrislatta.org/lost.html
```


