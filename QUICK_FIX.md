# Quick Deployment Reference

## Current Issue
```
GET https://algonive-real-time-chat-app.vercel.app/register 404 (Not Found)
```

## Why This Happens
Your app has TWO parts:
1. **Backend** (API Server) - NOT deployed yet ❌
2. **Frontend** (UI) - Deployed on Vercel ✅

Frontend needs backend to work!

## Quick Fix

### Option 1: Deploy Backend to Render (Recommended - FREE)
1. Go to [render.com](https://render.com)
2. New → Web Service
3. Connect your GitHub repo
4. Settings:
   - Root Directory: `backend`
   - Build: `npm install`
   - Start: `npm start`
5. Add Environment Variables (see DEPLOYMENT_FIX.md)
6. Deploy

### Option 2: Deploy Backend to Railway
1. Go to [railway.app](https://railway.app)
2. New Project → Deploy from GitHub
3. Select `backend` folder
4. Add Environment Variables
5. Deploy

### Option 3: Use Vercel for BOTH (Monorepo)
This requires restructuring - not recommended for beginners.

## After Backend is Deployed

1. Copy backend URL (e.g., `https://your-app.onrender.com`)
2. Go to Vercel → Your Project → Settings → Environment Variables
3. Add:
   ```
   VITE_API_URL=https://your-backend-url.onrender.com
   VITE_SOCKET_URL=https://your-backend-url.onrender.com
   VITE_VAPID_PUBLIC_KEY=your_vapid_public_key
   ```
4. Redeploy frontend

## Test
Visit your Vercel URL and try registering - should work! ✅

## Full Guide
See `DEPLOYMENT_FIX.md` for detailed step-by-step instructions in Hindi.

## Need Help?
Check:
- Backend logs on Render/Railway
- Browser console (F12)
- Network tab for API calls
