---
layout: blog
author: "Scott Motte"
title: "why we created vestauth"
categories:
  - blog
video: "https://github.com/user-attachments/assets/b1cf9fe2-b9f7-471d-a210-65779287bb40"
excerpt: "a conversation about why we built vestauth for autonomous agents and providers."
---

**scott:** this is scott. i created `dotenv` and `dotenvx`, with a combined 80 million weekly installs and 25,000 github stars.

**laura:** and i am laura. i have led teams through 50+ private equity technology diligence efforts.

**scott:** about a month ago, i started building a new way for agents to store and rotate secrets as part of `dotenvx`, and i ran into a core problem: agents cannot sign themselves up autonomously. and they need to do that without a human in the loop. captchas, usernames, and passwords were built for humans, not agents.

**laura:** over the last month we got obsessed with this. for agents, the problem is they are bypassing controls. for providers, the problem is complexity and duplicated effort across mcp integrations, oauth flows, api keys, and key rotation.

**scott:** there is a better way. it's called [`vestauth`](https://github.com/vestauth/vestauth). it handles both sides. on the agent side, one command sets up a cryptographic identity, without human-first handshake patterns like oauth. on the provider side, there is no api key management, no username/password auth, and no user table just to identify agents. verification is cryptographic and can be added with a single line of code.

**laura:** we have been building for four weeks. the product is live. we started distribution through [`dotenv`](https://github.com/motdotla/dotenv) and [`dotenvx`](https://github.com/dotenvx/dotenvx). it is still early, but agent autonomy is accelerating every day.

**scott:** authentication needs to catch up. that is why we created `vestauth`.
