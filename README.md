# QuantumCuriousMinds.com — static site (Phase 1)

Three-page static site for **Quantum-Curious Minds — A Quantum Curiosity Mission**.
Plain HTML/CSS, self-hosted fonts, **no JavaScript, no analytics, no ad pixels, no third-party requests**.
The full club, CMS, database and payments come later (phase 2).

```
index.html        Home: mission, the book, characters, Q-urious Club (coming soon)
about.html        About the book and the mission
contact.html      Waitlist / enquiry form (Netlify Forms, adults only) + privacy note
thank-you.html    Form success page (noindex)
404.html          Not-found page
netlify.toml      Security headers, caching, redirects (/about, /contact, /start)
robots.txt, sitemap.xml, site.webmanifest, favicon.ico, apple-touch-icon.png
assets/           css/, fonts/ (Montserrat + Nunito, OFL), img/ (logo, mark, icons, og-image)
tools/build.py    Regenerates the HTML pages (shared header/footer). Edit content here, then run it.
```

Edit copy in `tools/build.py`, run `python3 tools/build.py`, commit. Preview locally with
`python3 -m http.server 8080` and open http://localhost:8080.

## Deploy

### 1. GitHub
1. Create an empty repo (e.g. `quantumcuriousminds-site`), private is fine. Don't add a README.
2. From this folder:
   ```
   git remote add origin git@github.com:<you>/quantumcuriousminds-site.git
   git branch -M main
   git push -u origin main
   ```

### 2. Netlify
1. Netlify → **Add new site → Import an existing project → GitHub** → pick the repo.
2. Build command: *(leave empty)*. Publish directory: `.` (already set in `netlify.toml`). Deploy.
3. **Forms:** Site configuration → Forms → **Enable form detection**, then trigger a redeploy so the `waitlist` form is registered.
   Add a notification (Forms → Form notifications → Email) so new submissions reach the team.
   Spam: the form has a honeypot (`bot-field`); Netlify's Akismet filtering is on by default.
4. Test: submit the form on the `*.netlify.app` URL → you should land on `/thank-you.html` and see the entry under Forms.

### 3. Domain (Hostinger → Netlify)
In Netlify: **Domain management → Add a domain → `quantumcuriousminds.com`** (also add `www`). Then either:

**Option A — keep DNS at Hostinger** (hPanel → Domains → DNS / Nameservers → DNS records):
| Type  | Name | Value                       | TTL  |
|-------|------|-----------------------------|------|
| A     | @    | `75.2.60.5`                 | 3600 |
| CNAME | www  | `<your-site>.netlify.app`   | 3600 |

Delete any existing parking `A`/`AAAA`/`CNAME` records for `@` and `www` first (Hostinger parking adds them), or the domain may keep showing the parked page.

**Option B — Netlify DNS:** in Netlify choose *Set up Netlify DNS*, then in Hostinger replace the nameservers with the four `dns*.p0*.nsone.net` servers Netlify shows.

Then in Netlify → Domain management → HTTPS: **Verify DNS / Provision certificate** (Let's Encrypt; usually minutes, can take up to 24 h for DNS). Set the primary domain (apex or www) — Netlify redirects the other one.

### Checks after go-live
- `https://quantumcuriousminds.com`, `/about.html`, `/contact.html`, `/nope` (404) all load over HTTPS.
- A test form submission arrives in Netlify Forms.
- Response headers include the CSP (check at securityheaders.com).

## Content rules (from the PRD and Brand & Design Guide)
- No price, buy link, ISBN, stock or partner logos until the client confirms — use "Coming soon" / "Enquire".
- No analytics or ad pixels. If analytics is added later, only on adult-facing pages, behind consent, and update the CSP + privacy note.
- Forms collect adult data only; never add child fields.
- Characters: official book illustrations only (placeholders are in place). Quinny-Qubit's pronouns aren't confirmed, so copy avoids them.
