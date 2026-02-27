# Surf World Monitor: where variants differ + deployment runbook

## 1) Where the "single codebase, one-click variant switch" is implemented

The variant architecture is controlled in these files:

1. `src/config/variant.ts`
   - Chooses the active variant (`VITE_VARIANT` at build time, or localStorage at localhost/desktop).
   - Local switching key is `localStorage['worldmonitor-variant']`.

2. `src/app/panel-layout.ts`
   - Renders the header switcher UI (World / Tech / Finance / Surf / Good News).
   - On production domains, each option can link to a different deployment.

3. `src/app/event-handlers.ts`
   - Handles clicks on variant links in local mode.
   - Writes the selected variant to localStorage and reloads.

4. `src/config/panels.ts`
   - Defines which panels and map layers are enabled for each variant.
   - The active exports are selected from `SITE_VARIANT`.

5. `src/config/variants/surf.ts`
   - Surf-specific panel and map-layer preset scaffold.

## 2) What to customize for a true surf-only vertical

To make `surf` distinct, focus on:

- Panel inventory: keep only surf-relevant modules in `SURF_PANELS`.
- Feed categories: add surf-focused feeds into `src/config/feeds.ts` and map them to surf panel IDs.
- Category tabs: adjust `PANEL_CATEGORY_MAP` for surf user journeys (conditions, competitions, hazards, athletes).
- Data adapters/loaders: for APIs that are not RSS (NOAA APIs, WindGuru endpoints, etc.), add sidecar/API routes.

## 3) Suggested surf news/data modules (expanded)

In addition to your list, add:

- **Marine safety**: rip current statements, harmful algal blooms, beach closures, bacteria alerts.
- **Forecast quality**: model disagreement panel (GFS vs ECMWF surf-relevant wind windows).
- **Big-wave watch**: buoy + pressure gradient + storm-track panel for XXL conditions.
- **Travel friction**: visa/strike/airport disruptions impacting surf travel.
- **Ocean conservation**: MPA policy changes, coastal development permits, reef restoration.
- **Local economy**: surf retail, board/wetsuit manufacturing, event tourism impact.
- **Talent pipeline**: junior circuits, challenger series, ISA pathway to Olympics.

## 4) One-hour deployment plan (Vercel)

### 0-20 min: ship surf variant build
1. Add/verify variant switch + `surf` support in config (done in this fork).
2. In Vercel project settings, add environment variable for production:
   - `VITE_VARIANT=surf`
3. Trigger redeploy.

### 20-40 min: validate and stabilize
1. Open deployed site.
2. Confirm header shows **Surf** and surf panels load.
3. Check console/network for failed feeds.
4. Keep feed count small first (10-20 robust sources), then expand.

### 40-60 min: domain + DNS
1. In Vercel Project → **Settings → Domains** add `surf.yourdomain.com`.
2. In DNS provider, create:
   - `CNAME surf -> cname.vercel-dns.com`
3. Wait for verification + SSL issuance.
4. Set this as primary domain for surf project.

## 5) Detailed Vercel deployment steps

1. Push branch to GitHub.
2. In Vercel: **Add New Project** (or open existing).
3. Framework preset: Vite (auto-detected).
4. Build command: `npm run build` (or `npm run build:full` if you keep full fallback).
5. Output dir: `dist`.
6. Environment variables:
   - `VITE_VARIANT=surf`
   - Add any API keys if you later integrate premium feeds.
7. Deploy.

## 6) Custom domain association checklist

- Buy/own domain at registrar (Hostinger/Cloudflare/Namecheap/etc.).
- Add domain in Vercel first (it shows DNS records to configure).
- Create required DNS records at registrar.
- Wait until Vercel marks domain as **Valid Configuration**.
- Enable redirect from `www` to root/subdomain as needed.

## 7) Ollama options (local + hosted)

### Local Ollama sidecar (fastest)
- Run Ollama where your sidecar/API has network access.
- Expose only private/internal endpoint for summarization/classification.
- Keep model small first (`llama3.1:8b`-class) for latency/cost balance.

### Hosted options (if you do not self-host)
- VM providers (Hetzner, DigitalOcean, AWS/GCP/Azure) with GPU/CPU instances.
- Managed inference alternatives (OpenRouter, Together, Fireworks, etc.) if Ollama hosting is not available.

### Hostinger / OpenClaw note
- Hostinger commonly supports VPS-level custom deployments.
- If they provide one-click Ollama, use their marketplace/app catalog in your VPS panel.
- If no one-click exists, deploy manually on VPS with Docker/systemd and expose a secured API.
- OpenClaw can be used similarly if it provides container/VPS control + reverse proxy.

## 8) Hosting buying options (practical)

- **Vercel**: best for frontend and edge API routes.
- **Hostinger VPS**: good for Ollama/API sidecar if you already have account.
- **Hetzner/DigitalOcean**: strong price/performance for always-on model workers.
- **Cloudflare**: DNS, WAF, caching in front of your Vercel app/API.

## 9) Recommended MCP servers you can add later

For hard-to-fetch/non-RSS sources, create MCP connectors for:

- NOAA/NWS marine + tsunami endpoints
- Surfline/MagicSeaweed/WindGuru normalization
- YouTube live surf channels + event streams
- WSL/ISA event schedule parser
- Surfrider/coastal policy trackers

Use MCP when source auth, HTML parsing, or rate-limiting requires controlled server-side integration.
