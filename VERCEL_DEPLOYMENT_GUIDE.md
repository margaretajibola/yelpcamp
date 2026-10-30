# Deploying to Vercel

This app includes an `api/index.js` entry point and a `vercel.json` config so it can run as a Vercel serverless function alongside (or instead of) a traditional host like Render.

## Steps

1. Push this repo to GitHub (or your git provider of choice).
2. In the [Vercel dashboard](https://vercel.com), click **Add New → Project** and import the repo.
3. Leave the Build Command empty — there's no build step; Vercel just installs dependencies and packages `api/index.js` as a serverless function.
4. Add these Environment Variables in the project settings (Settings → Environment Variables):
   - `DB_URL` — your MongoDB connection string
   - `CLOUDINARY_CLOUD_NAME`
   - `CLOUDINARY_KEY`
   - `CLOUDINARY_SECRET`
   - `SESSION_SECRET` — a random string used to sign session cookies
   - `NODE_ENV=production`
5. Click **Deploy**. Vercel will give you a `*.vercel.app` URL once it's live.

## Notes specific to Vercel's serverless environment

- Image uploads go straight to Cloudinary (not local disk), so they work fine in a serverless/read-only filesystem.
- Sessions are stored in MongoDB via `connect-mongo`, not in memory, so they persist correctly across serverless invocations.
- Each cold start re-opens a MongoDB connection; for a low-traffic app this is fine as-is, but if you see connection-limit issues under load, add connection caching across invocations.
