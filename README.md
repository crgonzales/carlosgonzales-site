# Carlos Gonzales · Engineering portfolio

[**carlosgonzales.dev**](https://carlosgonzales.dev/)

A personal portfolio covering spacecraft guidance, navigation and control,
flight hardware, and perception for autonomous vehicles. Static HTML and assets,
with a live docking simulator embedded on supported desktop browsers.

## Featured work

- [Orbital Docking GNC Lab](https://github.com/crgonzales/docking-sim): six-DOF spacecraft dynamics, navigation, docking autopilot and verification.
- [Autonomous-driving perception](https://github.com/crgonzales/sim2real-fcos3d-ros2): a ROS 2 camera pipeline for 3D object detection, with performance and robustness evaluation.

The simulator has its own repository and Cloudflare Pages deployment. This
repository owns the portfolio presentation.

## Simulator architecture

Development snapshot: **September 16, 2026**.

**🟢 Done · 🟡 In progress · 🔴 TBC / Not started**

Solid arrows show existing paths. Dashed arrows show unfinished integrations.

[![Docking simulator architecture, including simulation, flight software, rendering, GNC tools, Monte Carlo and MATLAB.](assets/docking-architecture.png)](https://carlosgonzales.dev/#sim-architecture)

[Open the zoomable diagram](https://carlosgonzales.dev/#sim-architecture) ·
[SVG](assets/docking-architecture.svg) ·
[Mermaid source](assets/docking-architecture.mmd)

Colors apply to the scope named in each block, not deployment status. The green
renderer is the existing Takram baseline; Volumetric Weather is in progress.
The offline MATLAB/Simulink plant has been verified separately. Live MATLAB /
SIL / HWIL integration and remote Runpod execution remain planned.

The source map lives in `docs/6-memo/codebase-map.md` in the simulator repository.
Keep both copies synchronized, preserving all 14 blocks, 20 connections and
status assignments. The website serves a pre-rendered SVG with a zoom dialog;
GitHub uses the matching PNG for reliable preview. No diagram engine runs in
the visitor's browser.

## Preview locally

```sh
python3 -m http.server 8000
```

Open [localhost:8000](http://localhost:8000/). There is no build or installation step.

## Publish

Cloudflare Workers Builds connects this repository's `main` branch to the
**carlosgonzales-site** Worker. A push to `main` runs the configured
`npx wrangler deploy` command and publishes the static site at
[carlosgonzales.dev](https://carlosgonzales.dev/).

After pushing, check the **Workers Builds: carlosgonzales-site** GitHub status
and verify the live page. The simulator deploys separately at
[docking-sim.pages.dev](https://docking-sim.pages.dev/).

## Simulator links and embed

| Reference | Purpose |
| --- | --- |
| `SIM_URL` | Loads `https://docking-sim.pages.dev/` when Play is pressed. |
| `SIM_BACKGROUND_URL` | Uses `?mode=sandbox` for the ambient autopilot background. |
| `#play-link` | Links to `/docking-sim`, a Cloudflare redirect to the separate simulator deployment. |

The iframe loads after the portfolio's first paint. On supported desktop
browsers it appears behind the content, blurred and dimmed, until Play makes
it interactive. **Back to portfolio** or **Escape** returns to the page.
Narrow screens, touch devices and reduced-motion preferences skip the embed
and use the direct link instead.

If the simulator host changes, update all three references and preserve its
iframe permissions. The `/docking-sim` route is a redirect, not a local build
or reverse proxy.

## Files and attribution

- `index.html`: page content, styling and interaction.
- `assets/docking-architecture.*`: the shared architecture snapshot.
- Public contact details omit phone number and street address.
- Keep the Credits section aligned with the simulator's `apps/web/public/assets/ASSETS.md`, including model creators, licenses, modifications and imagery/terrain sources.
