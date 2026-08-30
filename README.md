# daikonbun.com

The Daikonbun marketing landing page. Single self-contained `index.html` (no build step) plus
`robots.txt` and `sitemap.xml`.

## Deploy

Any static host works. `index.html` is fully self-contained (only Google Fonts is external).

**Cloudflare Pages (recommended — free, fast, easy custom domain):**
1. Push this folder to `daikonbun/website` on GitHub.
2. Cloudflare dash → Pages → Create → connect the repo → framework preset **None**, build command empty, output dir `/`.
3. Custom domains → add `daikonbun.com` (and `www`). Cloudflare sets the DNS if the domain is on Cloudflare; otherwise add the CNAME it shows you at your registrar.

**Vercel / Netlify:** import the repo, no build command, publish directory `.`, then add `daikonbun.com` as a domain.

## Ship checklist

1. **Newsletter endpoint.** Done — `NEWSLETTER_ENDPOINT` in `index.html` points at the live Buttondown form.
2. **OG image.** Done — `og.png` (1200×630, rendered from `og.svg`) is committed and serving at `https://daikonbun.com/og.png`.
3. **Domain in meta.** Done — `canonical`/`og:url`/sitemap `loc` all point at `https://daikonbun.com/`.
4. **Search Console / Bing verification.** Staged, not yet run — see `SEARCH-CONSOLE.md` for the exact steps (needs the account owner's Google/Bing accounts).

## Get indexed (SEO + AEO)

Already built in: semantic HTML, one `<h1>`, `<title>` + meta description, canonical, Open Graph + Twitter cards, and JSON-LD structured data (`Organization`, `WebSite`, `SoftwareApplication`, and a `FAQPage` mirroring the on-page FAQ). `robots.txt` explicitly allows the major AI/answer-engine crawlers (GPTBot, OAI-SearchBot, PerplexityBot, ClaudeBot, Google-Extended).

After deploy, follow `SEARCH-CONSOLE.md` in this repo for exact, copy-paste steps:
1. **Google Search Console** → add `daikonbun.com`, verify (DNS TXT), submit `https://daikonbun.com/sitemap.xml`, then "Request indexing" on the homepage.
2. **Bing Webmaster Tools** → add + verify (you can import from GSC), submit the sitemap. Bing also feeds several answer engines.
3. **AEO:** the FAQ + `SoftwareApplication` schema give answer engines clean, quotable facts. Keep on-page copy factual and definitional (a crawler should be able to answer "what is Daikonbun?" from the first paragraph — it can). Re-run a rich-results test after deploy: https://search.google.com/test/rich-results
4. Optional but high-signal for AEO/entity recognition: a short, factual presence on a couple of high-authority surfaces once there's a product (a GitHub repo README, a Product Hunt "coming soon", an About page) so engines can corroborate the entity.
