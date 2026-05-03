# RMS Cloud Backend — Self Hosting Guide

This is the backend server that powers RMS Cloud Sync. It allows users to back up and restore their RMS settings across devices using their Discord account.

---

## Stack

- **Node.js** + **Fastify**
- **PostgreSQL** database
- **Discord OAuth2** authentication
- **TypeScript**

---

## Requirements

- Node.js 18 or later
- PostgreSQL 14 or later
- A Discord application (for OAuth2)
- A domain or server to host on

---

## Setup

### 1. Clone the repo

```bash
git clone https://github.com/zxkuhl/RMS.git
cd RMS/Backend
```

### 2. Install dependencies

```bash
npm install
```

### 3. Create a Discord application

1. Go to [discord.com/developers/applications](https://discord.com/developers/applications)
2. Click **New Application**
3. Go to **OAuth2** → add a redirect URL: `https://yourdomain.com/auth/callback`
4. Copy your **Client ID** and **Client Secret**

### 4. Set up PostgreSQL

Create a database and user:

```sql
CREATE DATABASE rms;
CREATE USER rms_user WITH PASSWORD 'yourpassword';
GRANT ALL PRIVILEGES ON DATABASE rms TO rms_user;
```

### 5. Configure environment

Create a `.env` file in the `Backend` folder:

```env
PORT=8080

# PostgreSQL
DATABASE_URL=postgresql://rms_user:yourpassword@localhost:5432/rms

# Discord OAuth2
DISCORD_CLIENT_ID=your_client_id
DISCORD_CLIENT_SECRET=your_client_secret
DISCORD_REDIRECT_URI=https://yourdomain.com/auth/callback

# Secret key for signing tokens (make this long and random)
JWT_SECRET=your_super_secret_key_here

# Your domain
BASE_URL=https://yourdomain.com
```

### 6. Run database migrations

```bash
npm run migrate
```

### 7. Build and start

```bash
npm run build
npm start
```

For development with auto-reload:

```bash
npm run dev
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/auth/discord` | Start Discord OAuth2 login |
| GET | `/auth/callback` | OAuth2 callback |
| GET | `/settings` | Get user settings (auth required) |
| PUT | `/settings` | Upload user settings (auth required) |
| DELETE | `/settings` | Delete user settings (auth required) |

---

## Connecting RMS to your backend

In RMS Settings → Cloud, change the URL from `https://api.rms.dev/` to your own server URL, then click **Authenticate** to log in with Discord.

---

## Hosting Options

| Platform | Notes |
|----------|-------|
| **Railway** | Easy, free tier available, supports PostgreSQL |
| **Render** | Free tier, good for small projects |
| **DigitalOcean** | VPS, full control, ~$6/mo |
| **Fly.io** | Free tier, good performance |
| **Self-hosted VPS** | Full control, cheapest long term |

---

## Security Notes

- Keep your `.env` file private, never commit it to GitHub
- Use HTTPS in production
- Use a strong `JWT_SECRET` — at least 32 random characters
- Consider rate limiting the settings endpoints

---

## Troubleshooting

**Can't connect to database**
Make sure PostgreSQL is running and the `DATABASE_URL` is correct.

**Discord OAuth not working**
Make sure your redirect URI in the Discord developer portal exactly matches `DISCORD_REDIRECT_URI` in your `.env`.

**RMS can't reach your server**
Make sure your server is accessible from the internet and your domain is pointing to it. Check your firewall allows port 80/443.

---

## License

Copyright © 2026 zxkuhl. All rights reserved.
