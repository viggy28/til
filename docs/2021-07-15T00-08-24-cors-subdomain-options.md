---
date: 2021-07-15T00:08:24-07:00
title: Cors-Subdomain-Options
tags: cors
---

# Cors-Subdomain-Options
I came to know that even if the API is in the sub-domain of the host that's trying to reach you still need to enable CORS on the backend

https://stackoverflow.com/questions/26724137/do-i-need-to-enable-cors-when-my-api-is-on-a-subdomain-of-my-main-website/26724206

CORS uses preflight request which uses OPTIONS method. That's something you need to enable on the backend too.

https://flaviocopes.com/golang-enable-cors/

You can easily check whether the backend accepts OPTIONS request or not through `curl` 

```
curl -X OPTIONS [http://localhost:3001/createservice](http://localhost:3001/createservice) -i
```