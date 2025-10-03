# 🚀 API Automation Testing with Bruno CLI & GitHub Actions

A complete, minimal working example demonstrating API automation testing using **Bruno CLI**, **GitHub Actions**, and the **JSONPlaceholder public API**. Perfect for learning CI/CD automation.

---

## 📋 Table of Contents

- [Project Structure](#-project-structure)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [How to Run Tests Locally](#-how-to-run-tests-locally)
- [Understanding the Tests](#-understanding-the-tests)
- [GitHub Actions Workflow](#-github-actions-workflow)
- [Frontend Application](#-frontend-application)
- [GitHub Pages Deployment](#-github-pages-deployment)
- [Troubleshooting](#-troubleshooting)

---

## 📁 Project Structure

```
app-bruno-cli-automation/
├── .github/
│   └── workflows/
│       └── api-tests.yml                 # GitHub Actions workflow
├── bruno/
│   ├── collection.json                   # Bruno collection config
│   └── requests/
│       ├── 01_GET_users.bru             # Test: Get all users
│       ├── 02_GET_user_by_id.bru        # Test: Get single user with validation
│       ├── 03_GET_posts.bru             # Test: Get posts with query params
│       ├── 04_Negative_Test_404.bru     # Test: Negative test case
│       └── 05_Schema_Validation.bru     # Test: Response schema validation
├── frontend/
│   └── index.html                        # Standalone HTML/JS app
├── package.json                          # Node dependencies
├── .gitignore                            # Git ignore rules
└── README.md                             # This file
```

---

## 📦 Prerequisites

- **Node.js 16+** ([Download](https://nodejs.org/))
- **Git** installed
- A **GitHub account** (for CI/CD)
- **Bruno CLI** (installed via npm)

---

## ⚡ Quick Start

### 1. Clone or Create the Repository

```bash
# Navigate to your dev directory
cd ~/dev/Public
git clone https://github.com/YOUR_USERNAME/app-bruno-cli-automation.git
cd app-bruno-cli-automation
```

### 2. Install Dependencies

```bash
npm install
```

This installs Bruno CLI locally (or use `npm install -g @usebruno/cli` for global installation).

### 3. Run Tests

```bash
npm test
```

You should see 5 test files execute with all tests passing ✅

---

## 🧪 How to Run Tests Locally

### Using npm script (Recommended)

```bash
npm test
```

### Using Bruno CLI directly

```bash
bru run ./bruno --reporter cli
```

### Generate JSON report

```bash
npm run test:json
```

### Run a single test file

```bash
bru run ./bruno/requests/01_GET_users.bru
```

---

## 📊 Understanding the Tests

### Test Files Overview

| File | Purpose | What It Tests |
|------|---------|---------------|
| **01_GET_users.bru** | GET all users | Status 200, array response, required fields |
| **02_GET_user_by_id.bru** | GET single user | Specific user data, field validation, correct ID |
| **03_GET_posts.bru** | GET posts with filters | Query parameters, array response, userId filter |
| **04_Negative_Test_404.bru** | Handle missing data | Invalid ID behavior, empty response |
| **05_Schema_Validation.bru** | Response schema | Field types, nested objects, headers |

### Example Test (01_GET_users.bru)

```javascript
meta {
  name: Get All Users
  type: http
  seq: 1
}

get {
  url: https://jsonplaceholder.typicode.com/users
  body: none
}

tests {
  test("Status should be 200", function() {
    expect(res.getStatus()).to.equal(200);
  });
  
  test("Response should be an array", function() {
    expect(res.getBody()).to.be.an('array');
  });
  
  test("First user should have required fields", function() {
    const users = res.getBody();
    const firstUser = users[0];
    expect(firstUser).to.have.property('id');
    expect(firstUser).to.have.property('name');
    expect(firstUser).to.have.property('email');
  });
}
```

**Test assertions covered:**
- ✅ HTTP status codes
- ✅ Response type validation
- ✅ Field existence checks
- ✅ Data type validation
- ✅ Nested object validation

---

## 🤖 GitHub Actions Workflow

### How It Works

When you `push` to the repository or create a `pull request`:

1. **Workflow triggers** (Main or Develop branch)
2. **Sets up Node.js** environment (version 18.x)
3. **Installs Bruno CLI** globally
4. **Runs all tests** from the `bruno/` directory
5. **Reports results** in the GitHub Actions UI
6. **Fails the pipeline** if any test fails ❌

### Workflow File (.github/workflows/api-tests.yml)

```yaml
name: API Automation Tests

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 18.x
      - run: npm install -g @usebruno/cli
      - run: bru run ./bruno --reporter cli
```

### Viewing Test Results in GitHub

1. Go to your repository on GitHub
2. Click **Actions** tab
3. Select the latest workflow run
4. Expand **Run API Tests** step to see detailed logs
5. All test assertions and results are displayed

---

## 🌐 Frontend Application

A standalone HTML/JavaScript application that consumes the **same API** used in the tests.

### Features

- ✨ Clean, modern UI (no build tools required)
- 📋 Load Users from the API
- 📄 Load Posts data
- 💬 Load Comments
- 📊 Display item counts
- 🔄 Real-time API calls
- 📱 Fully responsive design

### Running the Frontend

#### Option 1: Open in Browser

Simply open the file in your browser:

```bash
open frontend/index.html
# or
# Double-click the file in Finder/Explorer
```

#### Option 2: Using a Local Server

```bash
# Using Python 3
python -m http.server 8000

# Then open: http://localhost:8000/frontend/index.html
```

```bash
# Using Node.js
npx http-server
```

---

## 📖 GitHub Pages Deployment

Deploy the frontend automatically using GitHub Pages.

### Steps

1. **Push code to GitHub**
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

2. **Enable GitHub Pages**
   - Go to repository **Settings** → **Pages**
   - Set **Source** to `main` branch
   - Set folder to `frontend/` (or `/root` if you move index.html to root)
   - Click **Save**

3. **Access your site**
   - Your frontend will be available at: `https://YOUR_USERNAME.github.io/app-bruno-cli-automation/frontend/`

### Alternative: Move index.html to Root

If you want it at the repository root:

```bash
mv frontend/index.html .
```

Then update GitHub Pages source to `/` root.

---

## 🔄 Complete Setup Walkthrough

### Step 1: Initialize Git Repository

```bash
cd app-bruno-cli-automation
git init
git add .
git commit -m "Initial commit: API automation project"
```

### Step 2: Create GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Create a new repository named `app-bruno-cli-automation`
3. Follow the instructions to push existing code

```bash
git remote add origin https://github.com/YOUR_USERNAME/app-bruno-cli-automation.git
git branch -M main
git push -u origin main
```

### Step 3: Watch GitHub Actions Run

1. Go to **Actions** tab
2. See the workflow execute automatically
3. All tests will run and report results

### Step 4: Deploy Frontend (Optional)

Enable GitHub Pages following steps in the [GitHub Pages Deployment](#-github-pages-deployment) section.

---

## 📈 Test Execution Flow

```
GitHub Push
    ↓
GitHub Actions Triggered
    ↓
checkout code
    ↓
Setup Node.js 18.x
    ↓
Install Bruno CLI
    ↓
Execute: bru run ./bruno --reporter cli
    ↓
Runs all .bru files in order:
    ├─ 01_GET_users.bru (10 tests)
    ├─ 02_GET_user_by_id.bru (5 tests)
    ├─ 03_GET_posts.bru (5 tests)
    ├─ 04_Negative_Test_404.bru (3 tests)
    └─ 05_Schema_Validation.bru (4 tests)
    ↓
Total: 27 test assertions
    ↓
Summary Report in Logs
    ↓
✅ Success or ❌ Failure
```

---

## 🛠️ Troubleshooting

### Issue: Bruno CLI not found

**Solution:**
```bash
npm install -g @usebruno/cli
# Verify installation
bru --version
```

### Issue: Tests timeout on CI

**Solution:** Add timeout to workflow:
```yaml
- name: Run API Tests
  run: bru run ./bruno --reporter cli
  timeout-minutes: 5
```

### Issue: GitHub Pages not deploying

**Solution:**
1. Check **Settings** → **Pages** is enabled
2. Verify branch is set to `main`
3. Check for `.github/workflows/pages.yml` (auto-generated)
4. Wait 1-2 minutes for deployment

### Issue: Local tests fail but CI passes

**Solution:**
- Check Node.js version: `node --version`
- Verify internet connectivity to JSONPlaceholder API
- Clear npm cache: `npm cache clean --force`

---

## 📝 Next Steps

### Enhance Your Project

1. **Add more test scenarios**
   - POST requests with request bodies
   - PUT/PATCH update tests
   - DELETE operations

2. **Setup Environment Variables**
   ```bash
   # Store API endpoints in .env
   API_BASE=https://jsonplaceholder.typicode.com
   ```

3. **Add Request Chaining**
   - Use variables across multiple requests
   - Setup pre-request scripts

4. **Integration with Notifications**
   - Slack notifications on test failure
   - Email reports

5. **Performance Testing**
   - Add response time assertions
   - Monitor API performance

---

## 🎓 Learning Resources

- **Bruno Documentation**: [docs.usebruno.com](https://docs.usebruno.com)
- **GitHub Actions Guide**: [github.com/actions](https://github.com/actions)
- **JSONPlaceholder API**: [jsonplaceholder.typicode.com](https://jsonplaceholder.typicode.com)
- **Testing Best Practices**: [API Testing Handbook](https://www.saucelabs.com/blog/api-testing)

---

## 📄 License

MIT License - Feel free to use this as a template for your own projects.

---

## 🤝 Contributing

This is a learning project. Feel free to:
- Add new test scenarios
- Improve the frontend
- Enhance the GitHub Actions workflow
- Share improvements!

---

**Happy Testing! 🎉**

Built with ❤️ for learning DevOps and QA automation.
