# Switching on accounts

The site works right now without accounts. Progress saves on the device, and every share link works. Accounts add one thing: **the same progress on every device someone owns.**

That needs a server, so it can't come from the HTML file alone. This guide gets it running in about fifteen minutes. Free tier is fine — it will comfortably handle thousands of readers.

---

## Why Supabase, and why no passwords

Supabase gives you authentication and a database behind one signup. The sign-in method wired into the site is a **magic link**: the reader types their email, gets a one-time link, clicks it, and they're in.

That's deliberate. Passwords mean you are storing something people reuse across their bank and their email. Magic links mean there is no password to leak, no reset flow to build, and nothing sensitive in your database. For a project like this it's the right trade — fewer moving parts and less liability.

---

## Step 1 — Create the project

1. Go to **supabase.com**, sign up, and create a new project.
2. Pick a region near most of your readers.
3. Wait for it to finish provisioning (a minute or two).

## Step 2 — Create the table

Open **SQL Editor** in the left sidebar, paste this in, and run it:

```sql
create table progress (
  user_id    uuid references auth.users on delete cascade not null,
  key        text not null,
  value      text,
  updated_at timestamptz default now(),
  primary key (user_id, key)
);

alter table progress enable row level security;

create policy "own rows only"
  on progress for all
  using (auth.uid() = user_id)
  with check (auth.uid() = user_id);
```

**Don't skip the last two statements.** Row Level Security is what stops one reader from seeing another's data. Without those policies the table is readable by anyone with your public key. With them, the database itself enforces that people only ever touch their own rows — not your JavaScript, which can be bypassed.

## Step 3 — Get your keys

Go to **Project Settings → API** and copy two values:

- **Project URL** — looks like `https://abcdefgh.supabase.co`
- **anon public key** — a long string starting `eyJ...`

The anon key is *designed* to be published in a web page. It is not a secret. The `service_role` key on the same page **is** a secret — never put that in the HTML file.

## Step 4 — Paste them in

Open `bible-from-scratch.html`, scroll to near the bottom, and find:

```js
var BFS_CONFIG = {
  SUPABASE_URL: "",
  SUPABASE_ANON_KEY: ""
};
```

Fill both in, save, upload. That's it — the account button now offers sign-in instead of an explanation.

## Step 5 — Point the email links at your site

In Supabase, go to **Authentication → URL Configuration** and set **Site URL** to your live address (e.g. `https://biblefromscratch.com`). Add it under **Redirect URLs** too.

If you skip this, the magic-link email will send people to `localhost` and the link will appear broken.

---

## What happens on first sign-in

Someone who has already ticked twenty books on their phone, then signs in, keeps those twenty. The code merges local progress into the account rather than overwriting it — nobody loses work by creating an account, which is the main reason people refuse to create one.

Progress is also still written to the device as a fallback, so a network failure mid-session doesn't lose ticks.

## Testing it

1. Sign in on your laptop, tick a few books.
2. Open the site on your phone, sign in with the same email.
3. The same books should be ticked.

If they aren't, open the browser console. The two usual causes are a missing RLS policy (permission errors) and a Site URL that doesn't match (the link signs you in on the wrong domain).

---

## Costs

Free tier covers 50,000 monthly active users and 500MB of database. Each reader's progress is roughly 1KB, so storage is a non-issue. If this ever gets big enough to outgrow the free tier, that's a good problem.

## What accounts don't do yet

The current build syncs progress only. If you want notes, highlights, reading groups or public profiles, the table above is the right shape to extend — add a `notes` table with the same `user_id` pattern and the same two policies. Tell me and I'll build it.
