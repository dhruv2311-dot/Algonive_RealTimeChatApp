# 🔧 404 Error Fix - Deployment Guide

## समस्या क्या है?

आपको यह error मिल रहा है:
```
GET https://algonive-real-time-chat-app.vercel.app/register 404 (Not Found)
```

### कारण
आपकी application में **दो अलग-अलग parts** हैं:
1. **Backend** (Express API) - `/api/auth/register` endpoint provide करता है
2. **Frontend** (React App) - User Interface provide करता है

**समस्या**: आपने केवल **frontend** को Vercel पर deploy किया है, लेकिन **backend** deploy नहीं किया है। Frontend backend API को call करने की कोशिश कर रहा है, लेकिन backend server चल नहीं रहा।

---

## ✅ समाधान (Step by Step)

### Step 1: Backend को Deploy करें (Render.com पर - FREE)

#### 1.1. Render Account बनाएं
- [Render.com](https://render.com) पर जाएं और Sign Up करें (GitHub से login करें)

#### 1.2. Environment Variables तैयार करें
नीचे दिए गए values replace करें:

```env
# MongoDB Connection
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/algonive_chat

# JWT Secret (कोई भी random string)
JWT_SECRET=your_super_secret_random_string_123456

# Frontend URL (आपका Vercel URL)
FRONTEND_URL=https://algonive-real-time-chat-app.vercel.app

# Cloudinary (अपने Cloudinary Dashboard से लें)
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# VAPID Keys (नीचे command से generate करें)
VAPID_PUBLIC_KEY=your_generated_public_key
VAPID_PRIVATE_KEY=your_generated_private_key
```

**VAPID Keys Generate करने के लिए:**
```bash
cd backend
npx web-push generate-vapid-keys
```

#### 1.3. Backend Deploy करें

1. Render Dashboard में जाएं
2. **"New +"** → **"Web Service"** पर click करें
3. अपनी GitHub repository connect करें
4. निम्नलिखित settings configure करें:

   **Basic Settings:**
   - **Name**: `algonive-chat-backend`
   - **Region**: अपने closest region को चुनें
   - **Branch**: `main` (या आपकी default branch)
   - **Root Directory**: `backend`
   - **Runtime**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`

5. **Environment Variables** add करें (ऊपर दिए गए सभी variables)

6. **"Create Web Service"** पर click करें

7. Deploy होने का wait करें (5-10 minutes)

8. Deploy होने के बाद, आपको backend URL मिलेगा:
   ```
   https://algonive-chat-backend.onrender.com
   ```
   **इस URL को copy करें!**

---

### Step 2: Frontend को Update करें (Vercel पर)

#### 2.1. Vercel में Environment Variables Set करें

1. [Vercel Dashboard](https://vercel.com/dashboard) पर जाएं
2. अपनी project पर click करें
3. **Settings** → **Environment Variables** पर जाएं
4. निम्नलिखित variables add करें:

   **Name**: `VITE_API_URL`  
   **Value**: `https://algonive-chat-backend.onrender.com`  
   (Step 1 में मिला backend URL use करें)

   **Name**: `VITE_SOCKET_URL`  
   **Value**: `https://algonive-chat-backend.onrender.com`  
   (Same backend URL)

   **Name**: `VITE_VAPID_PUBLIC_KEY`  
   **Value**: `<आपकी VAPID public key>`  
   (Step 1.2 में generate की थी)

5. सभी environments select करें (Production, Preview, Development)

#### 2.2. Frontend को Redeploy करें

1. Vercel Dashboard में **"Deployments"** tab पर जाएं
2. Latest deployment के तीन dots (...) पर click करें
3. **"Redeploy"** select करें
4. Deploy होने का wait करें

---

### Step 3: Test करें

1. अपना Vercel URL open करें:
   ```
   https://algonive-real-time-chat-app.vercel.app
   ```

2. Register करने की कोशिश करें

3. अगर सब ठीक है तो:
   - ✅ 404 error नहीं आएगा
   - ✅ Registration successful होगा
   - ✅ Login work करेगा

---

## 🔍 Additional Troubleshooting

### अगर फिर भी error आए तो:

#### 1. Browser Console Check करें
- F12 press करें → Console tab
- क्या API URL सही है? Should be: `https://your-backend-url.onrender.com/api/auth/register`

#### 2. Backend Logs Check करें
- Render Dashboard → आपकी service → "Logs" tab
- देखें कि backend properly start हो रहा है

#### 3. CORS Error आए तो
Backend में `FRONTEND_URL` environment variable check करें - यह exactly आपका Vercel URL होना चाहिए (trailing slash के बिना)

#### 4. MongoDB Connection Error
- MongoDB Atlas में IP Whitelist check करें
- "Network Access" में `0.0.0.0/0` add करें (या Render के IP)

---

## 📋 Quick Checklist

- [ ] Backend Render पर deploy हो गया
- [ ] Backend successfully start हो रहा है (Logs में देखें)
- [ ] Frontend Vercel पर है
- [ ] Vercel में `VITE_API_URL` set है
- [ ] Vercel में `VITE_SOCKET_URL` set है
- [ ] Vercel में `VITE_VAPID_PUBLIC_KEY` set है
- [ ] Frontend redeploy कर दिया
- [ ] Browser cache clear किया (Ctrl + Shift + Del)
- [ ] Test registration successful

---

## 💡 Pro Tips

1. **Free Tier पर Render sleep में जाता है** (30 mins inactivity के बाद)
   - पहली request 30-60 seconds ले सकती है
   - Consider paid plan या keep-alive service use करें

2. **Local Testing के लिए:**
   ```bash
   # Terminal 1 (Backend)
   cd backend
   npm install
   npm run dev

   # Terminal 2 (Frontend)
   cd frontend
   npm install
   npm run dev
   ```
   Local frontend: `http://localhost:5173`  
   Local backend: `http://localhost:5000`

3. **Environment Variables update करने के बाद हमेशा redeploy करें**

---

## 🆘 अगर फिर भी problem हो

1. Browser DevTools → Network tab open करें
2. Failed request पर right-click → Copy as cURL
3. Error message screenshot लें
4. Backend logs का screenshot लें
5. मुझे share करें, मैं help करूंगा!

---

**Good Luck! 🚀**
