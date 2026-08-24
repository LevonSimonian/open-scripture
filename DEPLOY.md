# Getting this live on Vercel

Everything you need is in this folder. Total time: about twenty minutes, most of it waiting for emails.

## What's in the folder

| File | What it does |
|---|---|
| `index.html` | The entire site. This is the one that matters. |
| `og-image.png` | The preview card that appears when someone shares a link |
| `favicon.svg` | Browser tab icon |
| `apple-touch-icon.png` | Icon when someone saves the site to a phone home screen |
| `vercel.json` | Caching and security headers |
| `robots.txt` | Tells search engines they may index the site |
| `sitemap.xml` | Helps search engines find it |
| `ACCOUNTS-SETUP.md` | Separate guide for switching on sign-in |
| `DEPLOY.md` | This file |

Only the first one is essential. The rest make it behave like a real site rather than a file someone uploaded.

---

# Part 1 — Put the files on GitHub

Vercel deploys from a Git repository. Doing it this way means future changes go live by uploading a file, with no re-deploying.

**1.** Create a free account at **github.com** if you don't have one.

**2.** Click the **+** in the top right → **New repository**.

**3.** Name it `bible-from-scratch`. Leave it **Public**. Don't tick "Add a README". Click **Create repository**.

**4.** On the next screen click **uploading an existing file**.

**5.** Drag in all eight files from this folder. Scroll down, click **Commit changes**.

That's GitHub done.

---

# Part 2 — Deploy on Vercel

**1.** Go to **vercel.com** and click **Sign Up**. Choose **Continue with GitHub** — it makes the next step one click.

**2.** When it asks what you're using Vercel for, choose **Personal**. This puts you on the free Hobby plan.

**3.** On your dashboard click **Add New…** → **Project**.

**4.** Find `bible-from-scratch` in the list and click **Import**. If it isn't there, click **Adjust GitHub App Permissions** and grant access to the repo.

**5.** On the configuration screen:
   - **Framework Preset:** `Other`
   - **Root Directory:** leave as is
   - **Build and Output Settings:** leave everything empty — there is nothing to build

**6.** Click **Deploy**. It takes about thirty seconds.

You'll get a live URL like `bible-from-scratch-abc123.vercel.app`. Open it. The site should work fully — every section, search, progress ticking, share links.

---

# Part 3 — Fix the share preview

The preview image needs to know its own address, which you only learned in Part 2.

**1.** In your GitHub repo, click `index.html`, then the **pencil icon** to edit.

**2.** Press Ctrl+F (Cmd+F on Mac) and search for `YOUR-DOMAIN`. It appears three times, near the top.

**3.** Replace all three with your actual Vercel domain. So:

```
https://YOUR-DOMAIN.vercel.app/og-image.png
```
becomes
```
https://bible-from-scratch-abc123.vercel.app/og-image.png
```

**4.** Do the same in `robots.txt` and `sitemap.xml` (once each).

**5.** Commit the changes. Vercel redeploys automatically within a minute.

**Test it:** paste your URL into **opengraph.xyz** or just send it to yourself on WhatsApp. You should see the preview card rather than a bare link.

> If you add a custom domain later, come back and change these to the new domain.

---

# Part 4 — A custom domain (optional)

A `.vercel.app` address works permanently and costs nothing. A real domain looks more trustworthy for something people are meant to take seriously.

**1.** Buy a domain — Namecheap, Cloudflare or Porkbun, roughly $10–15/year. Something like `biblefromscratch.com`.

**2.** In Vercel: your project → **Settings** → **Domains** → enter the domain → **Add**.

**3.** Vercel shows you two DNS records to create. Go to wherever you bought the domain, find DNS settings, and add them exactly as shown.

**4.** Wait. Usually ten minutes, occasionally a few hours. Vercel sets up HTTPS automatically once the records resolve.

**5.** Go back to Part 3 and update the URLs to the new domain.

---

# Part 5 — Accounts (optional, do this last)

Follow `ACCOUNTS-SETUP.md`. It takes about fifteen minutes and needs a free Supabase account.

The one thing worth repeating here: in Supabase under **Authentication → URL Configuration**, the **Site URL** must exactly match your live address. If it says `localhost` the sign-in emails will send people nowhere.

Until you do this, the site works fine — progress saves per device, sharing works, and the account panel explains the situation honestly rather than pretending to be broken.

---

# Making changes later

1. Go to the file on GitHub
2. Click the pencil icon
3. Edit, scroll down, **Commit changes**
4. Vercel redeploys in under a minute

No terminal, no commands. If you'd rather work locally, clone the repo and `git push` — same result.

---

# Things worth knowing

**Staying free.** Hobby is free forever for personal, non-commercial use. It stays free as long as you don't add donations, ads, or payments — Vercel counts a donate button as commercial. If you ever want one, either upgrade to Pro ($20/month) or move the files to Cloudflare Pages, which permits commercial use on its free tier. Moving is a fifteen-minute job; nothing in the code is Vercel-specific.

**The 100GB limit.** Hobby includes 100GB of data transfer per month. Your page is about 275KB including images, so that's roughly 350,000 page views a month. If you exceed a Hobby limit the project pauses rather than charging you, and Hobby can't buy extra capacity — so if you ever get near it, that's the signal to move to Cloudflare.

**Analytics.** Vercel's own Web Analytics is one toggle in project settings, privacy-friendly, and free up to 50,000 events a month on Hobby. Worth switching on just to learn which section people actually read.

**Back it up.** Once it's on GitHub it's backed up. Keep a copy of `index.html` somewhere else too — it's a single file and everything is inside it.
