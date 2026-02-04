---
title: ""
---

<section class="min-h-screen font-mono">
  <div class="px-6 py-12">
    <div class="mx-auto w-full max-w-3xl text-[#d9ffe9]">
      <div>
        <h1 class="text-3xl font-semibold tracking-tight">vestauth</h1>
        <p class="mt-3 text-base text-[#c7f7db]">
          <strong class="text-[#30ff8a]">auth for agents</strong>–from the creator of <a class="underline text-[#b6f2cf] hover:text-[#30ff8a]" href="https://github.com/dotenvx/dotenvx" rel="noopener noreferrer" target="_blank">dotenvx</a>
        </p>
      </div>

      <section id="human" class="mt-10">
        <h2 class="text-xl font-semibold tracking-tight">Human Summary</h2>
        <p class="mt-3 text-base text-[#c7f7db]">
          vestauth lets agents initialize themselves and make authenticated curl calls to vestauth providers with a single command.
        </p>
        <div class="mt-4">
          <a class="text-base underline text-[#b6f2cf] hover:text-[#30ff8a]" href="https://github.com/vestauth/vestauth" rel="noopener noreferrer" target="_blank">github.com/vestauth/vestauth</a>
        </div>
        <div class="mt-6 text-sm text-[#b6f2cf]">
          <a class="underline hover:text-[#30ff8a]" href="mailto:laura@vestauth.com">laura@vestauth.com</a>
          <span class="text-[#7fcaa3]"> &amp; </span>
          <a class="underline hover:text-[#30ff8a]" href="mailto:scott@vestauth.com">scott@vestauth.com</a>
        </div>
      </section>

      <section id="agent-manifest" class="mt-12 rounded-2xl border border-[#1a3b2c] bg-[#0c241a] p-6 shadow-[0_0_0_1px_rgba(48,255,138,0.05),0_0_30px_rgba(48,255,138,0.08)]">
        <div class="flex flex-wrap items-center justify-between gap-3">
          <div class="flex items-center gap-3">
            <h2 class="text-lg font-semibold tracking-tight">Agent Manifest</h2>
            <span class="rounded-full border border-[#1f6a4a] bg-[#0b2a1d] px-2 py-0.5 text-[10px] uppercase tracking-[0.2em] text-[#7fffb2]">manifest v1</span>
          </div>
          <span class="text-xs text-[#b6f2cf]">schema_version: 1.0</span>
        </div>

        <dl class="mt-5 grid gap-4 text-sm">
          <div>
            <dt class="text-[#7fcaa3]">product_name</dt>
            <dd class="mt-1">vestauth</dd>
          </div>
          <div>
            <dt class="text-[#7fcaa3]">purpose</dt>
            <dd class="mt-1">Auth for agents. One command to sign up for services.</dd>
          </div>
          <div>
            <dt class="text-[#7fcaa3]">primary_users</dt>
            <dd class="mt-1">AI agents, service providers</dd>
          </div>
          <div>
            <dt class="text-[#7fcaa3]">core_flow</dt>
            <dd class="mt-1">
              <ol class="mt-2 list-decimal space-y-1 pl-5 text-[#c7f7db]">
                <li>Install CLI: <span class="text-[#d9ffe9]">curl -sSf https://vestauth.sh | sh</span></li>
                <li>Initialize agent: <span class="text-[#d9ffe9]">vestauth agent init</span></li>
                <li>Verify identity: <span class="text-[#d9ffe9]">vestauth agent curl https://api.vestauth.com/whoami</span></li>
              </ol>
            </dd>
          </div>
          <div>
            <dt class="text-[#7fcaa3]">auth_model</dt>
            <dd class="mt-1">Delegated, scoped access (agent-first)</dd>
          </div>
          <div>
            <dt class="text-[#7fcaa3]">integration_surface</dt>
            <dd class="mt-1">CLI + API (public details forthcoming)</dd>
          </div>
          <div>
            <dt class="text-[#7fcaa3]">contact</dt>
            <dd class="mt-1">laura@vestauth.com, scott@vestauth.com</dd>
          </div>
          <div>
            <dt class="text-[#7fcaa3]">last_updated</dt>
            <dd class="mt-1">2026-02-04</dd>
          </div>
        </dl>

        <script type="application/json" data-agent-manifest="vestauth">
          {
            "schema_version": "1.0",
            "product_name": "vestauth",
            "purpose": "Auth for agents. One command to sign up for services.",
            "primary_users": ["AI agents", "service providers"],
            "core_flow": [
              "curl -sSf https://vestauth.sh | sh",
              "vestauth agent init",
              "vestauth agent curl https://api.vestauth.com/whoami"
            ],
            "auth_model": "delegated_scoped_access",
            "integration_surface": "cli_api",
            "contact": "laura@vestauth.com, scott@vestauth.com",
            "last_updated": "2026-02-04"
          }
        </script>
      </section>
    </div>
  </div>
</section>
