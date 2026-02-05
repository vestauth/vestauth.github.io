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
          Vestauth gives agents a cryptographic identity and a simple way to authenticate HTTP requests.
        </p>
        <div class="mt-4">
          <a class="text-base underline text-[#b6f2cf] hover:text-[#30ff8a]" href="https://github.com/vestauth/vestauth" rel="noopener noreferrer" target="_blank">github.com/vestauth/vestauth</a>
        </div>
        <div class="mt-2">
          <a class="text-sm underline text-[#b6f2cf] hover:text-[#30ff8a]" href="/agent-manifest.json">agent-manifest.json</a>
          <span class="text-[#7fcaa3]"> · </span>
          <a class="text-sm underline text-[#b6f2cf] hover:text-[#30ff8a]" href="/provider-manifest.json">provider-manifest.json</a>
        </div>
        <div class="mt-6 text-sm text-[#b6f2cf]">
          <a class="underline hover:text-[#30ff8a]" href="mailto:laura@vestauth.com">laura@vestauth.com</a>
          <span class="text-[#7fcaa3]"> &amp; </span>
          <a class="underline hover:text-[#30ff8a]" href="mailto:scott@vestauth.com">scott@vestauth.com</a>
        </div>
      </section>

      <section id="manifests" class="mt-12 rounded-2xl border border-[#1a3b2c] bg-[#0c241a] p-6 shadow-[0_0_0_1px_rgba(48,255,138,0.05),0_0_30px_rgba(48,255,138,0.08)]" data-manifest-tabs>
        <div class="flex flex-wrap items-center justify-between gap-3">
          <div class="flex flex-wrap items-center gap-2">
            <button
              class="rounded-full border border-[#1f6a4a] bg-[#0b2a1d] px-3 py-1 text-xs font-semibold uppercase tracking-[0.2em] text-[#7fffb2]"
              data-manifest-tab="agent"
              type="button"
            >
              Agent Manifest
            </button>
            <button
              class="rounded-full border border-[#1a3b2c] px-3 py-1 text-xs font-semibold uppercase tracking-[0.2em] text-[#9fd8b6] hover:border-[#1f6a4a] hover:text-[#7fffb2]"
              data-manifest-tab="provider"
              type="button"
            >
              Provider Manifest
            </button>
          </div>
          <span class="text-xs text-[#b6f2cf]">schema_version: 1.0</span>
        </div>

        <div class="mt-5" data-manifest-panel="agent">
          <dl class="grid gap-4 text-sm">
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
              <dd class="mt-1">AI agents</dd>
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
              <dt class="text-[#7fcaa3]">identity_outputs</dt>
              <dd class="mt-1">AGENT_PUBLIC_JWK, AGENT_PRIVATE_JWK, AGENT_ID (stored in .env)</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">public_key_discovery</dt>
              <dd class="mt-1">/.well-known/http-message-signatures-directory</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">signing_standard</dt>
              <dd class="mt-1">RFC 9421 (HTTP Message Signatures)</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">signed_headers</dt>
              <dd class="mt-1">Signature, Signature-Input, Signature-Agent</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">signature_agent_format</dt>
              <dd class="mt-1">sig1=&lt;AGENT_ID&gt;.agents.vestauth.com</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">auth_tag</dt>
              <dd class="mt-1">web-bot-auth</dd>
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
              <dd class="mt-1">2026-02-05</dd>
            </div>
          </dl>

          <script type="application/json" data-agent-manifest="vestauth">
            {
              "schema_version": "1.0",
              "product_name": "vestauth",
              "purpose": "Auth for agents. One command to sign up for services.",
              "primary_users": ["AI agents"],
              "core_flow": [
                "curl -sSf https://vestauth.sh | sh",
                "vestauth agent init",
                "vestauth agent curl https://api.vestauth.com/whoami"
              ],
              "identity_outputs": ["AGENT_PUBLIC_JWK", "AGENT_PRIVATE_JWK", "AGENT_ID"],
              "public_key_discovery": "/.well-known/http-message-signatures-directory",
              "signing_standard": "RFC 9421 (HTTP Message Signatures)",
              "signed_headers": ["Signature", "Signature-Input", "Signature-Agent"],
              "signature_agent_format": "sig1=<AGENT_ID>.agents.vestauth.com",
              "auth_tag": "web-bot-auth",
              "auth_model": "delegated_scoped_access",
              "integration_surface": "cli_api",
              "contact": "laura@vestauth.com, scott@vestauth.com",
              "last_updated": "2026-02-05"
            }
          </script>
        </div>

        <div class="mt-5 hidden" data-manifest-panel="provider">
          <dl class="grid gap-4 text-sm">
            <div>
              <dt class="text-[#7fcaa3]">product_name</dt>
              <dd class="mt-1">vestauth</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">purpose</dt>
              <dd class="mt-1">Verify agent requests and authenticate agents at the provider edge.</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">primary_users</dt>
              <dd class="mt-1">Service providers, tool builders</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">core_flow</dt>
              <dd class="mt-1">
                <ol class="mt-2 list-decimal space-y-1 pl-5 text-[#c7f7db]">
                  <li>Install: <span class="text-[#d9ffe9]">npm install express vestauth --save</span></li>
                  <li>Verify: <span class="text-[#d9ffe9]">vestauth.provider.verify(req.method, fullUrl, req.headers)</span></li>
                  <li>Return agent: <span class="text-[#d9ffe9]">res.json(agent)</span></li>
                </ol>
              </dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">verification_function</dt>
              <dd class="mt-1">vestauth.provider.verify(method, fullUrl, headers)</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">full_url_construction</dt>
              <dd class="mt-1">`${req.protocol}://${req.get('host')}${req.originalUrl}`</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">success_response</dt>
              <dd class="mt-1">HTTP 200 with agent JSON</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">error_response</dt>
              <dd class="mt-1">HTTP 401 with error message</dd>
            </div>
            <div>
              <dt class="text-[#7fcaa3]">last_updated</dt>
              <dd class="mt-1">2026-02-05</dd>
            </div>
          </dl>

          <script type="application/json" data-provider-manifest="vestauth">
            {
              "schema_version": "1.0",
              "product_name": "vestauth",
              "purpose": "Verify agent requests and authenticate agents at the provider edge.",
              "primary_users": ["Service providers", "tool builders"],
              "core_flow": [
                "npm install express vestauth --save",
                "vestauth.provider.verify(req.method, fullUrl, req.headers)",
                "res.json(agent)"
              ],
              "verification_function": "vestauth.provider.verify(method, fullUrl, headers)",
              "full_url_construction": "${req.protocol}://${req.get('host')}${req.originalUrl}",
              "success_response": "HTTP 200 with agent JSON",
              "error_response": "HTTP 401 with error message",
              "last_updated": "2026-02-05"
            }
          </script>
        </div>

        <script>
          (() => {
            const root = document.querySelector('[data-manifest-tabs]');
            if (!root) return;
            const buttons = root.querySelectorAll('[data-manifest-tab]');
            const panels = root.querySelectorAll('[data-manifest-panel]');
            const activeClasses = ['bg-[#0b2a1d]', 'border-[#1f6a4a]', 'text-[#7fffb2]'];
            const inactiveClasses = ['border-[#1a3b2c]', 'text-[#9fd8b6]'];

            const setActive = (name) => {
              buttons.forEach((button) => {
                const isActive = button.dataset.manifestTab === name;
                activeClasses.forEach((cls) => button.classList.toggle(cls, isActive));
                inactiveClasses.forEach((cls) => button.classList.toggle(cls, !isActive));
              });

              panels.forEach((panel) => {
                panel.classList.toggle('hidden', panel.dataset.manifestPanel !== name);
              });
            };

            buttons.forEach((button) => {
              button.addEventListener('click', () => setActive(button.dataset.manifestTab));
            });
          })();
        </script>
      </section>
    </div>
  </div>
</section>
