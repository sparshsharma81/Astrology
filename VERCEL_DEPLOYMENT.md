DEPLOYMENT CHECKLIST FOR VERCEL
================================

Pre-Deployment Checklist
------------------------

✅ Code is ready
✅ Dependencies added to package.json (express, node-fetch, body-parser)
✅ vercel.json configured with build command
✅ api/gemini.js ready for Vercel serverless functions
✅ scripts/write-env.js ready to generate env.js
✅ Frontend (index.html, script.js, style.css) uses /api/gemini proxy
✅ .vercelignore created to ignore unnecessary files

Step-by-Step Deployment Guide
------------------------------

1. **Prepare your local environment** (optional for testing):
   ```bash
   npm install
   npm run write-env
   npm run serve
   # Test at http://localhost:3000
   ```

2. **Push to GitHub** (if not already done):
   ```bash
   git add .
   git commit -m "Ready for Vercel deployment"
   git push origin main
   ```

3. **Deploy on Vercel**:
   
   **Option A: Using Vercel Dashboard** (Recommended)
   1. Go to https://vercel.com/dashboard
   2. Click "Add New" → "Project"
   3. Import your GitHub repository
   4. In "Configure Project", make sure:
      - Framework Preset: "Other"
      - Build Command: `node scripts/write-env.js` (should be auto-detected)
      - Output Directory: `.` (root)
      - Root Directory: `.` (if monorepo)
   5. Click "Environment Variables" and add:
      - OPENCAGE_KEY: [your key]
      - ASTRO_KEY_1: [your key]
      - ASTRO_KEY_2: [your key]
      - (add more ASTRO_KEY_* as needed, up to 12)
      - GEMINI_KEY: [your key]
      - MODEL_NAME: gemini-2.0-flash (optional)
   6. Click "Deploy"

   **Option B: Using Vercel CLI**
   ```bash
   npm install -g vercel
   vercel login
   
   # First time setup
   vercel
   
   # Add environment variables
   vercel env add OPENCAGE_KEY
   vercel env add ASTRO_KEY_1
   # ... add more keys
   vercel env add GEMINI_KEY
   
   # Deploy to production
   vercel --prod
   ```

4. **Verify Deployment**:
   - Visit your Vercel domain (e.g., astro-scope.vercel.app)
   - Check that:
     - Homepage loads with hero section visible
     - Frontend displays properly (styling intact)
     - Environment variables are loaded (window.__ENV should have values)
     - Try generating a test chart to verify API calls work

Troubleshooting
---------------

**Issue: "502 Bad Gateway" or error from /api/gemini**
- Check: Is GEMINI_KEY set in Vercel environment variables?
- Check: Did Vercel redeploy after adding the env var?
- Fix: Go to Vercel dashboard → Deployments → Redeploy latest commit

**Issue: "Cannot find env.js"**
- Check: vercel.json has buildCommand set to "node scripts/write-env.js"
- Fix: Redeploy the project to trigger build script

**Issue: Frontend loads but no styling (CSS broken)**
- Check: Vercel served the right index.html and style.css
- Fix: Verify files are at root level (not in a subfolder)
- Fix: Hard refresh browser (Ctrl+Shift+R)

**Issue: OpenCage or Astro API calls fail**
- Check: Are OPENCAGE_KEY and ASTRO_KEY_* set?
- Check: Do the keys have remaining quota?
- Fix: Test keys locally first with: npm run serve

**Issue: Charts not generating**
- Check: All three API keys are set (OpenCage, Astro, Gemini)
- Check: Browser console for errors (F12 → Console tab)
- Fix: Try demo mode first to verify UI works

Environment Variable Reference
-------------------------------

| Variable | Source | Purpose | Visibility |
|----------|--------|---------|------------|
| OPENCAGE_KEY | OpenCage.io | Geolocation/timezone lookup | Written to env.js (public) |
| ASTRO_KEY_1 through ASTRO_KEY_12 | json.freeastrologyapi.com | Birth chart calculations | Written to env.js (public) |
| GEMINI_KEY | Google AI Studio | AI narrative generation | Server-side only (secure) |
| MODEL_NAME | User config | LLM model name | Written to env.js (public) |

How It Works After Deployment
------------------------------

1. User accesses astro-scope.vercel.app
2. Frontend loads index.html + style.css + script.js + env.js
3. User enters birth data (date, time, location)
4. Frontend calls:
   - OpenCage API for timezone (using OPENCAGE_KEY from env.js)
   - Astro API for birth chart data (using ASTRO_KEY_* from env.js)
   - Serverless /api/gemini for narrative generation (no key needed, server uses GEMINI_KEY)
5. Gemini API response is sent to frontend
6. Frontend displays full astrology report

Performance Notes
-----------------

- Vercel CDN caches static assets (html, css, js)
- API calls are rate-limited by their respective services
- First deployment takes ~1-2 minutes
- Subsequent deployments from GitHub are automatic
- Serverless functions cold-start may add 1-2 seconds to first /api/gemini call

Next Steps
----------

1. Deploy now using the steps above
2. Test thoroughly with real birth data
3. Share your Vercel URL with users
4. Monitor Vercel dashboard for errors/performance
5. Add custom domain if desired (Vercel → Settings → Domains)

Questions?
----------

Refer to:
- Vercel Docs: https://vercel.com/docs
- Deployment Guide: README_DEPLOY.md
