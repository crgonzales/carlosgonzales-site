# carlosgonzales.dev

Personal site. Static HTML and assets, no build step — deliberately outside the docking-sim
repo and outside its TRIP release flow.

## Local preview

```sh
python3 -m http.server 8000
# → http://localhost:8000
```

## Deploy

Deployed as a Cloudflare Worker with static assets (`carlosgonzales-site`), served on the
apex domain `carlosgonzales.dev`. Workers Builds is connected to this repo: every push to
`main` runs `npx wrangler deploy` and publishes a new version automatically. No build
command, no Node version, no install step.

CLI alternative (authenticate first with `npx wrangler login` — never paste an API key
into a chat or commit one):

```sh
npx wrangler deploy
```

## Wiring the sim

The simulator lives in the separate `docking-sim` repo and is deployed on its own as a
Cloudflare Pages project: **https://docking-sim.pages.dev**. It is embedded here rather
than merged in.

Two places reference it, and they must stay in sync:

| Where | Value | Purpose |
| ------------------------------- | -------------------------------- | ------------------------------------------------- |
| `SIM_URL` (2nd `<script>` block) | `https://docking-sim.pages.dev/` | loaded into the `<iframe>` when Play is pressed (opens the first-docking mission) |
| `SIM_BACKGROUND_URL` | `SIM_URL + '?mode=sandbox'` | initial `src` of the blurred background `<iframe>` (ambient autopilot approach, no briefing dialog) |
| `#play-link` `href` | `/docking-sim` | Shareable link / fallback for non-immersive visits |

`carlosgonzales.dev/docking-sim` is a Cloudflare **Redirect Rule** (302, wildcard
`https://carlosgonzales.dev/docking-sim*`) pointing at the Pages deployment. It is a
redirect, not a reverse proxy — serving the sim *at* that path without a redirect would
require rebuilding it with Vite `base: '/docking-sim/'`.

### How the embed behaves

- The sim runs in a fixed, full-viewport `<iframe>` behind the content, blurred, dimmed
  and `pointer-events: none`.
- It is loaded on `window.load` + 400 ms so the three.js bundle never competes with
  first paint.
- Clicking **Play** adds `body.focused`: the iframe sharpens and becomes interactive,
  the portfolio text and starfield fade out. **← Back to portfolio** or **Escape**
  reverses it.
- Narrow viewports, touch devices and `prefers-reduced-motion: reduce` skip the embed
  entirely and just follow the `#play-link` href.

If the sim ever moves, check that the new host does not send `X-Frame-Options: DENY` or a
restrictive `Content-Security-Policy: frame-ancestors` — the whole approach depends on it
being iframe-able. Cloudflare Pages does not set either by default.

## Content notes

- The docking project contains a dated architecture snapshot in `assets/docking-architecture.svg`, with the matching Mermaid source in `assets/docking-architecture.mmd`. It is exported from `docs/6-memo/codebase-map.md` in the simulator repository. Preserve all node IDs, connections and status assignments when updating it. The SVG is rendered in advance; visitors do not load Mermaid or a diagram engine. The page includes the status/connection legend and an accessible zoom dialog with a direct SVG fallback.
- Copy is drawn from the GNC resume. Phone number and street-level address are
deliberately omitted — public page.
- The **Credits** section carries the CC BY 4.0 attributions required for the models the
simulator displays (Crew Dragon by KUBAHA, F/A-18C Hornet by Rhine_Lab_Muelsyse: creator,
license, link, and the modification note) plus the public-domain imagery and terrain sources.
Keep it in sync with `apps/web/public/assets/ASSETS.md` in the sim repo.
