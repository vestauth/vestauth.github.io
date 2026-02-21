---
title: ""
---

<section class="relative min-h-screen overflow-hidden px-5 py-8 sm:px-12 sm:py-10">
  {% include layouts/globe.html %}

  <div class="relative z-10 max-w-5xl font-mono text-xs leading-5 text-black dark:text-white sm:text-sm sm:leading-6">
    <p class="break-words [overflow-wrap:anywhere]"><span class="text-[#1a7f52] dark:text-[#30ff8a]">$</span> vestauth agent init</p>

    <section class="mt-6 sm:mt-8">
      <h2 class="text-[#1a7f52] dark:text-[#30ff8a]">auth for agents</h2>
      <p class="mt-3 max-w-5xl italic">from the creator of <a class="underline" href="https://github.com/motdotla/dotenv#readme" target="_blank" rel="noopener noreferrer"><code>dotenv</code></a> and <a class="underline" href="https://github.com/dotenvx/dotenvx#readme" target="_blank" rel="noopener noreferrer"><code>dotenvx</code></a>.</p>
    </section>

    <section class="mt-4 sm:mt-6">
      <h2 class="text-[#1a7f52] dark:text-[#30ff8a]">Install</h2>
      <div class="mt-2 max-w-2xl text-[10px] leading-4 sm:text-xs">
{% highlight sh %}
$ curl -sSf https://vestauth.sh | sh
{% endhighlight %}
      </div>
    </section>

    <section class="mt-4 sm:mt-6">
      <h2 class="text-[#1a7f52] dark:text-[#30ff8a]">Usage</h2>

      <p class="mt-2 text-zinc-500 dark:text-zinc-500"># Give agents cryptographic identities…</p>
      <div class="mt-2 max-w-2xl text-[10px] leading-4 sm:text-xs">
{% highlight sh %}
$ vestauth agent init
{% endhighlight %}
      </div>

      <p class="mt-4 text-zinc-500 dark:text-zinc-500"># …and sign their curl requests with cryptographic authentication.</p>
      <div class="mt-2 max-w-2xl text-[10px] leading-4 sm:text-xs">
{% highlight sh %}
$ vestauth agent curl https://ping.vestauth.com/ping
{% endhighlight %}
      </div>
    </section>

    <section class="mt-4 sm:mt-6">
      <h2 class="text-[#1a7f52] dark:text-[#30ff8a]">Tool Usage</h2>
      <p class="mt-2 text-zinc-500 dark:text-zinc-500"># Verify requests and safely trust agent identity using cryptographic proof.</p>
      <div class="mt-2 max-w-2xl text-[10px] leading-4 sm:text-xs">
{% highlight js %}
// index.js
const express = require('express')
const vestauth = require('vestauth')
const app = express()

// vestauth agent curl https://ping.vestauth.com/ping
app.get('/whoami', async (req, res) => {
  try {
    const url = `${req.protocol}://${req.get('host')}${req.originalUrl}`
    const agent = await vestauth.tool.verify(req.method, url, req.headers)
    res.json(agent)
  } catch (err) {
    res.status(401).json({ code: 401, error: { message: err.message }})
  }
})

app.listen(3000)
{% endhighlight %}
      </div>
    </section>

  </div>

  {% include layouts/site-footer.html %}
</section>
