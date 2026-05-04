# Alchemforge — Static Website

A high-end, minimalist static site for **Alchemforge**, a workflow automation studio in Spring Valley, New York.

Light cream theme, classical serif typography (no soft variation axes), gold gradient accents matching the brand logo. Distinct visual personality on every page. Hand-written HTML, single CSS file, vanilla JS. No build step.

---

## Folder Structure

```
alchemforge/
├── index.html                      # Homepage
├── services.html                   # Services
├── about.html                      # About
├── contact.html                    # Contact (form posts to webhook)
├── roi.html                        # ROI Calculator (interactive)
├── README.md
└── assets/
    ├── css/site.css                # All site styles
    ├── js/site.js                  # Header / mobile menu / reveal-on-scroll
    └── images/
        ├── logo.png                # Full Alchemforge wordmark (transparent PNG)
        ├── logo-mark.png           # Just the triangle (square, transparent)
        ├── favicon.ico             # Multi-size favicon
        ├── favicon-16.png          # Tab icon (16x16)
        ├── favicon-32.png          # Tab icon (32x32)
        ├── favicon-192.png         # Android home (192x192)
        ├── favicon-512.png         # PWA splash (512x512)
        ├── apple-touch-icon.png    # iOS home (180x180)
        ├── illustration-hero.svg
        ├── illustration-workflow.svg
        ├── illustration-process.svg
        ├── illustration-integrations.svg
        ├── illustration-ai.svg
        ├── illustration-about.svg
        └── illustration-contact.svg
```

---

## 1. Logo & Favicon

The site uses your actual Alchemforge wordmark (`assets/images/logo.png`) — transparent PNG, sits cleanly on the cream background of every page.

The favicon set is built from your triangle mark on a warm-black rounded square — like Gmail's red M or YouTube's red play square. It's instantly recognizable in browser tabs.

If you ever want to swap files, keep the same filenames:

- **`logo.png`** — the full wordmark, transparent background, used in header & footer
- **`logo-mark.png`** — just the triangle, transparent background
- **`favicon.ico`** + **`favicon-16/32/192/512.png`** + **`apple-touch-icon.png`** — all the browser/OS icon sizes

---

## 2. Set the Contact Form Webhook URL

The contact form posts JSON to a webhook. Open `contact.html`, scroll to the bottom `<script>` block, and find:

```js
const WEBHOOK_URL = "https://your-webhook-url-here.example.com/alchemforge-contact";
```

Replace it with your endpoint. Compatible with:

- **n8n** webhook nodes
- **Make (Integromat)** custom webhooks
- **Zapier** webhooks (Catch Hook)
- **Formspree** / **Getform** / **Web3Forms**
- A Netlify Function, Cloudflare Worker, or your own backend

Payload format:

```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "phone": "+1 555 555 5555",
  "company": "Acme Co",
  "industry": "Manufacturing",
  "message": "We have a manual onboarding workflow…",
  "source": "alchemforge.com",
  "page": "/contact.html",
  "timestamp": "2026-04-30T18:00:00.000Z"
}
```

The form treats any HTTP 2xx as success. There's a hidden honeypot field (`website`) for bot rejection.

---

## 3. Office Hours

- **Monday — Thursday:** 9:00am — 5:00pm
- **Friday:** 9:00am — 12:00pm

To change: edit `contact.html` (visible hours in the contact info block) and `index.html` (JSON-LD schema in `<head>`).

---

## 4. Deploy to GitHub Pages

1. Create a new GitHub repository (e.g. `alchemforge.github.io` for a user/org site, or `alchemforge-site` for a project site).
2. Commit and push the entire contents of this folder to the `main` branch.
3. Go to **Settings → Pages** in your repo.
4. Under **Source**, choose `Deploy from a branch` and pick `main` / `(root)`.
5. Save. GitHub will give you a URL within a minute.

For a custom domain (e.g. `alchemforge.com`):

1. In **Settings → Pages**, enter your domain under **Custom domain**.
2. Add a file `CNAME` to the root of the repo containing just your domain on one line.
3. Configure DNS:
   - Apex domain: A records pointing to GitHub's Pages IPs (`185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`)
   - `www` subdomain: CNAME pointing to `<your-username>.github.io`

---

## 5. Customizing

### Colors

All tokens live at the top of `assets/css/site.css`:

```css
:root {
  --paper:      #f6f3ec;     /* main background — matches the logo card */
  --ink:        #14120e;     /* primary text — warm near-black */

  /* Gold — matched to your logo gradient */
  --gold:       #d4ad3f;     /* dominant gold */
  --gold-2:     #e0c574;     /* mid */
  --gold-3:     #ebd682;     /* highlight */
  --gold-deep:  #b8902b;     /* deepest, warm */
  --gold-rich:  #caa243;     /* saturated accent */
}
```

Italic accents (`<em class="italic-accent">…</em>`) and the big ROI number use a CSS gradient between these tones — not a flat color — so they read as *gold* rather than brown.

### Fonts

Loaded from Google Fonts in each HTML file:

- **Instrument Serif** — classical serif used for headings (single weight, no soft variation)
- **Inter** — body sans-serif
- **DM Mono** — small uppercase labels

### Animations

Each page has its own moving signature, all in the same family:

- **Home** — small mono-text marquee at the bottom; pulsing sparkle nodes inside the hero illustration; floating connection lines.
- **Services** — large serif-text ticker right under the hero ("Onboarding flows · Invoicing · Lead routing…"); flow lines inside service illustrations animate.
- **About** — vertically rolling word in the hero ("Built for the *slow,* / *careful,* / *thoughtful,* / *patient,* side"); the constellation triangles slowly orbit around the central forge mark.
- **Contact** — paper plane gently floats; trail dashes flow toward it; sparkles pulse around the plane.
- **ROI Calculator** — orbiting dashed ring around the result panel; numbers count up smoothly on every slider change; the big total bumps with a subtle scale animation each time you adjust.

All animations respect `prefers-reduced-motion: reduce`.

---

## 6. ROI Calculator

Three sliders:

- **Hours saved per week** — 1 to 200
- **People affected** — 1 to 500
- **Hourly rate** — $15 to $500

All five output numbers (annual savings, weekly hours, annual hours, weekly $, monthly $) animate together on every change.

---

## 7. Local Preview

```bash
# Python 3
python3 -m http.server 8000

# Node
npx serve .
```

Then visit <http://localhost:8000>.

---

## 8. SEO

Each page already includes:

- Unique `<title>` and `<meta name="description">`
- Open Graph and Twitter card meta tags
- Multi-size favicon set referenced in `<head>`
- Semantic HTML (`<header>`, `<main>`, `<nav>`, `<section>`, `<footer>`)
- A JSON-LD `Organization` schema block on the homepage including office hours
- Skip-to-content link and ARIA labels for accessibility

If you set up a custom domain, update the `og:image` URL in each `<head>` to the absolute URL of your domain.

---

## License & Credits

All copy and design is for Alchemforge. Logo files are provided by Alchemforge.
