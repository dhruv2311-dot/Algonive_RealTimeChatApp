# Algonive Real-Time Chat App

Full-stack messaging platform featuring JWT auth, Socket.io real-time transport, MongoDB persistence, Cloudinary uploads, message lifecycle controls, and push notifications.

---

## 1. Project Overview
Algonive_RealTimeChatApp delivers secure 1:1 and group messaging with live typing indicators, attachments, edits/deletes, and browser push alerts. The backend (Node/Express + Socket.io) exposes REST APIs, manages MongoDB state, and brokers websocket events, while the React frontend provides a polished sidebar/chat layout, auth pages, and service worker powered notifications.

## 2. Tech Stack
- **Backend:** Node.js, Express, MongoDB Atlas (Mongoose), Socket.io, JWT, bcrypt, Multer + Cloudinary, Web Push
- **Frontend:** React (Vite), React Router, Axios, Socket.io Client, custom service worker

## 3. Repository Structure
```
/backend
  controllers/        # Auth, room, message, push controllers
  middleware/         # JWT guard + multer-cloudinary uploader
  models/             # User, Room, Message schemas
  routes/             # Modular Express routers
  utils/              # Cloudinary + push helpers
  index.js            # Express + Socket.io bootstrap
  package.json, .env.example
/frontend
  public/service-worker.js
  src/                # React app, API helpers, components, styles
  package.json, .env.example
README.md
```

## 4. Environment Variables
### Backend `.env`
```
MONGO_URI=<mongo atlas connection string>
JWT_SECRET=<secure random string>
CLOUDINARY_CLOUD_NAME=<cloudinary name>
CLOUDINARY_API_KEY=<cloudinary key>
CLOUDINARY_API_SECRET=<cloudinary secret>
VAPID_PUBLIC_KEY=<web push vapid public>
VAPID_PRIVATE_KEY=<web push vapid private>
FRONTEND_URL=http://localhost:3000
PORT=5000
```

### Frontend `.env`
```
VITE_API_URL=http://localhost:5000
VITE_SOCKET_URL=http://localhost:5000
VITE_VAPID_PUBLIC_KEY=<same public vapid key as backend>
```

## 5. Backend Setup & Run
1. `cd backend`
2. `npm install`
3. Copy `.env.example` ➜ `.env` and fill in values
4. Start dev server: `npm run dev` (Nodemon) or `npm start`
5. Ensure MongoDB Atlas IP access is configured and `FRONTEND_URL` matches your React origin for CORS

## 6. Frontend Setup & Run
1. `cd frontend`
2. `npm install`
3. Copy `.env.example` ➜ `.env` and set API/socket URLs + VAPID key
4. Start Vite dev server: `npm run dev`
5. Visit `http://localhost:3000`

## 7. Features
- JWT registration/login with hashed credentials
- Sidebar listing direct/group rooms with creation form
- Real-time room join, live messaging, typing indicators
- Message edit/delete with UI state updates
- Cloudinary-backed file & image uploads
- Persistent message history w/ edited/deleted flags
- Push notification subscriptions saved per user (service worker + Web Push)
- Responsive, stylized UI with auto-scroll and file previews

## 8. Push Notification Setup
1. Generate VAPID keys (e.g., via `npx web-push generate-vapid-keys`)
2. Place public/private keys in backend `.env` and public key in frontend `.env`
3. Ensure site served over HTTPS in production for notifications
4. Browser prompts for permission on login; subscription stored via `/api/push/subscribe`

## 9. Cloudinary Setup
1. Create a Cloudinary account and an unsigned upload preset (or use API key/secret)
2. Populate `CLOUDINARY_*` vars
3. Uploaded files land in `algonive_chat_uploads` folder with secure URLs returned to frontend

## 10. MongoDB Atlas Setup
- Create Cluster ➜ Database user ➜ Obtain SRV URI
- Whitelist your IP / enable 0.0.0.0/0 for dev
- Set `MONGO_URI` accordingly

## 11. Testing With Two Users
1. Start backend (`http://localhost:5000`) and frontend (`http://localhost:3000`)
2. Open two browsers/incognito sessions
3. Register/login distinct accounts
4. Create a direct room (enter the other user’s email) or group room
5. Send messages/attachments; observe live updates, typing indicators, edits/deletes, and push alerts in both sessions

## 12. Deployment Notes
- **Backend (Render/Heroku):**
  - Set all environment variables
  - Enable WebSocket support
  - Update `FRONTEND_URL` to deployed React domain
- **Frontend (Vercel):**
  - Set `VITE_API_URL`/`VITE_SOCKET_URL` to backend HTTPS URL
  - Include `public/service-worker.js` in build output (Vite copies automatically)

## 13. Next-Steps Checklist
1. `cd backend && npm install`
2. Configure backend `.env`
3. Run backend: `npm run dev`
4. `cd ../frontend && npm install`
5. Configure frontend `.env`
6. Run frontend: `npm run dev`
7. Open two browser sessions to verify chat, uploads, edits/deletes, and push notifications
