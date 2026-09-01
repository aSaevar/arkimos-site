# arkimos.com — landing page

Static landing page for **https://arkimos.com**, served by GitHub Pages from
[`aSaevar/arkimos-site`](https://github.com/aSaevar/arkimos-site).

## Layout

| Path                        | Published? | What it is |
| --------------------------- | ---------- | ---------- |
| `site/`                     | **yes**    | Exactly what gets served. Everything here is public. |
| `site/CNAME`                | yes        | Custom domain. One bare hostname, no scheme. Do not delete — it is what keeps the custom domain pinned across deploys. |
| `site/.nojekyll`            | yes        | Serve files as-is; skip Jekyll processing. |
| `design/`                   | no         | Claude Design `.dc.html` sources. Kept for editing, never served. |
| `.github/workflows/pages.yml` | no       | The deploy. Uploads `./site` only. |

Only `site/` is uploaded, so design sources and repo docs are never exposed.

## Deploying

Push to `main`. That's it — the workflow builds and deploys.

```
git add -A && git commit -m "..." && git push
```

Watch it at <https://github.com/aSaevar/arkimos-site/actions>.

## DNS (Namecheap → arkimos.com → Advanced DNS)

Apex is primary; `www` redirects to it (GitHub does the redirect automatically once
the custom domain is set to the apex).

**Host Records — add:**

| Type   | Host | Value                | TTL       |
| ------ | ---- | -------------------- | --------- |
| A      | `@`  | `185.199.108.153`    | Automatic |
| A      | `@`  | `185.199.109.153`    | Automatic |
| A      | `@`  | `185.199.110.153`    | Automatic |
| A      | `@`  | `185.199.111.153`    | Automatic |
| CNAME  | `www`| `asaevar.github.io.` | Automatic |

Optional IPv6 (`AAAA` on `@`): `2606:50c0:8000::153`, `2606:50c0:8001::153`,
`2606:50c0:8002::153`, `2606:50c0:8003::153`.

**Delete if present:** any `URL Redirect Record` on `@`, any `CNAME` on `www` pointing
at `parkingpage.namecheap.com`, and any `A` record on `@` not in the table above.
A leftover parking record silently shadows the A records.

### Do not touch — these carry email

Removing any of these breaks mail to `saevar@arkimos.com`:

- `MX` `@` → `mail.tutanota.de` (priority 10) — lives under **Mail Settings**, not Host Records
- `TXT` `@` → `v=spf1 include:spf.tutanota.de -all`
- `TXT` `@` → `t-verify=43615dfbf1e8356b1352d4c4deddbb7d`

## Verifying

```
dig +short arkimos.com A          # the four 185.199.x.153
dig +short www.arkimos.com        # asaevar.github.io + those IPs
dig +short arkimos.com MX         # mail.tutanota.de — must still be there
```

Then `https://arkimos.com` should serve the page, and `https://www.arkimos.com` should
301 to it.
