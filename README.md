# FLASH NEWS

React + TypeScript + Tailwind frontend on Supabase (Postgres, Auth, Storage, RLS).

## Setup
1. `npm install`
2. `cp .env.example .env` and set `VITE_SUPABASE_URL` and `VITE_SUPABASE_PUBLISHABLE_KEY` (a `.env` for project `flash-news` is already included; the publishable key is safe in the browser).
3. `npm run dev` / `npm run build` / `npm test`

## Database
The schema, triggers and RLS policies were applied to the Supabase project `flash-news` as migrations `001_core_schema`, `002_security_workflow_rls` and `003_tags_and_requests` and `004_media_earnings_settings` and `005_revenue_share`. To keep them in the repo run: `supabase link --project-ref iylogcoocjovsixoxgph && supabase db pull`.
Never put the service-role key in this project.

## Auth setup (Supabase dashboard)
- Authentication > Providers > Email: enable "Confirm email".
- Authentication > URL Configuration: set Site URL and add your deployed URL to Redirect URLs (needed for verification and password reset).

## First admin
Sign up in the app, verify the email, then in the SQL editor run:
`update public.profiles set role='admin' where id = (select id from auth.users where email='YOU@EXAMPLE.COM');`

## Publisher onboarding
Reader signs up > /become-a-publisher > editor/admin approves in /admin (via `decide_application`) > publisher writes at /publisher/articles/new > editor reviews at /editor.

## Routes
/, /category/:slug, /news/:slug, /search, /sign-in, /become-a-publisher, /publishers/:slug, /publisher/dashboard, /publisher/articles(/new|/:id/edit), /editor, /admin

## Added in v0.2
Tags (publishers can create them; /tag/:slug pages), moderated comments with reporting, saved stories page, notifications page with unread count, cookie notice, correction and withdrawal requests (reach editors in Admin > Reports), comment moderation, sitemap generator (`SITE_URL=https://yourdomain npm run sitemap`; also runs before `npm run build` when SITE_URL is set).

## Added in v0.3
Media library (/publisher/media: pictures up to 10 MB, MP4/WebM videos up to 50 MB), pick library files inside the story editor, video shown on article pages (YouTube, Vimeo or library video), publisher earnings (admin sets a rate per 1,000 views; publishers see estimates and request payouts; admins pay manually and mark paid), admin Settings (earnings rate, WhatsApp group link, homepage advertisement banner labelled "Advertisement"), once-per-session view counting.

## Added in v0.4
Revenue sharing: admin records real advertising/sponsor income (Admin > Revenue), picks the percent for publishers (split by views of their published stories in the period) and editors (split by stories approved in the period), then presses "Share with team". Credits appear in each person's Earnings (editors under Editor > My earnings) alongside the per-view rate. Daily view counts are stored to make period splits possible.

## Not built yet
Rich-text editor (plain text with ## and > markers), server-rendered Open Graph tags (SPA sets them client-side only), email notifications and newsletter sending, news provider/RSS import (needs a server-side function and provider credentials), weather widget, Playwright end-to-end tests.

## Deploy
Vercel: import the repo, set the two env vars, build `npm run build`, output `dist` (`vercel.json` handles SPA routing).
