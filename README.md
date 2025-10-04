# 💰 ExpenseFlow - Smart Expense Management System

A production-ready, full-stack expense management and approval workflow system with **FastAPI**, **PostgreSQL**, **Celery/Redis**, and **React + Vite + TypeScript**.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Node 20+](https://img.shields.io/badge/node-20+-green.svg)](https://nodejs.org/)
[![Docker](https://img.shields.io/badge/docker-ready-blue.svg)](https://www.docker.com/)

> **Perfect for**: Companies looking to streamline expense management with automated approval workflows, multi-currency support, and role-based access control.

## ✨ Features

### 🔐 Authentication & User Management
- **Self-service registration** with automatic company creation
- **Role-based access control** (Admin, Manager, Employee)
- **JWT-based authentication** with secure token handling
- **Country/currency selection** during signup (auto-fetched from REST Countries API)
- **Manager relationships** for approval workflows

### 💰 Expense Management
- **Submit expenses** with amount, currency, category, description, date
- **Multi-currency support** with automatic conversion
- **Expense categorization** (Meal, Travel, Office, Equipment, Entertainment, Other)
- **OCR receipt scanning** (stub implementation, ready for Tesseract integration)
- **Expense history** with approval status tracking
- **Real-time status updates** (draft, submitted, pending_approval, approved, rejected)

### ✅ Approval Workflow Engine
- **Configurable multi-level approval flows**
  - Manager approval (optional first step)
  - Sequential approver chains (Step 1 → Step 2 → Step 3...)
- **Conditional approval rules**
  - **Percentage-based**: Auto-approve when X% of approvers approve (e.g., 60%)
  - **Specific approver**: Auto-approve when a specific person (e.g., CFO) approves
  - **Hybrid**: Combine both rules (60% OR CFO approval)
- **Approval step tracking** with comments
- **Automatic workflow advancement** after each decision

### 👨‍💼 Admin Panel
- **User management**: Create employees, managers, assign roles
- **Manager assignment**: Define reporting relationships
- **Approval rule builder**: Configure percentage, specific approver, or hybrid rules
- **Company settings**: View and manage company details

### 📊 Reporting & Analytics
- **Dashboard** with expense statistics
- **All expenses view** for managers/admins
- **Filter by status** (pending, approved, rejected)
- **Search functionality** across employees and categories
- **Real-time approval queue** for managers

### 🎨 Modern UI/UX
- **Canva-inspired light mode** with vibrant purple gradients
- **GitHub-inspired dark mode** with professional aesthetics
- **One-click theme toggle** with persistent preference
- **Interactive dashboards** with real-time data
- **Responsive layout** for mobile, tablet, and desktop
- **Smooth animations** and transitions
- **Intuitive navigation** with role-based menus
- **Visual approval workflow** tracking
- **Custom scrollbars** and polished details

## 🚀 Quick Start (Docker - Recommended)

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop) installed and running

### Setup in 3 Steps

```bash
# 1. Clone the repository
git clone <repository-url>
cd expense-management-system

# 2. Copy environment files (use defaults for Docker)
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env

# 3. Start everything with Docker
docker compose up --build
```

**Wait for startup messages:**
```
✅ backend    | INFO: Application startup complete.
✅ frontend   | VITE ready in xxx ms
```

### Access the Application

| Service | URL | Description |
|---------|-----|-------------|
| **Frontend** | http://localhost:5173 | Main application UI |
| **Backend API** | http://localhost:8000 | REST API |
| **API Docs** | http://localhost:8000/docs | Interactive Swagger UI |
| **ReDoc** | http://localhost:8000/redoc | Alternative API docs |

### First-Time Setup

1. **Open** http://localhost:5173
2. **Click** "Register" (top right)
3. **Fill in**:
   - Full Name
   - Company Name
   - Country (currency auto-fills)
   - Email & Password
4. **Login** and explore! 🎉

### Demo Data (Optional)

Want to test with sample data?

```bash
# Run the seeder script
docker compose exec backend python seed.py
```

**Demo accounts created:**
- `admin@demo.com` / `admin123` (Admin)
- `manager1@demo.com` / `manager123` (Manager)
- `employee1@demo.com` / `employee123` (Employee)

### Stop the Application

```bash
# Stop containers
docker compose down

# Stop and remove all data (fresh start)
docker compose down -v
```

## 🛠️ Local Development (without Docker)

### Backend
```bash
cd backend
python -m venv .venv
.venv\Scripts\activate  # Windows
# source .venv/bin/activate  # Linux/Mac
pip install -r requirements.txt
uvicorn app.main:app --reload --app-dir .
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

## 📖 User Guide

### For Employees
1. **Submit Expense**: Click "New Expense" → Fill details → Submit
2. **Track Status**: View approval progress in "My Expenses"
3. **OCR Upload**: Upload receipt image for auto-extraction (coming soon)

### For Managers
1. **Review Approvals**: Navigate to "Approvals" tab
2. **Select Expense**: Click on pending expense to review details
3. **Make Decision**: Approve or reject with optional comment
4. **View Team Expenses**: Check "All Expenses" for company-wide view

### For Admins
1. **Add Users**: Admin Panel → Users → Add User
2. **Assign Managers**: Set manager relationships when creating users
3. **Configure Workflows**: Admin Panel → Approval Rules → Add Rule
4. **Choose Rule Type**:
   - **Percentage**: Set threshold (e.g., 60% approval required)
   - **Specific Approver**: Designate key decision-maker (e.g., CFO)
   - **Hybrid**: Combine both for flexible workflows

## 🏗️ Architecture

### Backend (FastAPI)
```
backend/
├── app/
│   ├── main.py                 # FastAPI app & routes
│   ├── core/
│   │   ├── config.py          # Settings & environment
│   │   └── security.py        # JWT & password hashing
│   ├── db/
│   │   └── session.py         # SQLAlchemy setup
│   ├── models/
│   │   ├── user.py            # User, roles, manager relationships
│   │   ├── company.py         # Company with currency
│   │   ├── expense.py         # Expense with status tracking
│   │   ├── approval.py        # Approval steps & decisions
│   │   ├── approval_rule.py   # Workflow rules & flows
│   │   └── approval_sequence.py # Multi-level approver sequences
│   ├── schemas/               # Pydantic models
│   ├── api/v1/endpoints/
│   │   ├── auth.py           # Register, login
│   │   ├── users.py          # Current user info
│   │   ├── expenses.py       # CRUD + workflow creation
│   │   ├── approvals.py      # Pending approvals & decisions
│   │   ├── admin.py          # User & rule management
│   │   └── countries.py      # Country/currency API
│   └── services/
│       ├── currency.py       # Exchange rate conversion
│       └── ocr.py            # Receipt parsing (stub)
```

### Frontend (React + TypeScript)
```
frontend/
├── src/
│   ├── App.tsx               # Router & auth provider
│   ├── main.tsx              # React Query setup
│   ├── types.ts              # TypeScript interfaces
│   ├── contexts/
│   │   └── AuthContext.tsx   # Auth state management
│   ├── api/
│   │   └── client.ts         # Axios instance with interceptors
│   ├── components/
│   │   └── Layout.tsx        # Navigation & layout
│   └── pages/
│       ├── Login.tsx         # Auth with country selection
│       ├── Dashboard.tsx     # Stats & recent expenses
│       ├── Expenses.tsx      # Submit & view expenses
│       ├── Approvals.tsx     # Review pending approvals
│       ├── AllExpenses.tsx   # Company-wide expense table
│       └── Admin.tsx         # User & rule management
```

## 🔧 Configuration

### Environment Variables (backend/.env)
```env
DATABASE_URL=postgresql+psycopg2://postgres:postgres@db:5432/expenses
JWT_SECRET=your-secret-key-here
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60
REDIS_URL=redis://redis:6379/0
OCR_ENABLED=true
```

## 🌐 API Endpoints

### Authentication
- `POST /api/v1/auth/register` - Create admin account & company
- `POST /api/v1/auth/login` - Login with email/password

### Users
- `GET /api/v1/users/me` - Get current user info

### Expenses
- `POST /api/v1/expenses/` - Create expense (triggers workflow)
- `GET /api/v1/expenses/` - List my expenses
- `GET /api/v1/expenses/all` - List all company expenses (Manager/Admin)
- `POST /api/v1/expenses/ocr` - Upload receipt for OCR

### Approvals
- `GET /api/v1/approvals/pending` - Get expenses awaiting my approval
- `POST /api/v1/approvals/{step_id}/decide` - Approve/reject expense

### Admin
- `GET /api/v1/admin/users` - List all users
- `POST /api/v1/admin/users` - Create user
- `PATCH /api/v1/admin/users/{id}` - Update user
- `GET /api/v1/admin/approval-rules` - List approval rules
- `POST /api/v1/admin/approval-rules` - Create approval rule

### Countries
- `GET /api/v1/countries/` - Get all countries with currencies

## 🎯 Workflow Examples

### Example 1: Simple Manager Approval
```
Employee submits expense
  ↓
Manager reviews
  ↓
Approved/Rejected
```

### Example 2: Multi-Level Sequential
```
Employee submits expense
  ↓
Manager approves (Step 1)
  ↓
Finance approves (Step 2)
  ↓
Director approves (Step 3)
  ↓
Approved
```

### Example 3: Percentage Rule (60%)
```
Employee submits expense
  ↓
3 approvers assigned
  ↓
2 approve (66% > 60%)
  ↓
Auto-approved
```

### Example 4: Hybrid (60% OR CFO)
```
Employee submits expense
  ↓
CFO approves
  ↓
Auto-approved (specific approver rule)
```

## 🚧 Production Checklist

- [ ] Change `JWT_SECRET` in `.env`
- [ ] Set up Alembic migrations
- [ ] Configure CORS for production domain
- [ ] Integrate real OCR (Tesseract)
- [ ] Add file upload for receipts (S3/MinIO)
- [ ] Set up proper logging
- [ ] Add rate limiting
- [ ] Configure SSL/HTTPS
- [ ] Set up monitoring (Sentry, DataDog)
- [ ] Add email notifications for approvals
- [ ] Implement CSV/Excel export
- [ ] Add audit trail logging

## 📝 Important Notes

### Development
- **Database**: Tables auto-create on startup (use Alembic for production migrations)
- **Logging**: All requests logged to `backend/logs/app.log`
- **Error Handling**: Comprehensive error handlers with user-friendly messages
- **Health Check**: Monitor system health at `/health` endpoint
- **TypeScript Lints**: Expected until dependencies install - they'll disappear after `npm install`

### External APIs
- **Currency Conversion**: https://api.exchangerate-api.com/v4/latest/{BASE}
- **Country Data**: https://restcountries.com/v3.1/all?fields=name,currencies

### Theme System
- **Light Mode**: Canva-inspired with purple (#7d2ae8) primary color
- **Dark Mode**: GitHub-inspired with green (#238636) accents
- **Persistence**: Theme preference saved in localStorage
- **Smooth Transitions**: 200ms color transitions between modes

## 🤝 Contributing

This is a complete, production-ready scaffold. Extend it with:
- Real OCR integration
- Email notifications
- Advanced reporting
- Mobile app
- Audit trails
- Budget tracking

## 📄 License

MIT License - feel free to use for your projects!
