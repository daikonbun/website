# Search Console / Bing Webmaster setup

Staging only — this file gives the exact steps for the account owner to run.
No verification has been performed yet. Nothing here should be treated as
"done" until the owner completes it in their own Google/Bing accounts and
confirms the green checkmark on each dashboard.

## 1. Google Search Console

1. Go to https://search.google.com/search-console and sign in with the
   Google account that should own this property.
2. Click **Add property** → choose **Domain** (not "URL prefix"). Domain
   properties cover `daikonbun.com`, `www.daikonbun.com`, and both `http`/
   `https` in one verification, which is what we want.
3. Enter `daikonbun.com` and click **Continue**.
4. Google shows a **DNS TXT record** to add, in this shape:

   ```
   Host:  @  (or daikonbun.com, depending on your DNS provider's UI)
   Type:  TXT
   Value: google-site-verification=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
   ```

   Copy the exact value Search Console shows you — it is unique per
   property and is generated only when you reach this step, so it cannot be
   pre-staged here.

5. Add that TXT record at whichever DNS host manages `daikonbun.com`
   (Cloudflare, the domain registrar, etc.):
   - Cloudflare dashboard → DNS → Records → **Add record** → Type `TXT`,
     Name `@`, Content = the value from step 4, Proxy status: irrelevant for
     TXT records (DNS-only either way).
   - Any other registrar: the same fields, usually under "DNS management" or
     "Advanced DNS."
6. Wait for DNS propagation (usually minutes, occasionally up to ~24h), then
   return to Search Console and click **Verify**.
7. Once verified, go to **Sitemaps** in the left nav, enter
   `sitemap.xml`, and click **Submit**. Full URL: `https://daikonbun.com/sitemap.xml`.
8. Go to **URL inspection**, enter `https://daikonbun.com/`, and click
   **Request indexing** once the tool confirms the URL is fetchable.

### Alternative: HTML-file verification

If DNS access isn't available (e.g. domain managed elsewhere without easy
TXT access), Search Console also offers an **HTML file** verification
method instead of Domain/DNS:

1. In Search Console, choose **URL prefix** instead of Domain, enter
   `https://daikonbun.com/`, and pick the **HTML file** method.
2. Download the file it gives you (named like
   `google1234567890abcdef.html`).
3. Place that exact file, unmodified, at the repo root here (same level as
   `index.html`) so it serves at `https://daikonbun.com/google1234...html`.
4. Commit and push, wait for the GitHub Pages rebuild, confirm the file is
   reachable at that URL, then click **Verify** in Search Console.
5. Note: URL-prefix verification only covers `https://daikonbun.com/`, not
   the `www` or `http` variants — the DNS/Domain method above is preferred
   if DNS access is available.

The actual file contents for this method can only come from the owner's
Search Console session (each file is unique to the property + account) — do
not fabricate a placeholder file with guessed contents.

## 2. Bing Webmaster Tools

1. Go to https://www.bing.com/webmasters and sign in.
2. Click **Add a site**, enter `https://daikonbun.com/`.
3. Bing offers an **Import from Google Search Console** option once GSC is
   verified (step 1 above) — this is the fastest path and requires no
   additional DNS/file work. Authorize the connection and select the
   `daikonbun.com` property.
4. If importing isn't available, Bing also supports:
   - **DNS TXT record** verification (same mechanics as Google's, but with
     a Bing-issued value — add as a second TXT record alongside Google's,
     don't replace it).
   - **HTML file** verification (same mechanics as Google's HTML-file
     method above — a separate, Bing-issued file).
5. Once verified, go to **Sitemaps** in the left nav and submit
   `https://daikonbun.com/sitemap.xml`.
6. Use **URL Inspection** → **Request Indexing** on `https://daikonbun.com/`.

## 3. After both are verified

- Confirm both dashboards show a verified/green status for the property.
- Confirm the sitemap shows as "Success" (Google) / "Submitted" (Bing) —
  this can take a few minutes to a few hours to move past "Pending."
- Re-run a rich-results check to confirm the JSON-LD is still parsed
  cleanly after any recent edits: https://search.google.com/test/rich-results
- Optional: set up email alerts in both dashboards for coverage/manual
  action issues so problems surface without manually checking back.
