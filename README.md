# MERN Social Media App

A full-stack social media platform with real-time notifications, live chat, and media uploads. Built with the MERN stack and Socket.io. Deployed as a **single Render service** — the Express backend serves both the API and the built React frontend.

---

## Features

- Register / login with JWT auth stored in an httpOnly cookie (XSS protected)
- Create posts and reels with image/video uploads via Cloudinary CDN
- Like and comment on posts
- **Real-time notifications** — when someone likes or comments on your post, the notification appears instantly without any page refresh (Socket.io room-based events per user)
- **Live chat** — real-time messaging between users; message history persists in MongoDB and loads on refresh
- Follow / unfollow users
- Profile photo and name updates
- Search users

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React (Vite), Tailwind CSS |
| Backend | Node.js, Express |
| Database | MongoDB Atlas |
| Real-time | Socket.io |
| Media storage | Cloudinary + Multer |
| Auth | JWT (httpOnly cookie) |
| Deployment | Render (single service) |

## Architecture

The Express backend serves the built React frontend as static files from `/frontend/dist`. This means **one Render web service handles everything** — the REST API, WebSocket connections, and the React app itself.

Socket.io uses a singleton instance in `socket/socket.js` shared across all Express routes. Each connected user joins a room keyed to their user ID; likes, comments, and messages emit events directly to the relevant room.

## Local Setup

**Prerequisites:** Node.js 18+, MongoDB Atlas account, Cloudinary account

```bash
git clone https://github.com/khichar-monika15/mern-social-media.git
cd mern-social-media

# Backend
cd backend
npm install
# Fill in backend/.env with your credentials (see backend/.env for template)
node index.js    # runs on http://localhost:7000

# Frontend (new terminal)
cd frontend
npm install
npm run dev      # runs on http://localhost:5173
```

## Deployment (Render — single service)

1. Connect GitHub repo to Render as a new Web Service
2. **Build command:** `npm install && cd frontend && npm install && npm run build && cd ..`
3. **Start command:** `node backend/index.js`
4. Set environment variables in the Render dashboard:

| Variable | Description |
|---|---|
| `MONGO_URL` | MongoDB Atlas connection string |
| `JWT_SEC` | Random secret for signing JWTs |
| `Cloudinary_Cloud_Name` | Cloudinary cloud name |
| `Cloudinary_Api` | Cloudinary API key |
| `Cloudinary_Secret` | Cloudinary API secret |
| `SELF_URL` | Your Render service URL (used for keep-alive ping) |

See `backend/.env` for the full template.
