---
layout: blog
author: "Scott Motte"
title: "introducing vestauth"
categories:
  - blog
image: "/assets/img/blog/blog-1.png"
excerpt: "Agent authentication with verifiable identity and request signatures."
---

Today we are launching **vestauth**, authentication built for agents.

`vestauth` gives agents a cryptographic identity and lets tools verify signed requests with confidence. It is designed to be simple to adopt and strong by default.

## Why vestauth

The shift to agents introduces a new baseline requirement: tools need to know which agent made a request and whether that request was tampered with in transit.

`vestauth` addresses this with:

- agent identities backed by cryptographic keys
- signed requests agents can generate from the CLI
- verification primitives tools can use in their servers

## Install

```sh
curl -sSf https://vestauth.sh | sh
```

## Quick look

Initialize an agent identity:

```sh
vestauth agent init
```

Sign and send a request:

```sh
vestauth agent curl https://ping.vestauth.com/ping
```

Verify on the tool side:

```js
const agent = await vestauth.tool.verify(req.method, url, req.headers)
```

## What is next

Next posts will cover the protocol, threat model, and implementation details, along with practical integration guides.
