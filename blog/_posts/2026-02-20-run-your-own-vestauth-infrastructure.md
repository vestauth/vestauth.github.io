---
layout: blog
author: "Scott Motte"
title: "run your own vestauth infrastructure"
categories:
  - blog
excerpt: "vestauth is just HTTP. In this guide, we'll build a minimal registrar, verification service, and .well-known directory so you can run your own vestauth infrastructure locally."
---

Vestauth doesn't require special infrastructure. At its core, it's just HTTP: an endpoint to register agents, a place to publish public keys, and a way to verify signed requests.

To make that concrete, we'll build a minimal Vestauth service from scratch. We'll start with a small express server and add capabilities one piece at a time. By the end, you'll have a working inhouse vestauth infrastructure.

## Basic server

First, let's create a basic server and make sure it responds.

```js
// server.js
const express = require('express')

const app = express()
app.use(express.json())

app.get('/', (req, res) => {
  res.json({ hello: 'vestauth-inhouse' })
})

app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000')
})
```

Install express:

```sh
npm install express --save
```

Start the server:

```sh
node server.js
```

And visit:

```sh
http://localhost:3000
```

You should see:

```sh
{"hello":"vestauth-inhouse"}
```

From here, we'll begin adding the pieces Vestauth needs — starting with request verification at registration.

## Register endpoint (verification only)

Next, add a `/register` endpoint that verifies the incoming signed request.

```js
app.post('/register', async (req, res) => {
  const url = `${req.protocol}://${req.get('host')}${req.originalUrl}`

  const verified = await vestauth.primitives.verify(
    req.method,
    url,
    req.headers,
    req.body.public_jwk
  )

  res.json(verified)
})
```

Install the new dependencies:

```sh
npm install vestauth --save
```

Restart your server so the new dependency is loaded:

```sh
node server.js
```

Now test registration from the CLI.

```sh
vestauth agent init --hostname http://localhost:3000
```

If verification succeeds, the endpoint returns the verified registration payload as JSON.

In the next section, we'll add `try/catch` error handling, then persist the agent and its `public_jwk`.
