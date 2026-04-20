# 📝 NoteTaker

> A full-stack note-taking web application built with Node.js, Express, and PostgreSQL

---

## Overview

NoteTaker is a lightweight, RESTful web application that lets users create, edit, and manage personal notes through a clean browser interface. Users log in with a username (no password required), and all notes are stored persistently in a PostgreSQL database. The app also supports inserting real-time weather data into notes using your device's location.

---

## Features

| Feature | Description |
|---|---|
| User Authentication | Login with any username (3–20 chars, alphanumeric + underscore) |
| Auto Registration | New users are created automatically on first login |
| Create Notes | Write and save notes up to 5000 characters |
| Edit Notes | Update any existing note via a modal editor |
| Delete Notes | Remove notes with a confirmation dialog |
| Weather Insertion | Insert current weather data into a note using your location |
| API Key Security | All API routes protected with `x-api-key` header validation |
| PostgreSQL Storage | Notes and users persisted in a relational database |

---

## Tech Stack

- **Node.js + Express.js** — Backend server and REST API
- **PostgreSQL** — Relational database for users and notes
- **pg (node-postgres)** — Database client
- **dotenv** — Environment variable management
- **Vanilla JS + HTML/CSS** — Frontend (no frameworks)
- **Open-Meteo API** — Free weather data (no API key needed)

---

## Prerequisites

Make sure you have the following installed before setting up the project:

- Node.js v18 or higher
- npm
- PostgreSQL 14 or higher

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/NoteTaker.git
cd NoteTaker
```

### 2. Install Dependencies

```bash
npm install
```
### 3. Create the Tables

```bash
sudo -u postgres psql -d note_taker_db -c "
CREATE TABLE IF NOT EXISTS users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(20) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS notes (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
"
```

### 4. Configure Environment Variables

Create a `.env` file in the project root:

```env
DB_USER=postgres
DB_HOST=127.0.0.1
DB_NAME=note_taker_db
DB_PASSWORD=yourpassword
DB_PORT=5432
```

> ⚠️ **Important:** Use `127.0.0.1` instead of `localhost` for `DB_HOST` to avoid IPv6 connection issues with the `pg` library.

### 5. Start the Server

```bash
node server.js
```

The app will be available at **http://localhost:3000**

---

## API Reference

All API routes require the following header:

```
x-api-key: peas-and-carrots
```

| Method | Endpoint | Description | Body |
|---|---|---|---|
| `POST` | `/api/login` | Login or create user | `{ username }` |
| `GET` | `/api/users/:username/notes` | Fetch all notes | — |
| `POST` | `/api/users/:username/notes` | Create a note | `{ content }` |
| `PUT` | `/api/users/:username/notes/:noteId` | Update a note | `{ content }` |
| `DELETE` | `/api/users/:username/notes/:noteId?confirm=true` | Delete a note | — |

---

## Using the App

1. Open **http://localhost:3000** in your browser
2. Enter a username (3–20 characters, letters/numbers/underscores only)
3. If the username doesn't exist, an account is automatically created
4. Create notes, edit them, or delete them using the buttons on each note card
5. Click **Insert Weather** to automatically append current weather conditions to your note

---

## License

This project is for educational purposes. Feel free to use and modify it as needed.
