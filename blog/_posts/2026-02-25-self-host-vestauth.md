---
layout: blog
author: "Scott Motte"
title: "self-host vestauth"
categories:
  - blog
image: "https://github.com/user-attachments/assets/b05ba917-c37a-4a53-9ec7-c5c8d78ad1c7"
excerpt: "Run your own Vestauth server step by step and keep agent identity infrastructure under your control."
---

Vestauth does not require a central auth provider.

At its core, Vestauth is just HTTP plus public key cryptography:

* agents generate keys locally
* agents sign requests
* tools verify signatures
* public keys are published via `.well-known`

That means you can run your own Vestauth infrastructure when you want tighter control over discovery, storage, hosting, and operations.

This guide walks through self-hosting the built-in Vestauth server step by step.

## Why self-host Vestauth?

Vestauth works great with the default hosted infrastructure, but there are good reasons to run your own:

* **Control your infrastructure**: keep registration, discovery, and key serving under your domain.
* **Compliance / data residency**: run on your own cloud, region, or network boundaries.
* **Custom operations**: use your own Postgres, observability, backups, and deployment pipeline.
* **Private environments**: support internal agents and internal tools.
* **Future flexibility**: customize trust rules, federation, and deployment topology as your agent ecosystem grows.

Like I wrote in the "run your own vestauth infrastructure" post: Vestauth is just HTTP. You're running an agent identity service that registers agents, stores public keys, and serves them for verification.

## What you are setting up

When you self-host the Vestauth server, you are running the infrastructure that:

* registers agent identities
* stores agent public keys
* serves `/.well-known/http-message-signatures-directory`
* supports tool-side verification workflows

In other words, you control the agent identity authority for your environment.

## Prerequisites

Before starting, make sure you have:

* `curl`
* `postgres` (local or managed)
* a server/VM/container to run the Vestauth service (for production)
* DNS control of the hostname you plan to use (for production)

## Step 1: Install Vestauth CLI

Install the CLI (same binary includes server commands).

```sh
$ curl -sSf https://vestauth.sh | sh
```

You can also install via npm or GitHub releases, but [`vestauth.sh`](https://vestauth.sh) is the fastest path for a fresh machine.

## Step 2: Initialize the server

Initialize a Vestauth server project.

```sh
$ vestauth server init
```

This creates the local server files and an `.env` configuration you can edit.

## Step 3: Create the database

Create the Postgres database used by the server.

```sh
$ vestauth server db:create
Created database 'vestauth_production'
```

By default this targets `vestauth_production` on your local Postgres unless you set `DATABASE_URL`.

## Step 4: Run migrations

Run the database migrations.

```sh
$ vestauth server db:migrate
```

This creates the tables Vestauth needs for agents and public keys.

## Step 5: Configure the server (`.env`)

Edit `.env` and set your runtime config.

```ini
PORT="3000"
HOSTNAME="http://localhost:3000"
DATABASE_URL="postgres://localhost/vestauth_production"
```

### Local development config

For local development, this is usually enough:

* `HOSTNAME="http://localhost:3000"`
* `DATABASE_URL` pointing to your local Postgres

### Production config

For production, update:

* `HOSTNAME` to your public hostname (for example `https://vestauth.yoursite.com`)
* `DATABASE_URL` to your managed Postgres connection string

Example:

```ini
PORT="3000"
HOSTNAME="https://vestauth.yoursite.com"
DATABASE_URL="postgresql://USER:PASS@aws-1-us-east-1.pooler.supabase.com:5432/postgres"
```

## Step 6: Start the server

Start your self-hosted Vestauth server.

```sh
$ vestauth server start
vestauth server listening on http://localhost:3000
```

You can also override values at runtime:

```sh
$ vestauth server start --port 4567
$ vestauth server start --hostname vestauth.yoursite.com
$ vestauth server start --database-url postgresql://...
```

## Step 7: Create an agent against your server

Now create an agent and point it at your self-hosted hostname.

```sh
$ mkdir your-agent
$ cd your-agent
```

```sh
$ vestauth agent init --hostname http://localhost:3000
✔ agent created (.env/AGENT_UID=agent-4b94ccd425e939fac5016b6b)
```

This does a few important things:

* generates an Ed25519 keypair locally
* stores `AGENT_PRIVATE_JWK` and `AGENT_PUBLIC_JWK` in `.env`
* registers the agent with your Vestauth server
* returns an `AGENT_UID` used to build the agent discovery FQDN

## Step 8: Verify it works

Use your new agent to sign a request against your self-hosted server and confirm the identity path works end to end.

```sh
$ vestauth agent curl http://localhost:3000/whoami --pp
```

Or hit one of your own tools that calls `vestauth.tool.verify(...)`.

The key point is this: the request is signed locally by the agent, and tools verify it using public keys discovered from your self-hosted infrastructure.

## What changes when you self-host?

The cryptography and standards stay the same.

What changes is the infrastructure under the hood:

* **discovery domain** is yours
* **database** is yours
* **deployment + uptime** are yours
* **network boundaries** are yours
* **operational controls** are yours

You still get the same Vestauth developer experience:

* `vestauth agent init`
* `vestauth agent curl`
* `vestauth tool.verify(...)`

But the trust and hosting boundary is now under your control.

## Production notes (important)

### 1) Configure wildcard DNS

You must configure a wildcard DNS record for `*.${HOSTNAME}`.

Example:

* If `HOSTNAME=vestauth.yourapp.com`
* Add wildcard DNS for `*.vestauth.yourapp.com`

This is required for `.well-known` discovery in the web-bot-auth architecture because agents resolve to subdomains under your host.

### 2) Use HTTPS in production

Use HTTPS for your public Vestauth hostname in production. Local `http://localhost` is fine for development.

### 3) Run managed Postgres (recommended)

Use a managed Postgres provider (Supabase, RDS, etc.) for backups, failover, and operations unless you explicitly want to operate Postgres yourself.

### 4) Add normal service hardening

Treat the Vestauth server like any production HTTP service:

* logging and metrics
* backups
* timeouts
* health checks
* deploy rollbacks

## Who should self-host?

Self-hosting is a strong fit if you are:

* building an internal agent platform
* shipping a product that needs your own trust domain
* operating in regulated or enterprise environments
* standardizing agent identity across multiple services

If you are just exploring Vestauth, start with the hosted defaults first. You can migrate to self-hosting later without changing the underlying HTTP Message Signatures model.

## Closing

Vestauth is designed so you can start simple and grow into your own infrastructure.

If you self-host, you are not opting out of the standard. You are adopting it more fully by running the identity, key discovery, and trust boundary yourself.

That gives you a cryptographic agent auth layer you can run, audit, and operate on your own terms.
