# carlosgonzales.dev

Personal site. Static, single file, no build step — deliberately outside the docking-sim
repo and outside its TRIP release flow.

## Local preview

```sh
python3 -m http.server 8000
# → http://localhost:8000
```

## Deploy (Cloudflare Pages)

Dashboard → Workers & Pages → Create → Pages → Connect to Git.

| Setting             | Value      |
| ------------------- | ---------- |
| Framework preset    | None       |
| Build command       | *(empty)*  |
| Build output directory | `/`     |

No build step, so no Node version or install command is needed. Add the apex domain
under **Custom domains** once the first deploy is green.

CLI alternative (authenticate first with `npx wrangler login` — never paste an API key
into a chat or commit one):

```sh
npx wrangler pages deploy . --project-name=carlosgonzales-site
```

## Wiring the sim

The **Play** section links to `#play-link` in `index.html`. It currently points at the
GitHub repo. Once the simulator itself is deployed, change that `href` to the sim's URL.

## Content notes

- Copy is drawn from the GNC resume. Phone number and street-level address are
  deliberately omitted — public page.
- The **Credits** section carries the CC BY 4.0 attribution required for the ESO
  starmap used in the simulator (creator, license, link, and the modification note).
  Keep it in sync with `apps/web/public/assets/ASSETS.md` in the sim repo.
