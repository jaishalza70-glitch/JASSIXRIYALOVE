# Deploy JAYSI × RIYA LOVEVERSE

This project is configured for Netlify, but a live backend requires your own Supabase project.

## Required once before deployment
1. Create a Supabase project at https://supabase.com.
2. In Supabase SQL Editor, run `supabase/schema.sql`.
3. Enable Realtime for `public.messages` in Database/Replication.
4. In Supabase Project Settings → API, copy the Project URL and publishable/anon key.
5. Push this folder to a private GitHub repository.
6. In Netlify, import that repository. `netlify.toml` sets the build command and publish folder automatically.
7. In Netlify Site configuration → Environment variables, set:
   - `VITE_SUPABASE_URL` = your Supabase Project URL
   - `VITE_SUPABASE_ANON_KEY` = your Supabase publishable/anon key
8. Deploy the site. If you add/change environment variables later, trigger a new deploy.

## Couple setup
- Each person signs up with their own email/password.
- Save both profiles.
- One person creates a room and shares the code privately.
- The other person joins using that code.

## Admin
After creating your own account, copy its user UUID from Supabase Authentication → Users.
Run in SQL Editor:
```sql
update public.profiles set is_admin = true where id = 'YOUR-USER-UUID';
```
Refresh the site. Never grant admin access from a public client UI.

## Important
Do not put a Supabase `service_role` or secret key in Netlify frontend environment variables.
This is a starter app. Before public use, harden profile update policies and switch all coin/gift
changes to the secure database RPC functions described in `supabase/schema.sql`.
The race is currently local/pass-the-phone, and custom quiz questions are session-only.
