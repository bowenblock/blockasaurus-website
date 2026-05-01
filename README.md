# Snow Raiders / Blockasaurus marketing site

Static HTML marketing + legal pages. Hosted on GitHub Pages with the
custom domain `blockasaurus.com`.

## Files

| File | Purpose | Linked from App Store? |
|---|---|---|
| `index.html` | Marketing landing page | Marketing URL |
| `privacy.html` | Privacy Policy | Privacy Policy URL (required) |
| `terms.html` | Terms of Use | (linked from footer) |
| `support.html` | Support / FAQ | Support URL |
| `_shared.css` | Styling shared by all pages | — |
| `CNAME` | Tells GitHub Pages to serve at `blockasaurus.com` | — |

## Repo layout

This `website/` folder lives inside the **private** Unity repo
(`bowenblock/snow-raiders`) as the source of truth. GitHub Pages on the
free tier needs a **public** repo, so we mirror these files into a
separate public repo. The Unity repo is the editor; the public repo is
the deploy target.

## One-time setup (do this once)

### 1. Create the public mirror repo

On github.com, create a new **public** repo named
`blockasaurus-website` (under your personal account, or a
`Blockasaurus` org if you create one). Leave it empty — no README, no
.gitignore, no license.

### 2. Push the website files to it

We use a separate clone so the Unity repo stays untangled.

```bash
# One-time clone of the empty public repo somewhere outside SledSurfers:
cd ~
git clone git@github.com:<YOUR_USER_OR_ORG>/blockasaurus-website.git
cd blockasaurus-website

# Copy the website files in:
cp -R /Users/Bo/SledSurfers/website/. .
git add .
git commit -m "Initial marketing site"
git push origin main
```

For future updates, run `cp -R /Users/Bo/SledSurfers/website/. .`
in `~/blockasaurus-website` then `git add . && git commit && git push`.

### 2. Enable GitHub Pages

In the new repo's **Settings → Pages**:
- Source: **Deploy from a branch**
- Branch: `main`, folder `/` (root)
- Custom domain: `blockasaurus.com`
- Check **Enforce HTTPS** once the cert provisions (takes a few minutes)

The `CNAME` file in this folder will already be detected.

### 3. Configure DNS at your domain registrar

Add these records for `blockasaurus.com`:

**Apex (`blockasaurus.com`) — A records → GitHub Pages IPs:**
```
A   @   185.199.108.153
A   @   185.199.109.153
A   @   185.199.110.153
A   @   185.199.111.153
```

**Optional `www` subdomain → CNAME:**
```
CNAME   www   <YOUR_USER_OR_ORG>.github.io
```

DNS can take 5 min to a few hours to propagate. GitHub Pages will then
provision a free Let's Encrypt cert automatically.

### 4. Verify

After DNS propagates, check:
- https://blockasaurus.com/
- https://blockasaurus.com/privacy.html
- https://blockasaurus.com/terms.html
- https://blockasaurus.com/support.html

### 5. Push the URLs to App Store Connect

```bash
cd /Users/Bo/SledSurfers
python3 Automation/sync.py --metadata-only
```

URLs in `Automation/game_config.yaml` are already wired to
`https://blockasaurus.com/...`.

## Updating the site later

Edit the HTML/CSS, commit, push to `main`. GitHub Pages redeploys in
~1 minute. No rebuild step.
