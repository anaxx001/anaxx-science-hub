ANAXX SCIENCE HUB v2 — PHONE-FRIENDLY SETUP

WHAT IS INCLUDED
- index.html: public visitor website (only published articles are shown)
- admin.html: admin sign-in, article editor, draft/publish actions, phone image upload, logo/profile photo branding controls
- config.js: your Supabase project URL and publishable/anon key placeholders
- setup.sql: database tables, admin allow-list, row-level security and image bucket rules

IMPORTANT
This is a starter implementation. It must be connected to your own Supabase project and tested before you rely on it. Do not add a Supabase service_role/secret key to config.js or any HTML file. Do not upload sensitive images: the article-images bucket is public by design so published article images can load for visitors. Draft article TEXT is protected by database policies, but an uploaded image's direct URL can be viewed by anyone who obtains it.

STEP A — CREATE A SUPABASE PROJECT
1. Visit https://supabase.com/ and create/sign in to an account.
2. Create a project named anaxx-science-hub. Choose a strong database password and keep it private.
3. Wait for the project to finish provisioning.
4. Open SQL Editor, create a new query, paste all of setup.sql, and run it.

STEP B — CREATE YOUR ADMIN ACCOUNT
1. In Supabase, open Authentication > Users.
2. Add a user with your email and a strong password (wording may vary; use the create/add user option).
3. Confirm the account if Supabase asks you to.
4. Copy the user's UUID/UID from the user details.
5. Return to SQL Editor and run this query, replacing the example UUID:
   insert into public.admin_users (user_id) values ('YOUR_AUTH_USER_UUID');
   Keep that UUID private enough to avoid confusion, but it is not a password.

STEP C — CONNECT THE WEBSITE
1. In Supabase, open Project Settings > API (or Data API / API Keys; labels can vary).
2. Copy the Project URL.
3. Copy the publishable key, or legacy anon key if that is what the dashboard offers.
4. Open config.js in your GitHub repository and replace the two placeholder strings:
   window.SUPABASE_URL = "https://YOUR-PROJECT.supabase.co";
   window.SUPABASE_PUBLISHABLE_KEY = "YOUR_PUBLISHABLE_OR_ANON_KEY";
5. Commit the change. The publishable/anon key is designed for browser use only when RLS is configured correctly. NEVER use a service_role or secret key in the website.

STEP D — UPLOAD THESE FILES TO GITHUB
1. Open your repository: https://github.com/anaxx001/anaxx-science-hub
2. Upload/replace index.html and add admin.html, config.js. Keep them together in the repository root.
3. Do NOT upload setup.sql publicly if you prefer to keep your setup notes private; it is not a password, but you can keep it locally instead.
4. Commit the changes and wait for GitHub Pages to deploy.

STEP E — TEST
1. Public site: https://anaxx001.github.io/anaxx-science-hub/
2. Admin page: https://anaxx001.github.io/anaxx-science-hub/admin.html
3. In the admin dashboard, use Website branding to upload your logo and profile photo; then tap Save branding.
3. Sign in with the approved admin email/password.
4. Create an article and save as draft. Check the public site: it should NOT appear.
5. Open the admin dashboard, edit it and press Publish. Refresh the public site: it should appear.
6. Test an image upload using a non-sensitive image you have permission to publish.

SUPABASE FREE PLAN NOTES (verify current quotas in Supabase dashboard/docs)
- Current published pricing/docs list a Free plan with 500 MB database per project, 1 GB file storage, 5 GB egress and 50,000 monthly active users. Free projects may pause after one week of inactivity.
- Free storage file limit can be configured up to 50 MB; this starter app limits uploads to 5 MB.
- Keep images compressed and delete unused images manually from Storage if needed. Storage object cleanup is not automatic when an article is deleted.
- GitHub Pages is static hosting; Supabase provides authentication/database/storage.

CONTENT NOTES
- This editor uses plain text with line breaks rather than a rich text editor.
- Verify anatomy/medical content against trusted references, cite sources in each article, and only upload images you own or are licensed to use.
- Do not publish identifiable patient images or other private material.
- Take a backup of your articles periodically.

SECURITY NOTES
- Database RLS policies restrict drafts and article edits to user IDs explicitly listed in admin_users.
- Do not add public sign-up UI. Create admin accounts in Supabase yourself.
- If you later allow other authors, change the admin model intentionally rather than weakening the policies.
- The public image bucket means uploaded image URLs are public. For strictly private draft images, a more advanced private-bucket/signed-URL workflow is needed.
