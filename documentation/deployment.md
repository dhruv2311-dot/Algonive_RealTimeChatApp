# Deployment Guide

## Prerequisites
- Node.js 18+
- MongoDB Atlas URI
- Cloudinary credentials
- VAPID key pair for push notifications
- Separate terminal sessions for backend and frontend

## Backend Deployment
1. Copy .env.example to .env and fill values.
2. Install dependencies:
   `ash
   cd backend
   npm install
   `
3. Build assets (if any) and start production server with your process manager, e.g.:
   `ash
   npm run start
   `
4. Ensure environment exposes:
   - PORT
   - FRONTEND_URL
   - MONGO_URI
   - JWT_SECRET
   - CLOUDINARY_*
   - VAPID_PUBLIC_KEY / VAPID_PRIVATE_KEY
5. Set up reverse proxy (Nginx/Apache) to forward HTTPS traffic to the Node process.

## Frontend Deployment
1. Copy .env.example to .env and set:
   - VITE_API_URL pointing to backend HTTPS URL
   - VITE_SOCKET_URL pointing to backend socket origin
   - VITE_VAPID_PUBLIC_KEY
2. Install dependencies:
   `ash
   cd frontend
   npm install
   `
3. Create production build:
   `ash
   npm run build
   `
4. Serve dist/ via any static host:
   - Vercel / Netlify (drag-and-drop dist)
   - Nginx with oot /var/www/algonive/dist;
   - AWS S3 + CloudFront (upload dist contents)
5. Verify service worker and push notifications using HTTPS domain.

## End-to-End Checklist
-  Backend reachable at https://api.example.com
-  Frontend served at https://chat.example.com
-  CORS FRONTEND_URL matches production frontend origin
-  Service worker registered (check DevTools > Application)
-  Push subscription saved (monitor backend logs)
-  Socket events flow between multiple clients
