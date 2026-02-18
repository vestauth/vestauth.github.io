---
title: "Blog"
---

<section class="relative min-h-screen overflow-hidden px-5 py-8 sm:px-12 sm:py-10">
  {% include layouts/globe.html root_id="globe-bg-blog" %}

  <div class="relative z-10 max-w-4xl font-mono text-xs leading-5 text-black dark:text-white sm:text-sm sm:leading-6 [text-transform:lowercase]">
    <p class="break-words [overflow-wrap:anywhere]"><span class="text-[#1a7f52] dark:text-[#30ff8a]">$</span> <a class="underline decoration-[#1a7f52]/50 underline-offset-4 hover:decoration-[#1a7f52] dark:decoration-[#30ff8a]/50 dark:hover:decoration-[#30ff8a]" href="/">vestauth</a> / blog</p>

    <section class="mt-6 sm:mt-8">
      <p class="max-w-3xl text-zinc-600 dark:text-zinc-400">short notes on agentic auth and its tools.</p>
    </section>

    <section class="mt-5 sm:mt-6">
      <ul class="list-disc space-y-3 pl-5 marker:text-[#1a7f52] dark:marker:text-[#30ff8a]">
        {% for post in site.categories.blog %}
          <li class="max-w-3xl">
            <a class="underline decoration-[#1a7f52]/50 underline-offset-4 hover:decoration-[#1a7f52] dark:decoration-[#30ff8a]/50 dark:hover:decoration-[#30ff8a]" href="{{ post.url }}">{{ post.title | downcase }}</a>
            <span class="ml-2 text-zinc-500 dark:text-zinc-400">{{ post.date | date: "%Y-%m-%d" }}</span>
          </li>
        {% endfor %}
      </ul>
    </section>
  </div>

  {% include layouts/site-footer.html %}
</section>
