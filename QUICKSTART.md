# ⚡ Quick Start Guide

Get ExpenseFlow running in **5 minutes**!

## Option 1: Docker (Easiest) 🐳

### Requirements
- Docker Desktop installed

### Steps

```bash
# 1. Get the code
git clone <repo-url>
cd expense-management-system

# 2. Copy environment files
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env

# 3. Start everything
docker compose up --build

# 4. Open browser
# http://localhost:5173
```

**That's it!** Register your account and start using ExpenseFlow.

### Optional: Load Demo Data

```bash
docker compose exec backend python seed.py
```

Login with:
- `admin@demo.com` / `admin123`
- `manager1@demo.com` / `manager123`
- `employee1@demo.com` / `employee123`

---

## Option 2: Manual Setup 💻

### Requirements
- Python 3.11+
- Node.js 20+
- PostgreSQL 15+
- Redis

### Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv .venv
.venv\Scripts\activate  # Windows
# source .venv/bin/activate  # Mac/Linux

# Install dependencies
pip install -r requirements.txt

# Setup environment
cp .env.example .env
# Edit .env with your database credentials

# Start backend
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### Frontend Setup (New Terminal)

```bash
cd frontend

# Install dependencies
npm install

# Setup environment
cp .env.example .env

# Start frontend
npm run dev
```

### Start Services (Separate Terminals)

```bash
# Terminal 1: Redis
redis-server

# Terminal 2: Backend (see above)

# Terminal 3: Celery
cd backend
.venv\Scripts\activate
celery -A app.celery_worker.celery_app worker --loglevel=INFO

# Terminal 4: Frontend (see above)
```

---

## First Steps After Setup

### 1. Register Admin Account
1. Go to http://localhost:5173
2. Click "Register"
3. Fill in your details
4. Select your country (currency auto-fills)
5. Create account

### 2. Explore Features

**Dashboard** - View expense statistics and recent activity

**My Expenses** - Submit and track your expenses
- Click "+ New Expense"
- Fill in amount, category, description
- Submit for approval

**Admin Panel** (Admin only)
- Create users (employees, managers)
- Set up approval rules
- Configure workflows

**Approvals** (Manager/Admin)
- Review pending expenses
- Approve or reject with comments

**All Expenses** (Manager/Admin)
- View company-wide expenses
- Filter and search
- Export data (coming soon)

### 3. Set Up Your Workflow

1. **Create Users** (Admin → Users → + Add User)
   - Add managers
   - Add employees
   - Assign manager relationships

2. **Create Approval Rules** (Admin → Approval Rules → + Add Rule)
   - **Percentage**: Auto-approve when X% approve
   - **Specific Approver**: Auto-approve when key person approves
   - **Hybrid**: Combine both rules

3. **Submit Test Expense** (My Expenses → + New Expense)
   - Watch the approval workflow in action!

---

## Common Commands

### Docker

```bash
# Start
docker compose up

# Start in background
docker compose up -d

# Stop
docker compose down

# View logs
docker compose logs -f

# Rebuild
docker compose up --build

# Reset everything
docker compose down -v
docker compose up --build
```

### Manual

```bash
# Backend
cd backend
.venv\Scripts\activate
uvicorn app.main:app --reload

# Frontend
cd frontend
npm run dev

# Database reset
psql -U postgres
DROP DATABASE expenses;
CREATE DATABASE expenses;
\q
```

---

## Troubleshooting

### "Port already in use"
```bash
# Stop Docker containers
docker compose down

# Or change ports in docker-compose.yml
```

### "Cannot connect to database"
```bash
# Restart Docker
docker compose down
docker compose up
```

### "Module not found"
```bash
# Backend
pip install -r requirements.txt

# Frontend
npm install
```

### Frontend shows blank page
1. Check browser console (F12)
2. Verify backend is running: http://localhost:8000/health
3. Clear cache and reload

---

## Next Steps

✅ Read the full [README.md](README.md) for all features

✅ Check [SETUP_GUIDE.md](SETUP_GUIDE.md) for detailed instructions

✅ Review [FEATURES.md](FEATURES.md) for complete feature list

✅ Explore API docs at http://localhost:8000/docs

---

## Need Help?

- Check the [SETUP_GUIDE.md](SETUP_GUIDE.md) for detailed troubleshooting
- Review existing GitHub issues
- Open a new issue with details

---

**Happy expense managing! 🎉**
