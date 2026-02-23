---
layout: blog
author: "Scott Motte"
title: "run your own vestauth infrastructure"
categories:
  - blog
image: "/assets/img/blog/blog-3.png"
excerpt: "vestauth is just HTTP. In this guide, we'll build a minimal registrar, verification service, and .well-known directory so you can run your own vestauth infrastructure locally."
---

Vestauth doesn't require special infrastructure. At its core, it's just HTTP: an endpoint to register agents, a place to publish public keys, and a way to verify signed requests.

To make that concrete, we'll build a minimal Vestauth service from scratch. We'll start with a small express server and add capabilities one piece at a time. By the end, you'll have a working inhouse vestauth infrastructure.

## What Vestauth does

Vestauth is an identity authority. It:

* Registers agents
* Stores their public keys
* Serves keys via .well-known
* Verifies signed requests

![](https://github.com/user-attachments/assets/859105b4-0f92-46d3-8a0d-4ea61e201b9f)

> Lifecycle at a glance:
> 1. Agent generates a keypair locally.
> 2. Agent signs `POST /register`.
> 3. Server verifies the signature and stores the agent's public key.
> 4. Later requests are verified by looking up keys via the agent's `.well-known` endpoint.

## What you're building

In practice, you're building an agent identity authority: a service that registers agent identities, proves key ownership at registration time, and publishes public keys for discovery.

`POST /register` verifies proof-of-possession, meaning the agent can sign with the private key that matches the public key it is asking you to store.

`/.well-known/http-message-signatures-directory` is the discovery endpoint other systems use to fetch that agent's public keys so signed requests can be verified.

## Build it

### Create server.js

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

Install express.

```sh
$ npm install express --save
```

Start the server.

```sh
$ node server.js
Server is running on http://localhost:3000
```

Visit [localhost:3000](http://localhost:3000) and you should see:

```sh
{"hello":"vestauth-inhouse"}
```

From here, we'll begin adding the pieces Vestauth needs — starting with request verification at registration.

### POST /register

Next, add a **/register** endpoint that verifies the incoming signed request.

```js
const vestauth = require('vestauth')

...

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

Install the new dependencies.

```sh
$ npm install vestauth --save
```

Restart your server so the new dependency is loaded.

```sh
$ node server.js
Server is running on http://localhost:3000
```

Now test making a POST to /register.

```sh
$ curl -X POST http://localhost:3000/register -d '{}' -H "Content-Type: application/json"
```

You should receive a `MISSING_SIGNATURE_INPUT` error.

```sh
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>Error: [MISSING_SIGNATURE_INPUT] missing --signature-input<br> &nbsp; &nbsp;at Errors.missingSignatureInput...</pre>
</body>
</html>
```

Let's catch the error and return a `401` status code.

```js
app.post('/register', async (req, res) => {
  try {
    const url = `${req.protocol}://${req.get('host')}${req.originalUrl}`

    const verified = await vestauth.primitives.verify(
      req.method,
      url,
      req.headers,
      req.body.public_jwk
    )

    res.json(verified)
  } catch (err) {
    res.status(401).json({ error: { status: 401, code: 401, message: err.message }})
  }
})
```

Next, we're ready to initialize our agent.

### Initialize agent

Create a folder to place your agent in.

```sh
$ mkdir your-agent
$ cd your-agent
```

And then initialize your agent. This will call your `/register` endpoint.

```sh
$ vestauth agent init --hostname http://localhost:3000
```

### Add datastore

We'll add a simple in-memory datastore, but you can replace this with a robust datastore like postgres.

> Warning: This example uses in-memory storage. Restarting the server will lose all agents and keys. In production, use a persistent datastore.

The agent signs the registration request with its private key. This verifies proof-of-possession of the public key being registered.

```js
...
const crypto = require('crypto')

const AGENTS = []
const PUBLIC_JWKS = []

app.post('/register', async (req, res) => {
  try {
    const url = `${req.protocol}://${req.get('host')}${req.originalUrl}`
    const verified = await vestauth.primitives.verify(req.method, url, req.headers, req.body.public_jwk)

    // insert agent
    const uid = `agent-${crypto.randomBytes(12).toString('hex')}`
    const agent = { uid }
    AGENTS.push(agent)

    // insert public_jwk
    const publicJwk = {
      agent_uid: agent.uid,
      kid: verified.kid,
      value: verified.public_jwk
    }
    PUBLIC_JWKS.push(publicJwk)

    // response must be this format
    json = {
      uid: agent.uid,
      kid: publicJwk.kid,
      public_jwk: verified.public_jwk,
      is_new: true
    }

    res.json(json)
  } catch (err) {
    res.status(401).json({ error: { status: 401, code: 401, message: err.message }})
  }
})
```

Great! We can now track registered agents and their public json web keys. (We'll leave it an exercise for the reader to handle upserts and relationships correctly).

> Note: This example always creates a new agent. Production systems should handle upserts, multiple keys per agent, key rotation, and revocation.

### Add `.well-known` endpoint

Next we need to add the endpoint for public key discovery. This is according to the emerging [web-bot-auth](https://datatracker.ietf.org/doc/html/draft-meunier-web-bot-auth-architecture) standard.

```js
...
app.get('/.well-known/http-message-signatures-directory', (req, res) => {
  const keys = PUBLIC_JWKS
    .filter(jwk => jwk.agent_uid === req.agentUid)
    .map(jwk => jwk.value)

  res.json({ keys })
})
```

### Add wildcard support

Next, add support for wildcard subdomains. This is according to the [spec](https://datatracker.ietf.org/doc/html/draft-meunier-web-bot-auth-architecture).

Add it to your `app.use()` middleware.

```js
...
const app = express()
app.use(express.json())
app.use((req, res, next) => {
  const hostNoPort = (req.headers.host || '').split(':')[0].toLowerCase()

  // agent-c235... .localhost
  if (hostNoPort.endsWith('.localhost')) {
    const sub = hostNoPort.slice(0, -'.localhost'.length) // "agent-c235..."
    req.agentUid = sub
    return next()
  }

  next()
})
...
```

### Add `/whoami` endpoint

Add a whoami endpoint to surface the well known endpoint.

```js
...
app.get('/whoami', async (req, res) => {
  try {
    const url = `${req.protocol}://${req.get('host')}${req.originalUrl}`
    const verified = await vestauth.tool.verify(req.method, url, req.headers)

    res.json(verified)
  } catch(err) {
    console.log(err)
    res.status(401).json({ error: { status: 401, code: 401, message: err.message }})
  }
})
```

### Check `/whoami`

Restart your server.

```sh
$ node server.js
```

Init your agent.

```sh
$ vestauth agent init --hostname http://localhost:3000
```

And check who you are.

```sh
$ vestauth agent curl http://localhost:3000/whoami --pp
{
  "uid": "agent-92d4b4502cf024b987cc5bc0",
  "kid": "QPECTgNUTajbcflmFwxsF8ZLuzl_iUypYzCzJwMWl5w",
  "public_jwk": {
    "crv": "Ed25519",
    "x": "ebdK8WgbLJ8Z1F-ybGCY76J0H8PLYRoS0R1h2XR25iM",
    "kty": "OKP",
    "kid": "QPECTgNUTajbcflmFwxsF8ZLuzl_iUypYzCzJwMWl5w"
  },
  "well_known_url": "http://agent-92d4b4502cf024b987cc5bc0.localhost:3000/.well-known/http-message-signatures-directory"
}
```

Visit the .well-know FQDN for your agent:

```
http://agent-92d4b4502cf024b987cc5bc0.localhost:3000/.well-known/http-message-signatures-directory
```

Confirm the public key is being served.

```json
{
  "keys": [
    {
      "crv": "Ed25519",
      "x": "ebdK8WgbLJ8Z1F-ybGCY76J0H8PLYRoS0R1h2XR25iM",
      "kty": "OKP",
      "kid": "QPECTgNUTajbcflmFwxsF8ZLuzl_iUypYzCzJwMWl5w"
    }
  ]
}
```

## What's Next

From here, the next step is production hardening:

* Swapping in your own database
* Handling upserts and duplicate registrations
* Supporting multiple keys, rotation, and revocation

## Conclusion

Great! You now run your own Vestauth infrastructure.

You control agent identity, key ownership, discovery, and verification — fully inhouse.

That means your systems no longer rely on API keys floating around in config files. Agents can prove who they are cryptographically, and other systems can verify them using standard HTTP.

The core is already there: proof-of-possession at registration, public key discovery via `.well-known`, and request verification on top.

This is the foundation for agents to authenticate with proof of possession instead of shared secrets.

You now have the core of a standards-based agent identity layer you can run, audit, and extend inhouse.
