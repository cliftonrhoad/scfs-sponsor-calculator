# SCFS Sponsor Menu Calculator

The 2026 Sponsor Menu Calculator for [Summer Camp for Songwriters](https://www.summercampforsongwriters.com) — a self-contained web app served via GitHub Pages and embedded on the Squarespace site.

## Architecture

- **`index.html`** — the entire app. Brand fonts embedded as data URIs; no build step.
- **Rate card lives in Supabase**, not in this repo. The client fetches `rate_config` at boot; unit prices, rights weights, and rider math never appear in this source.
- **Auth**: Supabase email OTP (6-digit code, open signup). Signed-in users get six cloud save slots for proposal builds.
- **Admin** (allow-listed emails in the `admins` table): full pricing UI + a rate-card editor that publishes rates back to the cloud for everyone.
- **`supabase/`** — auth configuration (`config.toml`) and the OTP email template, pushed with `supabase config push`.

## Embedding

```html
<iframe src="https://cliftonrhoad.github.io/scfs-sponsor-calculator/"
        style="width:100%;height:1600px;border:0" title="2026 Sponsor Menu Calculator"
        loading="lazy"></iframe>
```
