# SCFS Sponsor Menu Calculator

The 2026 Sponsor Menu Calculator for [Summer Camp for Songwriters](https://www.summercampforsongwriters.com) — a self-contained web app served via GitHub Pages and embedded on the Squarespace site.

## Architecture

- **`index.html`** — the entire app. Brand fonts embedded as data URIs; no build step.
- **Rate card lives in Supabase**, not in this repo. The client fetches `rate_config` at boot; unit prices, rights weights, and rider math never appear in this source.
- **Auth**: Supabase email OTP (6-digit code, open signup). Signed-in users get six cloud save slots for proposal builds.
- **Admin** (allow-listed emails in the `admins` table): full pricing UI + a rate-card editor that publishes rates back to the cloud for everyone.
- **`supabase/`** — auth configuration (`config.toml`) and the OTP email template, pushed with `supabase config push`.

## Embedding

Recommended (auto-fit): the frame grows to the calculator's full height so the host
page scrolls as one, and the script streams viewport positions in so the deal bar
keeps floating at the real screen bottom.

```html
<iframe id="scfs-embed" src="https://cliftonrhoad.github.io/scfs-sponsor-calculator/"
        style="width:100%;height:88vh;border:0;display:block" title="2026 Sponsor Menu Calculator"
        loading="lazy"></iframe>
<script>
(function(){
  var f=document.getElementById('scfs-embed');
  var origin='https://cliftonrhoad.github.io';
  var m=location.search.match(/[?&]p=([a-z0-9]+)/i);
  if(m){ f.src+=(f.src.indexOf('?')<0?'?':'&')+'p='+m[1]; }
  function view(){
    var r=f.getBoundingClientRect();
    f.contentWindow.postMessage({scfs:'view',top:-r.top,vh:window.innerHeight},origin);
  }
  window.addEventListener('message',function(e){
    if(e.origin!==origin||!e.data||e.data.scfs!=='height')return;
    f.style.height=e.data.h+'px'; view();
  });
  window.addEventListener('scroll',view,{passive:true});
  window.addEventListener('resize',view);
})();
</script>
```

Bare-iframe fallback (no script allowed): the calculator scrolls inside a
screen-height frame instead.

```html
<iframe src="https://cliftonrhoad.github.io/scfs-sponsor-calculator/"
        style="width:100%;height:88vh;border:0;display:block" title="2026 Sponsor Menu Calculator"
        loading="lazy"></iframe>
```
