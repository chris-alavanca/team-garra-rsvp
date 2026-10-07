# Team Garra MMA — ¿Quién viene?

RSVP and monthly-fee tracker for Team Garra MMA (Duke Gym, Costa Adeje). Single `index.html`, vanilla JS, supabase-js from CDN. Spanish UI.

## How it works
- Sign-in with Supabase Auth by email (link now; 6-digit code once custom SMTP is set up).
- Members RSVP to the next 5 sessions; the past week is read-only. Live updates via Supabase Realtime.
- Monthly fee: a member taps "Ya he pagado" (claimed); the coach confirms it or marks it paid. Status only, no amounts. The app never handles money.
- Coach view: members by status for the current/previous month, and confirmed payments per month (12 months).

## Data and security
- Tables: `profiles` (display name, role), `rsvps` (date, user), `payments` (user, month, status). Row-level security on all.
- A member sees only their own payment status; the coach sees all. The coach role is set directly in SQL, never by sign-up:
  `update public.profiles set role = 'coach' where id = '<user uuid>';`
- Retention (pg_cron, nightly): RSVPs after 4 weeks, payment statuses after 12 months. Deleting an auth user cascades to all their data.

## Hosting
Vercel (static) + Supabase free tier (eu-west-1). Free Supabase pauses after 7 days without activity.
