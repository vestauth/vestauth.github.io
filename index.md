---
title: ""
---

<section class="relative min-h-screen overflow-hidden px-5 py-8 sm:px-12 sm:py-10">
  <div aria-hidden="true" class="pointer-events-none absolute inset-y-0 right-[-220px] z-0 hidden w-[760px] items-center opacity-20 dark:opacity-30 md:flex lg:right-[-180px] lg:w-[860px]">
    <div id="globe-bg" class="h-[760px] w-[760px] lg:h-[860px] lg:w-[860px]"></div>
  </div>

  <div class="relative z-10 max-w-5xl font-mono text-xs leading-5 text-black dark:text-white sm:text-sm sm:leading-6">
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

// vestauth agent curl https://ping.vestauth.com/ping
app.get('/whoami', async (req, res) =&gt; {
  try {
    const url = `${req.protocol}://${req.get('host')}${req.originalUrl}`
    const agent = await vestauth.provider.verify(req.method, url, req.headers)
    res.json(agent)
  } catch (err) {
    res.status(401).json({ code: 401, error: { message: err.message }})
  }
})

app.listen(3000)</code></pre>
    </section>

  </div>

  <div class="mt-10 flex items-center gap-4 text-base font-semibold tracking-tight text-black dark:text-white sm:mt-14 sm:text-lg">
    <span>vestauth</span>
    <a class="text-sm font-normal underline sm:text-base" href="https://github.com/vestauth/vestauth" target="_blank" rel="noopener noreferrer">github</a>
    <a class="text-sm font-normal underline sm:text-base" href="/docs">docs</a>
  </div>

  <script src="https://unpkg.com/globe.gl"></script>
  <script>
    (() => {
      const root = document.getElementById('globe-bg');
      if (!root || typeof Globe === 'undefined') return;
      if (!window.matchMedia('(min-width: 768px)').matches) return;

      const globe = Globe()
        .backgroundColor('rgba(0,0,0,0)')
        .pointColor((d) => 'rgba(255,255,255,' + (d.opacity ?? 1) + ')')
        .pointRadius((d) => d.r)
        .pointAltitude((d) => d.altitude)
        .pointOfView({ altitude: 2.3, lat: 20, lng: -20 }, 0)
        .pointsData([])
        .htmlElementsData([])
        (root);

      const material = globe.globeMaterial();
      material.color.set('#dff7ec');
      material.emissive.set('#c9efdd');
      material.emissiveIntensity = 0.03;
      material.shininess = 0.02;
      material.transparent = true;
      material.opacity = 0.24;

      globe.atmosphereColor('#9fe5c5');
      globe.atmosphereAltitude(0.03);
      const isDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
      globe
        .polygonCapColor(() => 'rgba(0, 0, 0, 0)')
        .polygonSideColor(() => 'rgba(0, 0, 0, 0)')
        .polygonStrokeColor(() => isDark ? 'rgba(184, 255, 220, 0.58)' : 'rgba(26, 90, 62, 0.5)')
        .polygonAltitude(0.006);

      fetch('https://cdn.jsdelivr.net/gh/vasturiano/three-globe@master/example/country-polygons/ne_110m_admin_0_countries.geojson')
        .then((res) => res.json())
        .then((countries) => {
          if (countries && Array.isArray(countries.features)) {
            globe.polygonsData(countries.features);
          }
        })
        .catch(() => {});

      function fitGlobe() {
        const rect = root.getBoundingClientRect();
        globe.width(Math.max(240, rect.width));
        globe.height(Math.max(240, rect.height));
      }

      fitGlobe();
      window.addEventListener('resize', fitGlobe, { passive: true });

      const controls = globe.controls();
      controls.autoRotate = true;
      controls.autoRotateSpeed = 0.28;
      controls.enableZoom = false;
      controls.enablePan = false;

      const pointGrowMs = 180;
      const pointHoldMs = 1200;
      const pointFadeMs = 2400;
      const pointRadius = 0.05;
      const activePoints = [];
      let lastSeen = 0;

      globe.pointsData(activePoints);

      async function pullPings() {
        try {
          const res = await fetch('https://ping.vestauth.com/pings?since=' + encodeURIComponent(lastSeen));
          if (!res.ok) return;
          const batch = await res.json();
          if (!Array.isArray(batch) || batch.length === 0) return;

          let maxSeen = lastSeen;
          for (let i = 0; i < batch.length; i += 1) {
            const ping = batch[i];
            if (typeof ping.ts === 'number' && ping.ts > maxSeen) maxSeen = ping.ts;
            if (typeof ping.id === 'number' && ping.id > maxSeen) maxSeen = ping.id;
            activePoints.push({
              lat: ping.lat,
              lng: ping.lng,
              altitude: 0,
              maxAltitude: ping.altitude ?? 90,
              r: pointRadius,
              opacity: 0.9,
              born: performance.now(),
              grown: false
            });
          }
          lastSeen = maxSeen;
        } catch (_) {}
      }

      function animatePoints() {
        const now = performance.now();
        let changed = false;
        for (let i = 0; i < activePoints.length; i += 1) {
          const p = activePoints[i];
          const age = now - p.born;
          const lifeMs = pointGrowMs + pointHoldMs + pointFadeMs;
          if (age >= lifeMs) {
            activePoints.splice(i, 1);
            i -= 1;
            changed = true;
            continue;
          }

          if (age <= pointGrowMs) {
            const growT = Math.min(1, age / pointGrowMs);
            const nextAltitude = p.maxAltitude * growT;
            if (nextAltitude !== p.altitude) {
              p.altitude = nextAltitude;
              changed = true;
            }
          } else if (!p.grown) {
            p.altitude = p.maxAltitude;
            p.grown = true;
            changed = true;
          }

          const fadeStart = pointGrowMs + pointHoldMs;
          if (age >= fadeStart) {
            const fadeT = Math.min(1, (age - fadeStart) / pointFadeMs);
            const nextOpacity = Math.max(0, 0.9 - fadeT * 0.9);
            const nextAltitude = Math.max(0, p.maxAltitude * (1 - fadeT));
            if (nextOpacity !== p.opacity) {
              p.opacity = nextOpacity;
              changed = true;
            }
            if (nextAltitude !== p.altitude) {
              p.altitude = nextAltitude;
              changed = true;
            }
          }
        }

        if (changed) {
          globe.pointsData(activePoints.slice());
        }
        requestAnimationFrame(animatePoints);
      }

      pullPings();
      setInterval(pullPings, 800);
      animatePoints();
    })();
  </script>
</section>
