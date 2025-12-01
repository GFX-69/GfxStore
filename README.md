# GfxStore - Minecraft Content Marketplace

A comprehensive marketplace platform for Minecraft content including plugins, mods, texture packs, maps, shaders, server setups, and Bedrock Edition files. Built with Next.js 15, TypeScript, Prisma, and SQLite.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation (A-Z)](#installation-a-z)
- [Configuration](#configuration)
- [Running Locally](#running-locally)
- [Deployment](#deployment)
- [Database](#database)
- [Troubleshooting](#troubleshooting)

## Features

✅ Full Minecraft content marketplace  
✅ User authentication with NextAuth.js  
✅ Content upload and management  
✅ Real-time chat with Socket.io  
✅ Role-based access control  
✅ Admin dashboard with analytics  
✅ Dark/Light theme support  
✅ Responsive design  
✅ Payment integration (Nagad/bKash)

## Tech Stack

**Frontend:** Next.js 15 | TypeScript 5 | React 18 | Tailwind CSS 4 | shadcn/ui  
**Backend:** Next.js API Routes | Prisma ORM | NextAuth.js | Socket.io  
**Database:** SQLite (Replit) / PostgreSQL (Production)  
**Storage:** Local files (Development) / S3 compatible (Production)

## Prerequisites

- **Node.js v18+** ([Download](https://nodejs.org/))
- **npm v8+** (comes with Node.js)
- **Git** ([Download](https://git-scm.com/))
- A code editor (VS Code recommended)

## Installation (A-Z)

### Step 1️⃣ Clone the Repository

```bash
# Clone the project
git clone <your-repo-url>
cd GfxStore
```

### Step 2️⃣ Install All Dependencies

```bash
# Install npm packages
npm install
```

**Expected time:** 2-5 minutes  
**Expected output:** "added X packages, and audited Y packages in Z seconds"

### Step 3️⃣ Create Environment Variables File

```bash
# Create .env.local file
touch .env.local
```

Add this to `.env.local`:

```env
# Database URL (SQLite for development)
DATABASE_URL="file:./gfxstore.db"

# NextAuth Configuration
NEXTAUTH_URL="http://localhost:5000"
NEXTAUTH_SECRET="your-secret-key-min-32-chars-long"
```

**Generate a strong NEXTAUTH_SECRET:**

On macOS/Linux:
```bash
openssl rand -base64 32
```

On Windows (PowerShell):
```powershell
[System.Convert]::ToBase64String([System.Security.Cryptography.RandomNumberGenerator]::GetBytes(32))
```

### Step 4️⃣ Initialize the Database

```bash
# Create database schema
npm run prisma db push
```

**When prompted:** Press `y` to create the database

```bash
# Seed initial data (categories, admin user)
npx tsx prisma/seed.ts
```

**✅ Admin user created!**
- Email: `admin@gfxstore.com`
- Password: `admin123`
- Role: ADMIN

### Step 5️⃣ Build the Project

```bash
# Build for production
npm run build
```

**Expected output:** "Compiled successfully"

### Step 6️⃣ Start the Development Server

```bash
# Start with hot reload
npm run dev
```

**Access your app:**
- 🌐 Main site: http://localhost:5000
- 🔐 Admin panel: http://localhost:5000/admin
- 📊 Login: admin@gfxstore.com / admin123

---

## Configuration

### Environment Variables

Create `.env.local` with:

```env
# Required - Database
DATABASE_URL="file:./gfxstore.db"

# Required - Authentication
NEXTAUTH_URL="http://localhost:5000"
NEXTAUTH_SECRET="your-secret-key-here"

# Optional - Payment Methods
NAGAD_API_KEY="your-nagad-key"
BKASH_API_KEY="your-bkash-key"

# Optional - File Storage
FILEBASE_API_KEY="your-filebase-key"
AWS_REGION="us-east-1"
```

### Using PostgreSQL Instead of SQLite

1. **Update `.env.local`:**
```env
DATABASE_URL="postgresql://username:password@localhost:5432/gfxstore"
```

2. **Push schema:**
```bash
npm run prisma db push
```

3. **Reseed data:**
```bash
npx tsx prisma/seed.ts
```

---

## Running Locally

### Development Mode (With Hot Reload)

```bash
npm run dev
```

Visit: http://localhost:5000

### Production Mode

```bash
# Build first
npm run build

# Start production server
npm start
```

### Stop the Server

```bash
# Press Ctrl+C in terminal
```

---

## Deployment

### 🟦 Replit Deployment (Already Configured)

Your project is ready to run on Replit!

1. ✅ Push code to GitHub
2. ✅ Connect repo to Replit
3. ✅ Click "Run" button
4. ✅ App runs automatically on Replit's domain

No additional setup needed!

### 🖥️ VPS/Self-Hosted Deployment

#### Prerequisites on VPS

```bash
# SSH into your VPS
ssh root@your-vps-ip

# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js (Ubuntu/Debian)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs git

# Verify installation
node --version
npm --version
```

#### Clone and Setup

```bash
# Clone repository
git clone https://github.com/GFX-69/GfxStore.git
cd GfxStore

# Install dependencies
npm install

# Create .env file
nano .env.local
```

Paste your environment variables. To save:
- Press `Ctrl+X`
- Press `Y`
- Press `Enter`

```bash
# Initialize database
npm run prisma db push
npx tsx prisma/seed.ts

# Build for production
npm run build
```

#### Run with PM2 (Recommended)

```bash
# Install PM2 globally
sudo npm install -g pm2

# Start app
pm2 start npm --name "gfxstore" -- start

# Auto-start on reboot
pm2 startup
pm2 save

# Check status
pm2 status

# View logs
pm2 logs gfxstore
```

#### Setup Nginx Reverse Proxy

```bash
# Install Nginx
sudo apt install -y nginx

# Create config
sudo nano /etc/nginx/sites-available/gfxstore
```

Paste this:

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

```bash
# Enable site
sudo ln -s /etc/nginx/sites-available/gfxstore /etc/nginx/sites-enabled/

# Test config
sudo nginx -t

# Restart
sudo systemctl restart nginx
```

#### Add SSL Certificate (Let's Encrypt)

```bash
# Install Certbot
sudo apt install -y certbot python3-certbot-nginx

# Get certificate
sudo certbot --nginx -d your-domain.com

# Auto-renewal enabled automatically
```

---

## Database Management

### Open Database UI

```bash
npx prisma studio
```

Opens browser interface to view/edit data directly.

### Database Commands

```bash
# Sync schema to database
npm run prisma db push

# Generate Prisma client
npx prisma generate

# Create migration
npx prisma migrate dev --name migration_name

# Reset database (WARNING: Deletes all data)
npx prisma db push --force-reset
```

### Backup Database

```bash
# SQLite backup
cp gfxstore.db gfxstore.db.backup

# For PostgreSQL, use pg_dump
pg_dump -U username database_name > backup.sql
```

---

## Common Tasks

### Create New User

1. Visit: http://localhost:5000/auth/signup
2. Fill in email, username, password
3. User created with USER role

### Upgrade User to Admin

1. Go to admin panel: http://localhost:5000/admin
2. Go to "Users" tab
3. Find user, click edit
4. Change role to "ADMIN"
5. Save

### Upload Content

1. Login with Creator account
2. Go to: http://localhost:5000/upload
3. Fill form and upload file
4. Wait for admin approval

### Configure Payment Methods

1. Admin panel → "Settings"
2. Enter Nagad/bKash numbers
3. Save

---

## Troubleshooting

### Error: "Port 5000 already in use"

```bash
# Find process using port
lsof -i :5000

# Kill process
kill -9 <PID>
```

### Error: "Engine is not yet connected"

```bash
# Reinstall dependencies
rm -rf node_modules
npm install

# Restart server
npm run dev
```

### Database connection failed

```bash
# Check if database exists
ls gfxstore.db

# Reinitialize
npm run prisma db push
npx tsx prisma/seed.ts
```

### Build fails

```bash
# Clear cache
rm -rf .next

# Rebuild
npm run build
```

### Login not working

1. Clear browser cookies
2. Verify admin user exists: `npx prisma studio`
3. Check .env.local has NEXTAUTH_SECRET
4. Restart server: `npm run dev`

### Still having issues?

```bash
# Check logs
npm run dev    # Shows real-time logs

# Try full restart
rm -rf node_modules
npm install
npm run build
npm run dev
```

---

## Project Structure

```
GfxStore/
├── src/
│   ├── app/                 # Pages and layouts
│   ├── app/api/             # API endpoints
│   ├── components/          # React components
│   ├── lib/                 # Utilities
│   └── styles/              # CSS
├── prisma/
│   ├── schema.prisma        # Database schema
│   └── seed.ts              # Initial data
├── public/                  # Static files
├── .env.local              # Environment (create this)
├── next.config.js          # Next.js config
└── package.json            # Dependencies
```

---

## Command Reference

| Command | Purpose |
|---------|---------|
| `npm install` | Install dependencies |
| `npm run dev` | Start dev server |
| `npm run build` | Build for production |
| `npm start` | Start production server |
| `npm run prisma db push` | Sync database schema |
| `npx prisma studio` | Open database UI |
| `npx tsx prisma/seed.ts` | Seed initial data |
| `npm run lint` | Check code quality |

---

## Support

**Common questions:**

Q: Can I use this on a VPS?  
A: Yes! All setup commands work on any Linux/Ubuntu server with Node.js installed.

Q: Can I switch from SQLite to PostgreSQL?  
A: Yes! Just update DATABASE_URL in .env.local and run `npm run prisma db push`

Q: How do I update the app after deployment?  
A: Pull latest code, install deps, rebuild, and restart: `git pull && npm install && npm run build && npm start`

Q: Where are files stored?  
A: Development uses local storage. Production can use S3/Filebase via environment variables.

---

## Quick Start Commands (Copy & Paste)

```bash
# Complete setup from scratch
git clone https://github.com/GFX-69/GfxStore.git
cd GfxStore
unzip gfxstore.zip -y
npm install
echo 'DATABASE_URL="file:./gfxstore.db"
NEXTAUTH_URL="http://localhost:5000"
NEXTAUTH_SECRET="super-secret-key-min-32-chars"' > .env.local
npm run prisma db push
npx tsx prisma/seed.ts
npm run build
npm run dev
```

Then visit: **http://localhost:5000**  
Login with: **admin@gfxstore.com / admin123**

---

**Happy marketplace building! 🚀**

Last Updated: November 30, 2025
# GfxStore
