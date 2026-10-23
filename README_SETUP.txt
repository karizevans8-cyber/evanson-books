# Evanson Kariuki Books — one-package setup

## What's included
- `index.html`: responsive book website with catalogue search/filter, Supabase email sign-up/login, password reset, reader library, progress-saving interface, and a publisher metadata form.
- `setup.sql`: initial tables, reader RLS policies, and publisher write policies.
- This guide.

## Important: not every feature is production-ready
This is a starter implementation. It does not yet process payments, upload PDF files from the dashboard, or display/stream PDF content in the reading modal. The modal currently shows book description and progress controls. A secure checkout and server-side payment verification are required before selling/unlocking paid books. Do not store private PDFs in a public bucket.

## Setup
1. Open `index.html` in GitHub and replace the entire file contents with this file's contents, then commit.
2. In the script near the bottom of `index.html`, replace:
   - `PASTE_YOUR_SUPABASE_PROJECT_URL_HERE`
   - `PASTE_YOUR_SUPABASE_PUBLISHABLE_KEY_HERE`
   with the Project URL and publishable key from Supabase Project Settings → API. Never use the `service_role` or secret key in browser code.
3. Replace `REPLACE_WITH_YOUR_PUBLISHER_EMAIL` in `PUBLISHER_EMAILS` with the exact email you will use to sign in as publisher.
4. In `setup.sql`, replace `publisher@example.com` with that same exact email.
5. Run `setup.sql` in Supabase → SQL Editor. If you already have policies with the same names, duplicate-policy notices are harmless; review any other errors.
6. Confirm the `books` table has the existing published book row. For a free title use `price=0`, `is_free=true`, `is_published=true`, and a public `cover_url`.
7. In Supabase Authentication → URL Configuration, set the Site URL to `https://karizevans8-cyber.github.io/evanson-books/` and add that URL to Redirect URLs.
8. Open the live website in a fresh tab. Create a test reader account using an email you can access. If email confirmation is enabled, confirm it from your inbox, then log in.
9. Test adding a free book to My Library. If a database permission error appears, copy only the error message; never share passwords or secret keys.

## Existing table/policy caution
The SQL is designed to preserve data and uses `create table if not exists`. If your existing database has differently configured policies, inspect them rather than deleting tables or turning off RLS. The app's front-end publisher email check is for convenience only; database RLS is the actual access control.

## GitHub Pages
GitHub Pages serves a static website. This app uses Supabase's browser client and database policies. A production paid-book service should use a trusted backend/edge function to verify payment and issue time-limited access to private files.

## Launch checklist before public release
- Complete privacy policy and terms of service.
- Add a real reader for supported files (PDF/ePub) and implement server-authorized access to private book files.
- Add payment provider and server-side verification; never trust a client-side "paid" status.
- Secure publisher uploads with Storage RLS policies and restrict uploads to the publisher.
- Test email confirmation, password reset, mobile layout, RLS permissions, and error handling.
- Add backups and a support contact.
