# Deploying the Viraj Form Tutor Portal on Netlify

## What must be in the site (keep this exact structure)
```
index.html
config.js            (leave window.VIRAJ_API empty)
netlify.toml         (at the TOP level)
_headers  _redirects  robots.txt  package.json
viraj-logo.png  viraj-icon.png  google75c4d956ec608c89.html
netlify/functions/api.mjs      <- the server. Without this file login fails.
```

## Option A — GitHub (recommended, works every time)
1. Create a GitHub repository and upload everything in this folder (keep the `netlify/functions` folder).
2. Netlify -> Add new site -> Import from Git -> pick the repo.
3. Build command: leave empty. Publish directory: `.`  (netlify.toml already sets this). Deploy.

## Option B — Netlify CLI
```
npm i -g netlify-cli
netlify login
netlify deploy --prod --dir=.
```
(run inside this folder)

> Do NOT just drag the folder onto the Netlify "Drop" page: the server function and
> its storage are often not connected that way, which causes
> "The portal server did not answer correctly".

## Required setting (one time)
Netlify -> Site configuration -> Environment variables -> add:

| Name | Value |
|---|---|
| `ADMIN_EMAIL` | the administrator's email, e.g. `admin@viraj.com` (several allowed, comma separated) |
| `ACCESS_CODE` | a private school access code of your choice (tutors need it to register) |

Then **Deploys -> Trigger deploy -> Clear cache and deploy site**.

## First run
1. Open `https://YOUR-SITE.netlify.app/api` — you should see `{"service":"Viraj Form Tutor Portal",...,"storage":"ok","adminConfigured":true}`.
2. Register with the ADMIN_EMAIL address and your ACCESS_CODE -> you land in the Admin Center.
3. Manage Classes -> add classes. Manage Students -> type learners or import Excel.
4. Tutors register, pick their class, and see only that class. Messages you post reach them within ~15 seconds.

## If login still fails
Open `/api` (step 1). If it is not the JSON above, check Netlify -> Logs -> Functions -> api.
The portal now shows the real error text instead of a generic message.
