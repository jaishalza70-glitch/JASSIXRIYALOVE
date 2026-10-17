# JAYSI × RIYA LOVEVERSE

A mobile-first React + Vite + Supabase starter for a private long-distance couple space.

## Included
- Email/password accounts
- Couple profiles: display name, gender selection, avatar URL / upload UI
- Private room code workflow
- Realtime chat (Supabase Realtime)
- Love coins and virtual gift UI
- Custom quiz questions (currently kept in browser session only)
- Tap-to-boost racing mini-game (currently local/pass-the-phone; not online multiplayer)
- Admin panel shell with manually assigned admin flag
- Responsive mobile-first dark neon theme

## Important honest limitations
This is a runnable starter project, not a fully production-hardened service. Do not publish it for strangers or put real money/value into the coin system as-is. The starter UI includes direct coin updates for quick testing; a user can tamper with client-side code. The SQL includes secure `award_coins` and `send_love_gift` RPCs, but the frontend must be switched to those RPCs and profile update RLS must be tightened before public use. Quiz questions currently live only in the current browser session, and racing is a local mini-game, not synced online head-to-head. Chat is realtime when both users have internet and the Supabase project is active; no web app can guarantee “always connected” offline.

## 1. Create Supabase project
1. Go to https://supabase.com and create a project.
2. Open **SQL Editor**, paste `supabase/schema.sql`, and run it.
3. In **Project Settings → API**, copy the project URL and publishable/anon key.
4. In **Authentication → Providers**, enable Email. For easiest private testing, configure email confirmation as you prefer. Keep passwords strong.
5. In **Database → Replication** (or Publications, depending on dashboard UI), enable Realtime for `public.messages`.
6. In **Storage**, create an `avatars` bucket. For a private couple prototype, you can initially use avatar URLs instead; uploaded-image access requires Storage policies appropriate to your privacy model. Do not make private photos public without considering that anyone with the URL may see them.

## 2. Run locally
Install Node.js 20+ from https://nodejs.org/
```bash
cp .env.example .env.local
```
Edit `.env.local`:
```env
VITE_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_PUBLISHABLE_OR_ANON_KEY
```
The publishable/anon key is designed for browser use only when RLS is configured correctly. Never use `service_role` or secret keys in frontend variables.

Then:
```bash
npm install
npm run dev
```
Open the Vite local URL on your phone. To test from a second phone on the same Wi-Fi, Vite prints a LAN address; if it doesn't, run `npm run dev -- --host 0.0.0.0`.

## 3. Make profiles and connect
1. Create your account (Jassi) and sign in.
2. Save your profile.
3. Tap **Create room** and copy the 6-character code.
4. Send the code privately to Riya.
5. Riya creates her own account and enters the code in **Join room**.
6. Both can use the Chat tab. If the partner name doesn't appear immediately, refresh once.

## 4. Set yourself as admin
1. Sign in and open Supabase **Authentication → Users**.
2. Copy your user's UUID.
3. In SQL Editor run:
```sql
update public.profiles set is_admin = true where id = 'YOUR-USER-UUID';
```
4. Refresh the site. Do not add a browser control to grant admin privileges.

## 5. Deploy for mobile phones (Netlify)
1. Push this project to a private GitHub repository.
2. In Netlify, choose **Add new site → Import an existing project** and select the repo.
3. Build command: `npm run build`
4. Publish directory: `dist`
5. Add environment variables in Netlify site settings:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
6. Deploy. After changing environment variables, trigger a new deploy.
7. Open the HTTPS site on iPhone/Android. Use **Add to Home Screen** in the browser for an app-like shortcut.

Alternative hosting: Vercel or Cloudflare Pages can also host the Vite build. Keep the Supabase database in Supabase.

## Before inviting anyone else
- Apply the production hardening described in `supabase/schema.sql`.
- Use server-side RPCs for every coin reward and gift transaction.
- Tighten profile updates so users cannot modify `coins` or `is_admin` from the browser.
- Restrict room codes and profile reads to only the two expected members; add room membership records and server-verified invitations if this grows beyond two trusted accounts.
- Add rate limiting, abuse reporting, message deletion/export controls, and automated tests.
- Do not use this as a money, betting, or real-value currency system.

## Files
- `src/main.jsx` — UI and app logic
- `src/styles.css` — responsive neon theme
- `supabase/schema.sql` — database tables, initial RLS, and example secure coin RPCs
- `.env.example` — environment variable template
