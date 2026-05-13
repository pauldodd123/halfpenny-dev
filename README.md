# halfpenny.dev

The studio site. One static page, plain HTML and CSS, no build step. Designed to be edited by hand and served from GitHub Pages.

## Stack

- **HTML5 / CSS3** — no framework, no build step
- **Fonts**: Playfair Display + Inter, from Google Fonts (CDN)
- **Hosting**: GitHub Pages
- **DNS**: IONOS (apex `halfpenny.dev` + `www.halfpenny.dev` aliased to GitHub Pages)

## File layout

```
halfpenny-site/
├── index.html        ← the page
├── styles.css        ← all styling
├── CNAME             ← tells GitHub Pages which domain to serve
├── assets/
│   ├── logo.svg              ← full wordmark (dark text, copper accent)
│   ├── logo-white.svg        ← wordmark for dark backgrounds
│   ├── logo-mark.svg         ← just the halfpenny coin
│   └── og-image.png          ← social share image
└── README.md
```

## Local preview

No build step. Just open the file:

```bash
# from inside the halfpenny-site folder
python3 -m http.server 8000
# then visit http://localhost:8000
```

Or open `index.html` directly in a browser (note: relative paths starting with `/` need a server).

## Deploying to GitHub Pages

### One-time setup

1. Create a new GitHub repo under your account `pauldodd123`, named something like **`halfpenny-dev`** (any name works, but lowercase, no spaces).
2. From the `halfpenny-site` folder on your machine, push the files:
   ```bash
   git init
   git add .
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/pauldodd123/halfpenny-dev.git
   git push -u origin main
   ```
3. On GitHub, go to **Settings → Pages**:
   - **Source**: Deploy from a branch
   - **Branch**: `main`, folder `/ (root)`
   - Click **Save**.
4. Still in **Settings → Pages**, in the **Custom domain** field, type `halfpenny.dev` and click **Save**. GitHub will read the `CNAME` file in the repo and confirm.
5. Tick **Enforce HTTPS** once the certificate provisions (usually within an hour).

### DNS records to add at IONOS

In the IONOS control panel, open **Domains → halfpenny.dev → DNS**. Add the following:

| Type   | Host name | Value                              | TTL  |
|--------|-----------|------------------------------------|------|
| A      | `@`       | `185.199.108.153`                  | 3600 |
| A      | `@`       | `185.199.109.153`                  | 3600 |
| A      | `@`       | `185.199.110.153`                  | 3600 |
| A      | `@`       | `185.199.111.153`                  | 3600 |
| CNAME  | `www`     | `pauldodd123.github.io.`           | 3600 |

> Notes:
> - The four A records send the apex `halfpenny.dev` to GitHub Pages' edge servers.
> - The CNAME makes `www.halfpenny.dev` redirect to the same place.
> - Trailing dot on `pauldodd123.github.io.` is optional but cleaner.

After saving, DNS usually propagates within 15–60 minutes. GitHub Pages will then auto-provision a free Let's Encrypt SSL certificate for the domain.

### Going live, in order

1. Push the repo to GitHub.
2. Enable Pages and set the custom domain.
3. Add the DNS records at IONOS.
4. Wait 5–60 minutes.
5. Visit `https://halfpenny.dev` — should be live.

## Editing the site

Everything is in `index.html` and `styles.css`. The structure is commented so you can find:

- **Nav** — the sticky header
- **Hero** — top section with the headline and stats
- **Services** — the four cards
- **About** — dark section with bio
- **Contact** — the big email link
- **Footer** — small print

To change copy, edit `index.html`. To change colours, edit the `:root` block at the top of `styles.css` — every colour the site uses is a CSS variable in one place.

## Updating

After any edit:

```bash
git add .
git commit -m "Describe what changed"
git push
```

GitHub Pages redeploys automatically. New version is live in 30–60 seconds.

## Future-proofing

If the site grows (case studies, blog, multiple pages), the natural next step is to migrate to **Astro** or **Eleventy** so we can share a layout/header/footer between pages without duplicating HTML. The current code is small enough that this would take an afternoon.
