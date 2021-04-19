---
date: 2021-04-18T20:44:40-07:00
title: Accessing-Postgres-From-Cloudflare-Workers
tags: Postgres,Cloudflare,Workers
---

# Accessing-Postgres-From-Cloudflare-Workers

Cloudflare has [workers](https://workers.cloudflare.com/) - a serverless platform that runs your code across its edge location. One thing, it doesn't natively talk tcp yet which means accessing Postgres natively from a worker is not possible. However, it knows `http` (very well :) ). 
Recently, I saw [PostgREST](https://postgrest.org/en/stable/) project - Serve a RESTful API from a Postgres database. I wanted to try that and here I am.

Using the [PostgREST](https://postgrest.org/en/stable/tutorials/tut0.html), I was able to get it running locally.

Next step is connecting from Workers to PostgREST instance on my server. To do that securely, I turned to Cloudflare Tunnel. It provides a hostname which I can connect to and protects the server too. No need for VPN, or VPC etc.

![Screen Shot 2021-04-18 at 9.18.46 PM](/Users/visi/Library/Application Support/typora-user-images/Screen Shot 2021-04-18 at 9.18.46 PM.png)

A simple Workers script in Javascript, listens for "fetch" event. Hostname `https://cfworkerspostgresdemo.viggy28.dev/`is exposed through a Cloudflare tunnel. There is a PostgREST endpoint called `/todos`. 

**Workers script**

```javascript
addEventListener("fetch", event => {
  return event.respondWith(handleRequest())
})

async function handleRequest() {
  hostname = "https://cfworkerspostgresdemo.viggy28.dev/"
  const init = {
    headers: {
      "content-type": "text/html;charset=UTF-8",
    }
  }
  const response = await fetch("https://cfworkerspostgresdemo.viggy28.dev/todos", init)
  const results = await gatherResponse(response)
  return new Response(results, init)
}

/**
 * gatherResponse awaits and returns a response body as a string.
 * Use await gatherResponse(..) in an async function to get the response body
 * @param {Response} response
 */
async function gatherResponse(response) {
  const { headers } = response
  const contentType = headers.get("content-type") || ""
  if (contentType.includes("application/json")) {
    return JSON.stringify(await response.json())
  }
  else if (contentType.includes("application/text")) {
    return await response.text()
  }
  else if (contentType.includes("text/html")) {
    return await response.text()
  }
  else {
    return await response.text()
  }
}
```

Later, I realized I can also access the endpoint even without the worker also by visiting the Tunnel hostname `https://cfworkerspostgresdemo.viggy28.dev/`

**Cloudflare Tunnel Config**

```
tunnel: <replacemewithUUID>
credentials-file: <replacemewithUUID.json>

ingress:
  - hostname: cfworkerspostgresdemo.viggy28.dev
    service: http://localhost:3000
    originRequest:
      connectTimeout: 5s
      proxyType: socks
  - service: http_status:404
  # Catch-all rule, which responds with 404 if traffic doesn't match any of
  # the earlier rules
```

