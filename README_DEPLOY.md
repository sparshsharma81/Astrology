Deploying to Vercel
===================

This application supports full deployment to Vercel with both static frontend and serverless backend API.

Quick Start (Recommended)
-------------------------

1. **Install Vercel CLI** (optional but helpful):
   ```bash
   npm install -g vercel
   ```

2. **Deploy directly from GitHub**:
   - Go to https://vercel.com/new
   - Select this GitHub repository
   - Configure environment variables (see below)
   - Click "Deploy"

Environment Variables on Vercel
--------------------------------

In your Vercel project settings, add these environment variables:

| Variable | Value | Notes |
|----------|-------|-------|
| `OPENCAGE_KEY` | Your OpenCage API key | For geolocation/timezone lookup |
| `ASTRO_KEY_1` through `ASTRO_KEY_12` | Your Astro API keys | Used for birth chart calculations |
| `GEMINI_KEY` | Your Google Gemini API key | Used by `/api/gemini` serverless function |
| `MODEL_NAME` | `gemini-2.0-flash` (or your preferred model) | Optional; defaults to `gemini-2.0-flash` |

How to Add Environment Variables:
1. Go to your Vercel project dashboard
2. Click "Settings" → "Environment Variables"
3. Add each variable and its value
4. Click "Save"
5. Redeploy (or trigger a new deployment from GitHub)

Build and Deployment Process
------------------------------

When you deploy, Vercel automatically:

1. **Builds the environment file**: Runs `node scripts/write-env.js` which:
   - Reads environment variables you set in Vercel
   - Generates `env.js` containing public-safe configuration
   - Makes keys available to the frontend as `window.__ENV`

2. **Deploys serverless API**: 
   - Deploys `/api/gemini.js` as a serverless Node function
   - Handles requests to `/api/gemini` (proxies to Google Generative API)
   - Keeps `GEMINI_KEY` server-side secure

3. **Serves static frontend**:
   - Serves all `.html`, `.css`, `.js` files
   - Routes all unmatched requests to `index.html` (SPA routing)
   - CDN caches immutable assets

How the Frontend Uses Environment Variables
---------------------------------------------

The frontend loads `env.js` which exposes `window.__ENV`:

```javascript
// env.js is auto-generated at build time and contains:
window.__ENV = {
  opencageKey: "...",
  astroKey1: "...",
  astroKey2: "...",
  // ... up to astroKey12
  geminiKey: "", // kept empty for server proxy use
  modelName: "gemini-2.0-flash"
}
```

The application checks:
- If `window.__ENV.geminiKey` is present and not empty, it calls Google Generative API directly
- Otherwise, it calls `/api/gemini` (server proxy) which uses the server-side `GEMINI_KEY`

Security Recommendations
------------------------

✅ **What goes in `env.js` (public)**:
- OpenCage API key (typically has rate limits but safe to expose)
- Astro API keys (typically safe to expose if rate-limited)
- Model name

❌ **What stays server-side only**:
- `GEMINI_KEY` in `api/gemini.js` – Use the serverless proxy, NOT client-side key

Local Development
-----------------

Before deploying, test locally:

1. **Create a `.env` file** (already exists):
   ```env
   OPENCAGE_KEY=your_key
   ASTRO_KEY_1=your_key
   GEMINI_KEY=your_key
   MODEL_NAME=gemini-2.0-flash
   ```

2. **Generate env.js**:
   ```bash
   npm run write-env
   ```

3. **Start local server**:
   ```bash
   npm run serve
   ```
   Opens http://localhost:3000

4. **Or run with Express (simulates server proxy)**:
   ```bash
   npm run start-server
   ```

Vercel Deployment Configuration
--------------------------------

The `vercel.json` file is pre-configured and contains:

- **buildCommand**: `node scripts/write-env.js` – Generates `env.js` from environment variables
- **builds**: Specifies `api/**/*.js` as Node serverless functions
- **routes**: 
  - `/api/*` → forwards to serverless API
  - `/*` → serves static files; unmatched → `index.html` (SPA routing)

Vercel CLI Deployment
---------------------

If you prefer using the CLI:

```bash
# Link to a Vercel project (first time)
vercel

# Deploy (automatically picks up environment variables from Vercel project)
vercel --prod

# Or set environment variables locally for testing
vercel env pull
npm run write-env
vercel --prod
```

Troubleshooting
---------------

**Issue**: Environment variables not showing up in the frontend
**Solution**: Ensure `vercel.json` has `buildCommand: "node scripts/write-env.js"` and redeploy

**Issue**: `/api/gemini` returns 500 error
**Solution**: Check that `GEMINI_KEY` is set in Vercel environment variables

**Issue**: Frontend can't connect to OpenCage
**Solution**: Verify `OPENCAGE_KEY` is set and rate limit hasn't been exceeded

**Issue**: Astrology charts not generating
**Solution**: Ensure at least one `ASTRO_KEY_*` is set in Vercel environment

Custom Domain
-----------

To use a custom domain:

1. In Vercel dashboard, go to "Settings" → "Domains"
2. Add your domain
3. Update DNS records according to Vercel instructions
4. Vercel automatically provisions SSL/TLS

More Information
---------------

- Vercel Docs: https://vercel.com/docs
- Environment Variables: https://vercel.com/docs/projects/environment-variables
- Serverless Functions: https://vercel.com/docs/functions/serverless-functions

