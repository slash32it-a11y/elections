# elections.slash32.ca — 2026 Midterm Results

Static site: `index.html` (the page), `broadcast.html` (1920×1080 view for OBS / YouTube Live), `data.json` (results, updated hourly), `_headers` (stops Cloudflare caching data.json).
The page re-reads `data.json` every 60 seconds and redraws when it changes, so visitors never need to refresh.

## One-time setup (about 10 minutes)

1. **GitHub repo**
   - Create a new repository, e.g. `slash32-elections` (public or private).
   - Upload these three files to the root: `index.html`, `data.json`, `_headers`.

2. **Cloudflare Pages**
   - Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git → pick `slash32-elections`.
   - Framework preset: None. Build command: leave empty. Build output directory: `/`.
   - Deploy. You get a `*.pages.dev` address; open it to check the page loads.

3. **Custom domain**
   - In the Pages project → Custom domains → Set up a domain → `elections.slash32.ca`.
   - If slash32.ca's DNS is on Cloudflare, it adds the record for you.
   - If DNS is still at GoDaddy: GoDaddy → DNS for slash32.ca → Add record
     - Type `CNAME`, Name `elections`, Value `<your-project>.pages.dev`, TTL 1/2 hour.
   - Wait for Cloudflare to show the domain as Active (usually minutes; HTTPS is automatic).

4. **Let Claude push the hourly updates**
   - Connect GitHub to Claude in your claude.ai settings (Connectors) and give it access to `slash32-elections`.
   - Tell Claude the repo name (`your-username/slash32-elections`). The hourly scheduled task will then commit a new `data.json` each hour; Cloudflare redeploys automatically in under a minute.

## data.json format

One race per line. `c` = candidates `[name, party D/R/I, vote % or null, incumbent 0/1]`;
`rep` = % of expected vote counted; `s` = `wait` | `count` | `call`; `w` = winning party when called; `by` = outlets that projected it.
`meta.house` = House seats called per party; `meta.updated` = time of last update (Eastern).

You can also edit `data.json` by hand on GitHub — the site picks it up on the next deploy.
