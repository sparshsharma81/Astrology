🚀 QUICK START - DEPLOY IN 5 MINUTES
═════════════════════════════════════════════════════════════════

✅ STEP 1: Commit & Push
────────────────────────────────────────────────────────────────
Run these in your terminal:

  git add .
  git commit -m "Vercel deployment ready"
  git push

(This uploads all the configuration changes to GitHub)


✅ STEP 2: Go to Vercel
────────────────────────────────────────────────────────────────
Visit: https://vercel.com/dashboard

Click: "Add New" → "Project"


✅ STEP 3: Connect GitHub
────────────────────────────────────────────────────────────────
Select: Your GitHub repository (sparshsharma81/Astrology)

Click: "Import"


✅ STEP 4: Configure Build
────────────────────────────────────────────────────────────────
Vercel will auto-detect:
  • Framework: Other
  • Build Command: node scripts/write-env.js
  • Output Directory: .

Just verify it's correct and continue.


✅ STEP 5: Add Environment Variables
────────────────────────────────────────────────────────────────
Click: "Environment Variables" section

Add these (copy from VERCEL_ENV_KEYS.txt):
  
  Name: OPENCAGE_KEY
  Value: 4334b95ef0c645e79092cc2c3739083c
  
  Name: ASTRO_KEY_1
  Value: dpyZOtWAnT1yYO8p2tfKP4GcyBiIIFivaQ6PFQJ4
  
  ... (add all 12 ASTRO keys - see VERCEL_ENV_KEYS.txt)
  
  Name: GEMINI_KEY
  Value: AIzaSyCl5o1mjs0o27tRD8_h0HCwLgdjdF34bBQ
  
  Name: MODEL_NAME
  Value: gemini-2.0-flash (optional)

For each variable:
  • Enter Name and Value
  • Select: Production, Preview, Development
  • Click "Add Environment Variable"


✅ STEP 6: Deploy
────────────────────────────────────────────────────────────────
Click: "Deploy"

Wait 1-2 minutes...


✅ STEP 7: You're Live! 🎉
────────────────────────────────────────────────────────────────
Vercel will give you a URL like:
  https://astrology-xxxx.vercel.app

Click it and your site is live!

Add custom domain later if you want.


WHAT TO TEST:
═════════════════════════════════════════════════════════════════

After deployment, try:
  1. Open the site
  2. See the hero section with styling intact
  3. Enter birth data (date/time/location)
  4. Generate a chart
  5. See the Gemini-powered narrative

If anything doesn't work, see the troubleshooting in:
  → VERCEL_DEPLOYMENT.md


COMMON MISTAKES TO AVOID:
═════════════════════════════════════════════════════════════════

❌ Don't forget Step 5 (Environment Variables)
   Without them, the site won't work!

❌ Don't put keys in GitHub
   They should ONLY be in Vercel environment variables

❌ Don't skip the GEMINI_KEY
   Needed for AI narratives

❌ Don't forget to add at least one ASTRO_KEY_*
   Needed for birth chart generation


NEED HELP?
═════════════════════════════════════════════════════════════════

1. Check build logs in Vercel dashboard
2. Check browser console (F12 → Console)
3. Read VERCEL_DEPLOYMENT.md for troubleshooting
4. Check that environment variables are actually set


DONE! 🚀
═════════════════════════════════════════════════════════════════

Your astrology app is now live on Vercel!

Share your URL with users and they can generate birth charts
with AI-powered narratives.

