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
python Server_Side_Flask.py
```

The server starts on `http://127.0.0.1:5000`.

A demo database (`Database/MazeDatabase.db`) is included with pre-seeded users and mazes — no database setup required.

### Email verification

A demo Gmail account is pre-configured in `Server_Side_Flask.py`. New signups receive a verification link automatically. If you want to use your own Gmail account instead:

1. Enable 2FA on your Google account
2. Generate an [App Password](https://support.google.com/accounts/answer/185833) (Mail)
3. Replace the credentials in `Server_Side_Flask.py` near `Gmail.login(...)`:
   ```python
   Gmail.login('your-email@gmail.com', 'your-16-char-app-password')
   ```

You can skip signup entirely and use the pre-seeded demo accounts below.

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
