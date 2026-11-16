# Mazenta — Server-Side Maze App

Flask REST API backend for the Mazenta maze platform. Handles email-verified user accounts, maze storage, social features (likes/comments), and solve-time tracking with concurrent-write protection.

> This is the **backend**. The desktop GUI lives at [Client-Side Maze App](https://github.com/Trexicurity07/Client_Side_Maze_App).

## What it does

- **Email-verified signup** — sends a time-limited (30 min) confirmation link via Gmail SMTP
- **Session tokens** — stateless per-request authentication
- **Maze storage** — paginated search/filter by name, algorithm, size, or creator
- **Social layer** — likes, comments, per-user activity feeds
- **Solve-time tracking** — records best times with timestamp ordering to handle concurrent writes

## Stack

| | |
|---|---|
| Framework | Python 3 / Flask |
| Database | SQLAlchemy / SQLite |
| Email | Gmail SMTP (app password) |

## Setup

**Requirements:** Python 3.10+

```bash
git clone https://github.com/Trexicurity07/Server_Side_Maze_App.git
cd Server_Side_Maze_App
pip install -r requirements.txt
```

### 1. Create a `.env` file

Copy `.env.example` to `.env` and fill in your Gmail credentials:

```
GMAIL_ADDRESS=your-email@gmail.com
GMAIL_APP_PASSWORD=xxxx xxxx xxxx xxxx
```

**Getting a Gmail App Password:**
1. Enable 2-Step Verification on your Google account (required)
2. Go to [Google Account → Security → App passwords](https://support.google.com/accounts/answer/185833)
3. Create a new app password — select app: Mail, device: Other
4. Copy the generated 16-character password into `GMAIL_APP_PASSWORD`

> This is needed because Gmail blocks plain password login from scripts. The app password is specific to this use — it does not give access to your full account.

### 2. Run

```bash
python Server_Side_Flask.py
```

The server starts on `http://127.0.0.1:5000`. A fresh SQLite database is created automatically at `Database/MazeDatabase.db` on first run.

### Skipping email verification

A demo database (`Database/MazeDatabase.db`) is included with pre-seeded accounts and mazes — no registration required. Use the demo credentials below to log straight in.

## Demo accounts

| Username | Password |
|---|---|
| alicechen | pass1234 |
| bencarter | pass1234 |
| claraosei | pass1234 |

## API reference

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/Signup` | — | Register — sends email verification link |
| GET | `/EmailAuth/<token>` | — | Confirm email and create account |
| POST | `/Login` | — | Returns `{ UserName, Token }` |
| DELETE | `/Logout/<token>` | token | Invalidates token |
| POST | `/Filter/<token>` | token | Paginated maze search |
| POST | `/CreateRecord/<token>` | token | Save a maze, like, or comment |
| DELETE | `/Unlike/<token>` | token | Remove a like |
| POST | `/FetchComments/<token>` | token | Fetch comments by maze or user |
| POST | `/UserProgress/<token>` | token | Fetch a user's solve history |
| POST | `/AddProgress/<token>` | token | Record a solve time |
