# FooTube — Node.js Server

The backend of **FooTube**, a YouTube-style video platform built by a team of three as a Bar-Ilan University Advanced Programming project. This server exposes a REST API for users, videos and comments, stores everything in MongoDB, and serves the production build of the React web client.

<p align="center">
  <img src="https://raw.githubusercontent.com/elayben/FooTube-CPP-Server/main/wiki/Screenshot_11.png" alt="FooTube home feed" width="720">
</p>

## The FooTube project

| Repository | What it is |
|------------|------------|
| **FooTube-Server** (this repo) | Node.js + Express REST API, MongoDB persistence, serves the web build |
| [FooTube-Web](https://github.com/elayben/FooTube-Web) | React web client |
| [FooTube-Android](https://github.com/elayben/FooTube-Android) | Native Android client (Java) |
| [FooTube-CPP-Server](https://github.com/elayben/FooTube-CPP-Server) | Multithreaded C++ TCP server for video recommendations |

## Tech stack

- **Node.js / Express** — routing and middleware
- **MongoDB / Mongoose** — users, videos and comments
- **JWT** (`jsonwebtoken`) — authentication for write operations
- **Multer** — media uploads
- **MVC structure** — `routes/` → `controllers/` → `models/`

## Getting started

**Prerequisites:** Node.js 18+, npm, and MongoDB running locally on the default port (`mongodb://localhost:27017`).

```bash
git clone https://github.com/elayben/FooTube-Server.git
cd FooTube-Server
npm install
npm start
```

The server starts at **http://127.0.0.1:8080** and connects to the `footube` database. Opening that address in a browser loads the bundled web client from `public/`.

## API

Routes marked 🔒 require a JWT, sent as `Authorization: Bearer <token>` (get one from `POST /api/tokens`).

### Users

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/users` | Register a new user |
| `POST` | `/api/tokens` | Log in and receive a JWT |
| `GET` | `/api/users` | List all users |
| `GET` | `/api/users/:id` 🔒 | Get a user |
| `PUT` / `PATCH` | `/api/users/:id` 🔒 | Update a user |
| `DELETE` | `/api/users/:id` 🔒 | Delete a user |

### Videos

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/videos` | Home feed: the 10 most-viewed videos plus 10 random ones |
| `GET` | `/api/users/:id/videos` | Videos uploaded by a user |
| `POST` | `/api/users/:id/videos` 🔒 | Upload a video |
| `GET` | `/api/users/:id/videos/:pid` | Get a video |
| `PUT` / `PATCH` | `/api/users/:id/videos/:pid` 🔒 | Edit a video |
| `DELETE` | `/api/users/:id/videos/:pid` 🔒 | Delete a video |
| `PATCH` | `/api/videos/:pid/views` | Increment view count |
| `PATCH` | `/api/users/:id/videos/:pid/likes` 🔒 | Like a video |
| `PATCH` | `/api/users/:id/videos/:pid/dislikes` 🔒 | Dislike a video |

### Comments

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/videos/:pid/comments` | Comments on a video |
| `POST` | `/api/videos/:pid/comments` 🔒 | Add a comment |
| `GET` | `/api/comments/:cid` | Get a comment |
| `PUT` / `PATCH` | `/api/comments/:cid` 🔒 | Edit a comment |
| `DELETE` | `/api/videos/:pid/comments/:cid` 🔒 | Delete a comment |

## Project structure

```
├── server.js          # App entry point: middleware, routes, Mongo connection
├── routes/            # Express routers (users, videos, comments)
├── controllers/       # Request handlers
├── models/            # Mongoose schemas + JWT auth middleware
└── public/            # Production build of the React web client
```

## Team

Built by a team of three, with work split by feature across web and Android and tracked in Jira. [Elay Ben Yehoshua](https://github.com/elayben) was one of the three developers.
