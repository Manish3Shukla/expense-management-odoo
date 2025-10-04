# 🚀 Complete Setup Guide - ExpenseFlow

This guide will help you set up and run the Expense Management System from scratch, even if you're new to development.

## 📋 Table of Contents
1. [Prerequisites](#prerequisites)
2. [Quick Start (Docker - Recommended)](#quick-start-docker)
3. [Manual Setup (Without Docker)](#manual-setup)
4. [First Time Usage](#first-time-usage)
5. [Troubleshooting](#troubleshooting)
6. [Common Issues](#common-issues)

---

## Prerequisites

### For Docker Setup (Easiest)
- **Docker Desktop** (includes Docker and Docker Compose)
  - Windows: [Download Docker Desktop](https://www.docker.com/products/docker-desktop)
  - Mac: [Download Docker Desktop](https://www.docker.com/products/docker-desktop)
  - Linux: Install Docker and Docker Compose separately

### For Manual Setup
- **Python 3.11+** - [Download Python](https://www.python.org/downloads/)
- **Node.js 20+** - [Download Node.js](https://nodejs.org/)
- **PostgreSQL 15+** - [Download PostgreSQL](https://www.postgresql.org/download/)
- **Redis** - [Download Redis](https://redis.io/download)

---

## Quick Start (Docker)

### Step 1: Install Docker Desktop

1. Download Docker Desktop for your operating system
2. Install and start Docker Desktop
3. Verify installation:
   ```bash
   docker --version
   docker compose version
   ```

### Step 2: Clone or Download the Project

```bash
# If you have git installed
git clone <repository-url>
cd expense-management-system

# OR download and extract the ZIP file, then navigate to the folder
```

### Step 3: Configure Environment Variables

```bash
# Copy example environment files
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env

# The default values work for Docker setup, no changes needed!
```

### Step 4: Start the Application

```bash
# From the project root directory
docker compose up --build
```

**What happens:**
- ✅ PostgreSQL database starts on port 5432
- ✅ Redis starts on port 6379
- ✅ Backend API starts on port 8000
- ✅ Celery worker starts for background tasks
- ✅ Frontend starts on port 5173

**Wait for these messages:**
```
backend    | INFO:     Application startup complete.
frontend   | VITE v5.x.x  ready in xxx ms
```

### Step 5: Access the Application

Open your browser and go to:
```
http://localhost:5173
```

**🎉 You should see the login page!**

### Step 6: Stop the Application

Press `Ctrl+C` in the terminal, then:
```bash
docker compose down
```

---

## Manual Setup (Without Docker)

### Step 1: Install Prerequisites

#### Install Python 3.11+
```bash
# Verify installation
python --version
# Should show: Python 3.11.x or higher
```

#### Install Node.js 20+
```bash
# Verify installation
node --version
npm --version
```

#### Install PostgreSQL
1. Download and install PostgreSQL
2. Remember your postgres password
3. Create a database:
   ```bash
   # Open psql terminal
   psql -U postgres
   
   # Create database
   CREATE DATABASE expenses;
   
   # Exit
   \q
   ```

#### Install Redis
- **Windows**: Use [Redis for Windows](https://github.com/microsoftarchive/redis/releases)
- **Mac**: `brew install redis`
- **Linux**: `sudo apt-get install redis-server`

### Step 2: Setup Backend

```bash
# Navigate to backend folder
cd backend

# Create virtual environment
python -m venv .venv

# Activate virtual environment
# Windows:
.venv\Scripts\activate
# Mac/Linux:
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Copy and configure environment
cp .env.example .env

# Edit .env file with your database credentials
# DATABASE_URL=postgresql+psycopg2://postgres:YOUR_PASSWORD@localhost:5432/expenses
```

### Step 3: Setup Frontend

```bash
# Open a NEW terminal
cd frontend

# Install dependencies
npm install

# Copy environment file
cp .env.example .env
```

### Step 4: Start Services

You'll need **4 separate terminal windows**:

#### Terminal 1: Start Redis
```bash
# Windows
redis-server

# Mac/Linux
redis-server
```

#### Terminal 2: Start Backend
```bash
cd backend
.venv\Scripts\activate  # Windows
# source .venv/bin/activate  # Mac/Linux

uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

#### Terminal 3: Start Celery Worker
```bash
cd backend
.venv\Scripts\activate  # Windows
# source .venv/bin/activate  # Mac/Linux

celery -A app.celery_worker.celery_app worker --loglevel=INFO
```

#### Terminal 4: Start Frontend
```bash
cd frontend
npm run dev
```

### Step 5: Access the Application

Open your browser:
```
http://localhost:5173
```

---

## First Time Usage

### 1. Register Your Admin Account

1. Click **"Register"** on the login page
2. Fill in the form:
   - **Full Name**: Your name
   - **Company Name**: Your company name
   - **Country**: Select your country (currency auto-fills)
   - **Email**: Your email address
   - **Password**: Choose a strong password
3. Click **"Create Account"**

**What happens:**
- ✅ Your admin account is created
- ✅ A company is created with your selected currency
- ✅ You're automatically logged in

### 2. Explore the Dashboard

After login, you'll see:
- **Dashboard**: Overview of expenses and stats
- **My Expenses**: Submit and track your expenses
- **Approvals**: Review pending approvals (Manager/Admin)
- **All Expenses**: Company-wide expense view (Manager/Admin)
- **Admin Panel**: Manage users and approval rules (Admin only)

### 3. Create Your First User (Admin Only)

1. Go to **Admin** → **Users** tab
2. Click **"+ Add User"**
3. Fill in:
   - Email
   - Password
   - Full Name
   - Role (Employee/Manager/Admin)
   - Manager (optional)
4. Click **"Create User"**

### 4. Set Up Approval Rules (Admin Only)

1. Go to **Admin** → **Approval Rules** tab
2. Click **"+ Add Rule"**
3. Configure:
   - **Rule Name**: e.g., "Standard Approval"
   - **Rule Type**:
     - **Percentage**: Auto-approve when X% approve
     - **Specific Approver**: Auto-approve when specific person approves
     - **Hybrid**: Combine both
   - **Approval Percentage**: e.g., 60%
   - **Manager Approval**: Check if manager must approve first
4. Click **"Create Rule"**

### 5. Submit an Expense (Any User)

1. Go to **My Expenses**
2. Click **"+ New Expense"**
3. Fill in:
   - Amount
   - Currency
   - Category
   - Date
   - Description
4. Click **"Submit Expense"**

**What happens:**
- ✅ Expense is created
- ✅ Approval workflow is triggered
- ✅ Approvers receive the request

### 6. Approve/Reject Expenses (Manager/Admin)

1. Go to **Approvals**
2. Click on a pending expense
3. Review details
4. Add optional comment
5. Click **"Approve"** or **"Reject"**

---

## Troubleshooting

### Docker Issues

#### "Docker daemon is not running"
**Solution:**
1. Start Docker Desktop application
2. Wait for Docker to fully start (whale icon in system tray)
3. Try again

#### "Port already in use"
**Solution:**
```bash
# Stop all containers
docker compose down

# Check what's using the port
# Windows:
netstat -ano | findstr :8000
netstat -ano | findstr :5173

# Mac/Linux:
lsof -i :8000
lsof -i :5173

# Kill the process or change ports in docker-compose.yml
```

#### "Cannot connect to database"
**Solution:**
```bash
# Restart all services
docker compose down
docker compose up --build
```

### Manual Setup Issues

#### "Module not found" (Python)
**Solution:**
```bash
# Make sure virtual environment is activated
# Windows:
.venv\Scripts\activate

# Reinstall dependencies
pip install -r requirements.txt
```

#### "Cannot find module" (Node.js)
**Solution:**
```bash
cd frontend
rm -rf node_modules package-lock.json
npm install
```

#### "Connection refused" to database
**Solution:**
1. Check PostgreSQL is running
2. Verify credentials in `backend/.env`
3. Test connection:
   ```bash
   psql -U postgres -d expenses
   ```

#### "Redis connection failed"
**Solution:**
1. Start Redis server
2. Verify it's running:
   ```bash
   redis-cli ping
   # Should return: PONG
   ```

---

## Common Issues

### Frontend shows blank page
1. Check browser console for errors (F12)
2. Verify backend is running: http://localhost:8000/health
3. Clear browser cache and reload

### Login not working
1. Check backend logs for errors
2. Verify database is running
3. Try registering a new account

### Expenses not appearing
1. Refresh the page
2. Check browser console for errors
3. Verify you're logged in as the correct user

### Approval workflow not working
1. Ensure approval rules are created (Admin panel)
2. Verify users have managers assigned
3. Check backend logs for errors

### Theme not switching
1. Clear browser cache
2. Check browser console for errors
3. Refresh the page

---

## Development Tips

### View Backend API Documentation
```
http://localhost:8000/docs
```

### View Database
```bash
# Using Docker
docker exec -it expense-management-system-db-1 psql -U postgres -d expenses

# Manual setup
psql -U postgres -d expenses
```

### View Logs

#### Docker
```bash
# All services
docker compose logs -f

# Specific service
docker compose logs -f backend
docker compose logs -f frontend
```

#### Manual
Check the terminal windows where services are running

### Reset Everything

#### Docker
```bash
docker compose down -v  # Removes volumes (deletes data)
docker compose up --build
```

#### Manual
```bash
# Drop and recreate database
psql -U postgres
DROP DATABASE expenses;
CREATE DATABASE expenses;
\q
```

---

## Next Steps

1. ✅ **Customize**: Update company details, add more users
2. ✅ **Configure**: Set up approval rules that match your workflow
3. ✅ **Test**: Submit test expenses and try the approval flow
4. ✅ **Explore**: Check out all features in each section
5. ✅ **Extend**: Add custom categories, integrate OCR, etc.

---

## Need Help?

- Check the main [README.md](README.md) for feature documentation
- Review [FEATURES.md](FEATURES.md) for complete feature list
- Check backend API docs at http://localhost:8000/docs
- Look for error messages in terminal/console

---

## Security Notes

⚠️ **Before deploying to production:**

1. Change `JWT_SECRET` in `backend/.env`
2. Use strong database passwords
3. Enable HTTPS/SSL
4. Configure proper CORS origins
5. Set up proper backup strategy
6. Enable rate limiting
7. Review security best practices

---

**Congratulations! You're all set up! 🎉**

Start by registering your admin account and exploring the features.
