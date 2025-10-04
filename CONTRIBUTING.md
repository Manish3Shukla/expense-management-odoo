# Contributing to ExpenseFlow

Thank you for your interest in contributing to ExpenseFlow! This document provides guidelines and instructions for contributing.

## 🌟 Ways to Contribute

- **Bug Reports**: Found a bug? Open an issue with details
- **Feature Requests**: Have an idea? We'd love to hear it
- **Code Contributions**: Submit pull requests for fixes or features
- **Documentation**: Help improve our docs
- **Testing**: Test the application and report issues

## 🚀 Getting Started

### 1. Fork the Repository

Click the "Fork" button at the top right of the repository page.

### 2. Clone Your Fork

```bash
git clone https://github.com/YOUR_USERNAME/expense-management-system.git
cd expense-management-system
```

### 3. Set Up Development Environment

Follow the [SETUP_GUIDE.md](SETUP_GUIDE.md) to set up your local environment.

### 4. Create a Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-fix
```

## 💻 Development Guidelines

### Code Style

#### Backend (Python)
- Follow PEP 8 style guide
- Use type hints
- Write docstrings for functions and classes
- Keep functions focused and small
- Use meaningful variable names

```python
# Good
def calculate_approval_percentage(approved: int, total: int) -> float:
    """Calculate the percentage of approvals.
    
    Args:
        approved: Number of approved steps
        total: Total number of steps
        
    Returns:
        Approval percentage as float
    """
    if total == 0:
        return 0.0
    return (approved / total) * 100
```

#### Frontend (TypeScript/React)
- Use TypeScript for type safety
- Follow React best practices
- Use functional components with hooks
- Keep components small and focused
- Use meaningful component and variable names

```typescript
// Good
interface ExpenseCardProps {
  expense: Expense
  onApprove: (id: number) => void
}

export function ExpenseCard({ expense, onApprove }: ExpenseCardProps) {
  // Component logic
}
```

### Commit Messages

Use clear, descriptive commit messages:

```bash
# Good
git commit -m "feat: Add percentage-based approval rules"
git commit -m "fix: Resolve currency conversion error"
git commit -m "docs: Update setup guide with Docker instructions"

# Format
# <type>: <description>
# 
# Types: feat, fix, docs, style, refactor, test, chore
```

### Testing

Before submitting:

1. **Test your changes locally**
   ```bash
   # Backend
   cd backend
   pytest
   
   # Frontend
   cd frontend
   npm test
   ```

2. **Test with Docker**
   ```bash
   docker compose up --build
   ```

3. **Check for errors in browser console and backend logs**

## 📋 Pull Request Process

### 1. Update Your Branch

```bash
git fetch upstream
git rebase upstream/main
```

### 2. Push Your Changes

```bash
git push origin feature/your-feature-name
```

### 3. Create Pull Request

1. Go to the original repository
2. Click "New Pull Request"
3. Select your branch
4. Fill in the PR template:

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
How did you test this?

## Screenshots (if applicable)
Add screenshots for UI changes

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Documentation updated
- [ ] No new warnings
- [ ] Tests added/updated
- [ ] All tests passing
```

### 4. Code Review

- Address review comments
- Push updates to the same branch
- Be patient and respectful

## 🐛 Reporting Bugs

### Before Reporting

1. Check existing issues
2. Try the latest version
3. Reproduce the bug

### Bug Report Template

```markdown
**Describe the bug**
Clear description of the bug

**To Reproduce**
Steps to reproduce:
1. Go to '...'
2. Click on '...'
3. See error

**Expected behavior**
What should happen

**Screenshots**
If applicable

**Environment:**
- OS: [e.g., Windows 11]
- Browser: [e.g., Chrome 120]
- Docker: [Yes/No]
- Version: [e.g., 1.0.0]

**Additional context**
Any other relevant information
```

## 💡 Feature Requests

### Feature Request Template

```markdown
**Is your feature request related to a problem?**
Clear description of the problem

**Describe the solution you'd like**
What you want to happen

**Describe alternatives you've considered**
Other solutions you've thought about

**Additional context**
Screenshots, mockups, examples
```

## 🏗️ Project Structure

```
expense-management-system/
├── backend/
│   ├── app/
│   │   ├── api/          # API endpoints
│   │   ├── core/         # Config, security
│   │   ├── db/           # Database setup
│   │   ├── models/       # SQLAlchemy models
│   │   ├── schemas/      # Pydantic schemas
│   │   ├── services/     # Business logic
│   │   └── utils/        # Utilities
│   ├── tests/            # Backend tests
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── api/          # API client
│   │   ├── components/   # React components
│   │   ├── contexts/     # React contexts
│   │   ├── pages/        # Page components
│   │   └── types.ts      # TypeScript types
│   └── package.json
└── docker-compose.yml
```

## 🔍 Code Review Checklist

### For Reviewers

- [ ] Code follows project style
- [ ] Changes are well-documented
- [ ] Tests are included
- [ ] No security vulnerabilities
- [ ] Performance considerations addressed
- [ ] Error handling is appropriate
- [ ] UI/UX is consistent

### For Contributors

- [ ] Self-reviewed the code
- [ ] Added comments for complex logic
- [ ] Updated documentation
- [ ] Tested thoroughly
- [ ] No console errors
- [ ] Responsive design (if UI)
- [ ] Accessible (if UI)

## 📚 Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [React Documentation](https://react.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS](https://tailwindcss.com/docs)
- [SQLAlchemy Documentation](https://docs.sqlalchemy.org/)

## 🤝 Community Guidelines

- Be respectful and inclusive
- Provide constructive feedback
- Help others learn and grow
- Follow the code of conduct
- Have fun and build great things!

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

## ❓ Questions?

- Open a discussion on GitHub
- Check existing issues and PRs
- Review the documentation

Thank you for contributing to ExpenseFlow! 🎉
