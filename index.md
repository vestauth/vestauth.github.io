---
title: ""
---

<section class="relative min-h-screen overflow-hidden px-5 py-8 sm:px-12 sm:py-10">
  <div aria-hidden="true" class="absolute top-[-10px] right-[-180px] z-20 flex w-[360px] sm:right-[-210px] sm:w-[460px] md:top-[-20px] md:right-[-240px] md:w-[560px] lg:right-[-220px] lg:w-[640px]">
    <div id="globe-bg" class="pointer-events-none h-[360px] w-[360px] sm:h-[460px] sm:w-[460px] md:pointer-events-auto md:h-[560px] md:w-[560px] md:cursor-grab md:active:cursor-grabbing lg:h-[640px] lg:w-[640px]"></div>
  </div>

  <div class="relative z-10 max-w-5xl font-mono text-xs leading-5 text-black dark:text-white sm:text-sm sm:leading-6">
    <p class="break-words [overflow-wrap:anywhere]"><span class="text-[#1a7f52] dark:text-[#30ff8a]">$</span> vestauth agent init</p>

    <section class="mt-6 sm:mt-8">
      <h2 class="text-[#1a7f52] dark:text-[#30ff8a]">auth for agents</h2>
      <p class="mt-3 max-w-5xl italic">from the creator of <a class="underline" href="https://github.com/motdotla/dotenv#readme" target="_blank" rel="noopener noreferrer"><code>dotenv</code></a> and <a class="underline" href="https://github.com/dotenvx/dotenvx#readme" target="_blank" rel="noopener noreferrer"><code>dotenvx</code></a>.</p>
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
      <p class="break-words [overflow-wrap:anywhere]"><span class="text-[#1a7f52] dark:text-[#30ff8a]">$</span> vestauth agent curl https://ping.vestauth.com/ping</p>
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
  <style>
    .ping-label {
      display: grid;
      gap: 2px;
      padding: 6px 8px;
      border-radius: 8px;
      border: 1px solid rgba(45, 52, 64, 0.42);
      background: rgba(248, 249, 251, 0.9);
      color: rgba(18, 22, 30, 0.95);
      font: 600 9px/1.2 "JetBrains Mono", "SF Mono", ui-monospace, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
      letter-spacing: 0.02em;
      text-transform: lowercase;
      white-space: nowrap;
      pointer-events: none;
      box-shadow: 0 8px 18px rgba(0, 0, 0, 0.16);
    }
    .ping-label__title { opacity: 0.78; }
    .ping-label__agent { color: rgba(14, 18, 24, 0.95); }
    .ping-label__kid { opacity: 0.88; }
    .ping-label__meta { opacity: 0.76; }
  </style>
  <script>
    (() => {
      const root = document.getElementById('globe-bg');
      if (!root || typeof Globe === 'undefined') return;

      const globe = Globe()
        .backgroundColor('rgba(0,0,0,0)')
        .pointColor((d) => 'rgba(58, 255, 134, ' + (d.opacity ?? 1) + ')')
        .pointRadius((d) => d.r)
        .pointAltitude((d) => d.altitude)
        .htmlElementsData([])
        .htmlLat((d) => d.lat)
        .htmlLng((d) => d.lng)
        .htmlAltitude((d) => {
          const a = (d.altitude ?? d.maxAltitude ?? 0) * 0.002 + 0.06;
          return Math.min(0.35, Math.max(0.06, a));
        })
        .htmlElement((d) => d.htmlEl || makeHtmlLabel(d))
        .pointsMerge(false)
        .pointOfView({ altitude: 2.3, lat: 20, lng: -20 }, 0)
        .pointsData([])
        (root);

      const renderer = globe.renderer();
      if (renderer && typeof renderer.setClearColor === 'function') {
        renderer.setClearColor(0x000000, 0);
      }

      const material = globe.globeMaterial();
      const darkQuery = window.matchMedia('(prefers-color-scheme: dark)');
      let lineColor = 'rgba(8, 14, 24, 0.98)';

      function resolveDarkMode() {
        const doc = document.documentElement;
        const body = document.body;
        const classDark = (doc && doc.classList.contains('dark')) || (body && body.classList.contains('dark'));
        return classDark || darkQuery.matches;
      }

      function applyThemeToGlobe() {
        const isDark = resolveDarkMode();
        globe.backgroundColor('rgba(0,0,0,0)');
        material.transparent = true;
        if (isDark) {
          material.color.set('#173d2a');
          material.emissive.set('#0a1f15');
          material.emissiveIntensity = 0.12;
          material.shininess = 0.18;
          material.opacity = 0.42;
          globe.atmosphereColor('#3AFF86');
          globe.atmosphereAltitude(0.07);
          lineColor = 'rgba(58, 255, 134, 0.8)';
        } else {
          material.color.set('#eceef0');
          material.emissive.set('#d4d8dd');
          material.emissiveIntensity = 0.03;
          material.shininess = 0.02;
          material.opacity = 0.3;
          globe.atmosphereColor('#dfe3e8');
          globe.atmosphereAltitude(0.03);
          lineColor = 'rgba(8, 14, 24, 0.98)';
        }
      }
      applyThemeToGlobe();
      darkQuery.addEventListener('change', applyThemeToGlobe);
      const themeObserver = new MutationObserver(applyThemeToGlobe);
      if (document.documentElement) {
        themeObserver.observe(document.documentElement, { attributes: true, attributeFilter: ['class', 'data-theme'] });
      }
      if (document.body) {
        themeObserver.observe(document.body, { attributes: true, attributeFilter: ['class', 'data-theme'] });
      }

      globe
        .polygonCapColor(() => 'rgba(0, 0, 0, 0)')
        .polygonSideColor(() => 'rgba(0, 0, 0, 0)')
        .polygonStrokeColor(() => lineColor)
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
      controls.enableRotate = true;
      controls.enableZoom = false;
      controls.enablePan = false;

      const pointGrowMs = 200;
      const pointHoldMs = 2000;
      const pointFadeMs = 5000;
      const pointRadius = 0.12;
      const activePoints = [];
      let lastSeen = 0;
      let pullInFlight = false;

      globe.pointsData(activePoints);
      globe.htmlElementsData(activePoints);

      function shortAgentId(value) {
        if (!value && value !== 0) return 'agent-????';
        const str = String(value);
        const marker = 'agent-';
        const idx = str.indexOf(marker);
        if (idx !== -1) {
          const after = str.slice(idx + marker.length);
          return 'agent-' + after.slice(0, 4).padEnd(4, '?');
        }
        return 'agent-' + str.slice(0, 4).padEnd(4, '?');
      }

      function shortKid(value) {
        if (!value && value !== 0) return 'kid:????';
        const str = String(value);
        return 'kid:' + str.slice(0, 4).padEnd(4, '?');
      }

      function formatMeta(d) {
        const lat = d.lat != null ? d.lat.toFixed(2) : '--';
        const lng = d.lng != null ? d.lng.toFixed(2) : '--';
        const ts = d.ts
          ? new Date(d.ts).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: false })
          : '--';
        return 'loc ' + lat + ',' + lng + ' · ' + ts;
      }

      function makeHtmlLabel(d) {
        const rootEl = document.createElement('div');
        rootEl.className = 'ping-label';
        rootEl.style.opacity = String(d.opacity ?? 1);

        const title = document.createElement('div');
        title.className = 'ping-label__title';
        title.textContent = 'id: ' + (d.id ?? '??');

        const agent = document.createElement('div');
        agent.className = 'ping-label__agent';
        agent.textContent = d.label || 'agent-????';

        const kid = document.createElement('div');
        kid.className = 'ping-label__kid';
        kid.textContent = d.kidShort || 'kid:????';

        const meta = document.createElement('div');
        meta.className = 'ping-label__meta';
        meta.textContent = formatMeta(d);

        rootEl.appendChild(title);
        rootEl.appendChild(agent);
        rootEl.appendChild(kid);
        rootEl.appendChild(meta);

        d.htmlEl = rootEl;
        return rootEl;
      }

      async function pullPings() {
        if (pullInFlight) return;
        pullInFlight = true;
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
              opacity: 1,
              born: performance.now(),
              grown: false,
              label: shortAgentId(ping.agent_id),
              kidShort: shortKid(ping.agent_kid),
              id: ping.id,
              ts: ping.ts,
              htmlEl: null
            });
          }
          lastSeen = maxSeen;
        } catch (_) {
        } finally {
          pullInFlight = false;
        }
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
            const nextOpacity = Math.max(0, 1 - fadeT);
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
          const next = activePoints.slice();
          globe.pointsData(next);
          globe.htmlElementsData(next);
        }

        for (let i = 0; i < activePoints.length; i += 1) {
          const p = activePoints[i];
          if (p.htmlEl) p.htmlEl.style.opacity = String(p.opacity ?? 1);
        }
        requestAnimationFrame(animatePoints);
      }

      pullPings();
      setInterval(pullPings, 1000);
      animatePoints();
    })();
  </script>
</section>
