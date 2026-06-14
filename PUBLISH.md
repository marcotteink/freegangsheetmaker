# How to publish Free Gang Sheet Maker (plain-English guide)

Your whole site is one file: `index.html`. Publishing means putting that file on a web host and pointing **freegangsheetmaker.com** at it.

You said the goal is to update and troubleshoot the site with **Claude Code / Cowork** going forward. That works best with this setup:

> **GitHub holds the code → DigitalOcean publishes it automatically → your domain points at DigitalOcean.**

Once it's wired up, updating the site is just: Claude edits the file, pushes it, and your live site refreshes in about a minute. No re-uploading by hand.

There are two phases. **Phase 1** gets it live (one-time setup). **Phase 2** is how you change it forever after.

---

## Phase 1 — One-time setup

### Step 1. Put the code on GitHub

GitHub is the storage that Claude Code reads and writes.

1. Make a free account at **github.com** if you don't have one.
2. Click the **+** (top right) → **New repository**.
3. Name it `freegangsheetmaker`, set it to **Public**, and click **Create repository**.
4. Leave that page open. The code is already prepared and committed on your computer in this folder. We just need to connect it and push.

I (Claude) can do the connect-and-push step for you live, or you run these three lines in Terminal from inside the folder (replace `YOUR-USERNAME`):

```
git remote add origin https://github.com/YOUR-USERNAME/freegangsheetmaker.git
git branch -M main
git push -u origin main
```

After this, refresh the GitHub page. You should see `index.html` listed. That means the code is safely on GitHub.

### Step 2. Connect DigitalOcean and publish

You already have DigitalOcean. Its **App Platform** hosts static sites for **free** and republishes every time GitHub changes.

1. Go to **cloud.digitalocean.com** → left menu → **App Platform** → **Create App**.
2. Choose **GitHub** as the source. Authorize it to see your repos (first time only).
3. Pick the **freegangsheetmaker** repo, branch **main**.
4. DigitalOcean will detect it. Make sure the resource type is set to **Static Site** (not "Web Service"). There is no build command. the output directory is the repo root (`/`).
5. On the plan screen pick the **free** static tier.
6. Click **Create Resources**. In a minute or two you'll get a live URL like `freegangsheetmaker-xxxx.ondigitalocean.app`. Open it to confirm the site works.

### Step 3. Point your domain at it

1. In the DigitalOcean app → **Settings** → **Domains** → **Add Domain** → enter `freegangsheetmaker.com` (and `www.freegangsheetmaker.com`).
2. DigitalOcean shows you the DNS records to add. There are two common ways:
   - **Easiest:** if you let DigitalOcean manage DNS, it gives you nameservers. Put those nameservers in your domain registrar (wherever you bought the domain). This moves DNS off Replit and onto DigitalOcean.
   - **Or:** keep your current DNS and add the **CNAME / A record** DigitalOcean lists, pointing to the app.
3. DNS changes can take a few minutes to a few hours to spread. DigitalOcean adds a free HTTPS certificate automatically once it sees the domain.

> Note: your domain currently points at **Replit**. Once the new DNS takes effect, the domain serves the DigitalOcean version instead. The old Replit project can be left alone or deleted later. nothing breaks during the switch because the old site keeps serving until DNS flips.

That's it. The site is live on your domain.

---

## Phase 2 — Updating the site with Claude Code (the part you wanted)

From now on, changing the site is simple. In Claude Code (or Cowork), point it at this folder and say what you want, for example:

- "Change the accent color to red."
- "Add a banner linking to dtf.stream."
- "The download button is broken on iPhone, fix it."

Claude edits `index.html`, then runs:

```
git add -A
git commit -m "describe the change"
git push
```

DigitalOcean sees the push and republishes automatically. Your live site updates in about a minute. You never touch a file manager or re-upload anything.

---

## Troubleshooting

- **Site shows old version after an update:** hard-refresh your browser (Cmd+Shift+R). DigitalOcean takes ~1 minute to rebuild after a push.
- **Domain not loading / "not secure":** DNS may still be spreading, or the HTTPS certificate is still being issued. Wait, then recheck. Confirm the DNS records match exactly what DigitalOcean listed.
- **Multi-page ZIP download doesn't work:** the site loads JSZip from a CDN, so it needs internet for ZIP bundling. Single-page PNG downloads work offline.
- **Push rejected / git asks for a password:** GitHub no longer accepts account passwords for git. Use a Personal Access Token (GitHub → Settings → Developer settings → Personal access tokens). Claude can walk you through this.

---

## Simpler fallback (no GitHub, no auto-deploy)

If you just want it live today and don't care about the Claude Code workflow yet:

1. Log into **Hostinger** → **hPanel** → **File Manager**.
2. Open the `public_html` folder of your site.
3. Upload `index.html` (replace any existing one).
4. Point `freegangsheetmaker.com` DNS to Hostinger (hPanel shows the nameservers).

Downside: every future change means uploading the file again by hand. The GitHub + DigitalOcean route above avoids that.

---

*Recommendation: do the GitHub + DigitalOcean setup once. It's a little more work up front, but it's what makes "just have Claude update my site" actually work.*
