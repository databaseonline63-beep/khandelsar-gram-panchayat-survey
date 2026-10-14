# खंडेलसर ग्राम पंचायत सर्वे — Live deployment starter

This is a deployable Cloudflare Worker + D1 starter. It is **not live until you deploy it** and configure secrets/database.

## Included
- Hindi candidate names and bilingual site title.
- Shared server-side vote totals and public chart refresh (15 seconds).
- Admin login, Admin-assisted vote batches, and combined chart.
- Public vote limit based on IP hash. Raw IP is not stored in the database.
- Admin-assisted votes are counted in the same totals and also disclosed as a separate aggregate.

## Important limitations
- IP-based blocking is not a reliable proof of one human = one vote. People sharing Wi-Fi may block each other; changing networks can bypass the limit. Do not represent this as a verified election.
- Admin-assisted votes can materially alter the chart. Use only for a clearly labelled informal poll, with transparent reporting.
- This starter does not record which individual voted for which candidate; avoid collecting identity-to-choice data.
- Admin password and secrets must be configured privately. Do not put secrets in public HTML or share them in chat.
- Consider privacy notice, consent, abuse controls, and applicable election/legal requirements before public launch.

## Deploy (requires a Cloudflare account)
1. Install Node.js and Wrangler: `npm install -g wrangler`
2. Sign in: `wrangler login`
3. From this folder, create D1 database: `wrangler d1 create khandelsar-survey`
4. Copy the returned database ID into `wrangler.toml` replacing `REPLACE_WITH_YOUR_D1_DATABASE_ID`.
5. Set secrets:
   - `wrangler secret put ADMIN_PASSWORD` (choose a strong unique password)
   - `wrangler secret put ADMIN_SECRET` (use a long random secret, 32+ bytes)
   - `wrangler secret put VOTER_HASH_SECRET` (use a different long random secret)
6. Deploy: `wrangler deploy`
7. The command will show your public `workers.dev` URL. Share that URL on WhatsApp.

The database tables/index are initialized by the Worker on first request. Protect the Admin password and secrets. For large Admin batches the platform's batch statement limit may apply; enter reasonable batches (e.g. 100–500) if necessary.
