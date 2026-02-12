---
title: ""
---

<section class="min-h-screen px-5 py-8 sm:px-12 sm:py-10">
  <div class="max-w-5xl font-mono text-xs leading-5 text-black dark:text-white sm:text-sm sm:leading-6">
    <p class="break-words [overflow-wrap:anywhere]"><span class="text-[#1a7f52] dark:text-[#30ff8a]">$</span> vestauth agent init</p>

    <section class="mt-6 sm:mt-8">
      <h2 class="text-[#1a7f52] dark:text-[#30ff8a]">Description</h2>
      <p class="mt-3 max-w-5xl italic">auth for agents&ndash;from the creator of <a class="underline" href="https://github.com/motdotla/dotenv#readme" target="_blank" rel="noopener noreferrer"><code>dotenv</code></a> and <a class="underline" href="https://github.com/dotenvx/dotenvx#readme" target="_blank" rel="noopener noreferrer"><code>dotenvx</code></a>.</p>
    </section>

    <section class="mt-4 sm:mt-6">
      <h2 class="text-[#1a7f52] dark:text-[#30ff8a]">Install</h2>
      <p class="mt-2 break-words [overflow-wrap:anywhere]"><span class="text-[#1a7f52] dark:text-[#30ff8a]">$</span> curl -sSf https://vestauth.sh | sh</p>
    </section>

    <section class="mt-4 sm:mt-6">
      <h2 class="text-[#1a7f52] dark:text-[#30ff8a]">Usage</h2>

      <p class="mt-2 text-zinc-500 dark:text-zinc-500"># Give agents cryptographic identities…</p>
      <p class="break-words [overflow-wrap:anywhere]"><span class="text-[#1a7f52] dark:text-[#30ff8a]">$</span> vestauth agent init</p>

      <p class="mt-4 text-zinc-500 dark:text-zinc-500"># …and sign their curl requests with cryptographic authentication.</p>
      <p class="break-words [overflow-wrap:anywhere]"><span class="text-[#1a7f52] dark:text-[#30ff8a]">$</span> vestauth agent curl https://api.vestauth.com/whoami</p>
    </section>

    <section class="mt-4 sm:mt-6">
      <h2 class="text-[#1a7f52] dark:text-[#30ff8a]">Provider Usage</h2>
      <p class="mt-2 text-zinc-500 dark:text-zinc-500"># Verify requests and safely trust agent identity using cryptographic proof.</p>
      <pre class="mt-2 max-w-2xl overflow-x-scroll rounded border border-[#d9d9d9] bg-[#f7f7f7] p-3 text-[10px] leading-4 text-black dark:border-[#2f4f40] dark:bg-[#0d2219] dark:text-white sm:text-xs"><code>// index.js
const express = require('express')
const vestauth = require('vestauth')
const app = express()

// vestauth agent curl http://localhost:3000/whoami
app.get('/whoami', async (req, res) =&gt; {
  try {
    const url = `${req.protocol}://${req.get('host')}${req.originalUrl}`
    const agent = await vestauth.provider.verify(req.method, url, req.headers)
    res.json(agent)
  } catch (err) {
    res.status(401).json({ code: 401, error: { message: err.message }})
  }
})

app.listen(3000, () =&gt; { console.log('listening on http://localhost:3000') })</code></pre>
    </section>

  </div>

  <div class="mt-10 flex items-center gap-4 text-base font-semibold tracking-tight text-black dark:text-white sm:mt-14 sm:text-lg">
    <span>vestauth</span>
    <a class="text-sm font-normal underline sm:text-base" href="https://github.com/vestauth/vestauth" target="_blank" rel="noopener noreferrer">github</a>
    <a class="text-sm font-normal underline sm:text-base" href="/docs">docs</a>
  </div>
</section>
