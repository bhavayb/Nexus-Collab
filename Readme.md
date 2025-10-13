# Nexus Collab !

Real-time collaborative development platform (MERN) with an AI assistant for in-project help and code generation. This repository contains a Node.js/Express backend and a React + Vite frontend.

## Key features
- Real-time collaboration using Socket.IO
- AI assistant integration via Google Generative AI (Gemini)
- User authentication with JWT
- MongoDB for persistence (Mongoose models for users and projects)
- Redis for caching/session features (ioredis)

## Tech stack
- Backend: Node.js, Express, Socket.IO, Mongoose, Redis
- Frontend: React, Vite
- AI: @google/generative-ai (Gemini model)

## Repository layout

- `/backend` - Express server, routes, controllers, services
- `/frontend` - React app built with Vite

## Prerequisites

- Node.js (v18+ recommended)
- npm or yarn
- MongoDB instance (Atlas or local)
- Redis instance (optional but recommended for caching)
- Google Cloud API key for the Generative AI SDK

## Environment variables

Create a `.env` file in the `backend` folder with the following variables (example):

```
PORT=3000
MONGODB_URI=mongodb+srv://<user>:<pass>@cluster0.mongodb.net/nexus-collab
JWT_SECRET=your_jwt_secret_here
GOOGLE_AI_KEY=your_google_api_key_here
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
REDIS_PASSWORD=your_redis_password_if_any
```

Frontend environment (create `.env` or use Vite env) — put in `frontend/.env`:

```
VITE_API_URL=http://localhost:3000
```

Note: The backend defaults to `PORT` or 3000 when not provided.

## Run the project locally

Open two terminals: one for backend, one for frontend.

Backend

```cmd
cd backend
npm install
REM ensure backend/.env exists with required variables
node server.js
```

If you prefer a start script, you can run:

```cmd
cd backend
npm install
node server.js
```

Frontend

```cmd
cd frontend
npm install
npm run dev
```

The frontend will start with Vite (usually on http://localhost:5173) and connect to the backend via `VITE_API_URL`.

## API overview

- Authentication & users: mounted under `/users`
- Projects: mounted under `/projects`
- AI endpoint: mounted under `/ai`

Example endpoints (adjust host/port as needed):

- POST /users/register - register a new user
- POST /users/login - login and receive JWT
- GET /projects/:id - retrieve project
- POST /ai/generate - (internal AI endpoints used by sockets/backend)

Refer to the code in `backend/routes` for exact route names and request shapes.

## Socket.IO (real-time collaboration)

The frontend connects to the backend Socket.IO server using the `VITE_API_URL` and passes the JWT as auth. In the frontend `src/config/socket.js` the client uses:

```js
socket(import.meta.env.VITE_API_URL, {
    auth: { token: localStorage.getItem('token') },
    query: { projectId }
})
```

Project messages that contain `@ai` are forwarded to the AI service and the generated response is broadcast back into the room as a message from the AI.

## Testing

- Backend: no automated tests included (see `backend/package.json`)
- Frontend: Vite dev server + linting via `npm run lint`

## Development notes & assumptions

- The backend `package.json` in this repo currently doesn't include a `start` script; running `node server.js` is the recommended straightforward way to start the server. You can add a `start` script such as `node server.js` if desired.
- Ensure `MONGODB_URI` and `JWT_SECRET` are set before starting the backend; socket connections verify JWT tokens.
- The AI integration requires a valid `GOOGLE_AI_KEY` environment variable.

## Contributing

1. Fork the repository
2. Create a feature branch (git checkout -b feat/my-feature)
3. Make changes with clear commits
4. Submit a pull request describing the change

## Contact / License

If you need help or want to report an issue, open an issue in this repository.

This project is provided as-is. Add a license file if you want to make licensing explicit.

---
Last updated: 2025-10-12
