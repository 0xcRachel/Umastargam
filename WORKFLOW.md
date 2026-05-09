# Workflow - UmaStarGam Project Operations

This document provides a detailed description of the project's operational processes from the user's perspective, the developer's perspective, to the deployment process.

---

## Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [User Workflow](#user-workflow)
3. [Development Workflow](#development-workflow)
4. [Data Flow](#data-flow)
5. [CI/CD Pipeline](#cicd-pipeline)
6. [Contribution Process](#contribution-process)

---

## Architecture Overview

### 3-Tier Architecture

```
┌─────────────────────────────────────────────────────┐
│          Frontend (ReactJS + Vite)                  │
│  - UI Components                                    │
│  - State Management (Zustand/Redux Toolkit)         │
│  - API Communication (Axios)                        │
│  - Responsive Design (Tailwind CSS)                 │
└──────────────────┬──────────────────────────────────┘
                   │ HTTP/REST API
┌──────────────────▼──────────────────────────────────┐
│         Backend (NestJS + Passport)                 │
│  - REST Endpoints                                   │
│  - Authentication (JWT + OAuth)                     │
│  - Business Logic                                   │
│  - Database Operations                              │
└──────────────────┬──────────────────────────────────┘
                   │ Prisma ORM
┌──────────────────▼──────────────────────────────────┐
│        Database (PostgreSQL)                        │
│  - Users                                            │
│  - Posts                                            │
│  - Comments                                         │
│  - Likes, Follows, Bookmarks                        │
└─────────────────────────────────────────────────────┘
```

### Core Components

| Component | Role | Technology |
|-----------|------|----------|
| **Frontend** | User Interface | ReactJS, Tailwind CSS, Vite |
| **Backend** | Business Logic & API | NestJS, Passport.js |
| **Database** | Data Storage | PostgreSQL, Prisma ORM |
| **Storage** | Image Storage | Cloudinary, Supabase |
| **Auth** | User Authentication | JWT, OAuth |

---

## User Workflow

### 1. Registration Process

```
User --> [Fill Registration Form]
          ├─ Email
          ├─ Password
          └─ Username
            │
            ▼
         Submit
            │
            ▼
      Frontend Validation
            │
            ▼
    POST /auth/register
            │
            ▼
      Backend Validation
      ├─ Check email exists?
      ├─ Validate password
      └─ Hash password
            │
            ▼
      Create User in DB
            │
            ▼
      Return JWT Token
            │
            ▼
      Store Token (localStorage)
      User --> Dashboard
```

### 2. Login Process

```
User --> [Enter Credentials]
          ├─ Email/Username
          └─ Password
            │
            ▼
      POST /auth/login
            │
            ▼
      Backend Verification
      ├─ Find user
      ├─ Verify password
      └─ Generate JWT
            │
            ▼
      Return Token
            │
            ▼
      Store Token (localStorage)
      User --> Feed/Dashboard
```

### 3. Create Post Process

```
User --> [Compose Post]
          ├─ Write text
          ├─ Upload image(s)
          ├─ Add tags
          └─ Select category
            │
            ▼
      Frontend Upload
      ├─ Validate content
      └─ Prepare form data
            │
            ▼
    POST /posts/create
            │
            ▼
      Upload image to Cloudinary
            │
            ▼
      Save post metadata to DB
      ├─ Content
      ├─ Image URL
      ├─ Tags
      ├─ Author ID
      └─ Timestamp
            │
            ▼
      Return Post Object
            │
            ▼
      Show confirmation
      Update user's feed
```

### 4. Social Interaction Process

```
┌─ Like Post
│  User [Click Like]
│  ├─ POST /posts/{id}/like
│  ├─ Backend: Create Like record
│  └─ Update like count
│
├─ Comment on Post
│  User [Write Comment]
│  ├─ POST /posts/{id}/comments
│  ├─ Save comment to DB
│  └─ Notify post author
│
├─ Bookmark Post
│  User [Click Bookmark]
│  ├─ POST /posts/{id}/bookmark
│  ├─ Save to user's bookmarks
│  └─ Show success message
│
└─ Follow User
   User [Click Follow]
   ├─ POST /users/{id}/follow
   ├─ Create follow relationship
   └─ Update followers count
```

### 5. View Feed Process

```
User opens app
       │
       ▼
Check JWT Token
       │
    ┌──┴──┐
    │     │
  Valid  Invalid
    │     └─> Redirect to Login
    │
    ▼
GET /feed
    │
    ▼
Backend fetch posts
├─ User's following feed
├─ Latest posts
└─ Trending posts
    │
    ▼
Apply filters & sorting
    │
    ▼
Paginate results
    │
    ▼
Return posts with metadata
├─ Author info
├─ Like count
├─ Comment count
└─ User's interactions
    │
    ▼
Display in UI
```

---

## Development Workflow

### 1. Environment Setup Process

```
Developer clones repo
       │
       ├─ Frontend Setup
       │  ├─ cd /client
       │  ├─ npm install
       │  ├─ Create .env file
       │  └─ npm run dev
       │
       └─ Backend Setup
          ├─ cd /server
          ├─ npm install
          ├─ Create .env file
          ├─ Setup PostgreSQL
          ├─ npx prisma migrate dev
          ├─ npx prisma db seed
          └─ npm run start:dev

        ▼
   Both services running
   - Frontend: http://localhost:5173
   - Backend: http://localhost:3000
```

### 2. Feature Development Process

```
1. Create Feature Branch
   └─ git checkout -b feature/feature-name

2. Development Phase
   ├─ Write code
   ├─ Test locally
   ├─ Run ESLint & Prettier
   └─ Write unit tests

3. Testing
   ├─ Frontend tests
   │  └─ npm test
   ├─ Backend tests
   │  └─ npm test
   └─ Manual testing

4. Commit Changes
   ├─ Follow commit conventions
   ├─ Clear commit messages
   └─ git push origin feature/feature-name

5. Create Pull Request
   ├─ Fill PR template
   ├─ Describe changes
   ├─ Link issues
   └─ Request review

6. Code Review
   ├─ Address feedback
   ├─ Push updates
   └─ Get approval

7. Merge to Main
   └─ Delete feature branch
```

### 3. Build & Test Process

```
Development
    │
    ├─ Frontend Build
    │  ├─ npm run build
    │  ├─ Generate dist/
    │  └─ Output size report
    │
    └─ Backend Build
       ├─ npm run build
       ├─ Generate dist/
       └─ Type checking
    
        ▼
    
    Run Tests
    ├─ Unit tests
    ├─ Integration tests
    └─ E2E tests (if available)
    
        ▼
    
    All Pass
```

---

## Data Flow

### 1. Create Post Data Flow

```
Frontend                      Backend                    Database
    │                            │                            │
    ├─ Form Data ──────────────> │                            │
    │  {title, content, image}  │                            │
    │                            │                            │
    │                ┌───────────┴──────────────┐             │
    │                │                          │             │
    │                ├─ Validate data          │             │
    │                ├─ Upload image           │             │
    │                └─ Process content        │             │
    │                            │                            │
    │                ┌───────────▼──────────────┐             │
    │                │                          │             │
    │                │ Create post record ────────────────> │
    │                │                          │             │
    │                │                    Response            │
    │                │                    (Post ID)           │
    │                │<──────────────────────────           │
    │                │                          │             │
    │ ◄──────────────┤ Post Object              │             │
    │                │ (with generated ID)      │             │
    │                │                          │             │
    └─ Update UI    │                          │             │
       (Show post)  │                          │             │
```

### 2. Get Feed Data Flow

```
Frontend                      Backend                    Database
    │                            │                            │
    ├─ Request Feed ────────────>│                            │
    │  GET /feed?page=1          │                            │
    │                            │                            │
    │                ┌───────────▼──────────────┐             │
    │                │                          │             │
    │                ├─ Get user ID            │             │
    │                ├─ Get following list     ────────────> │
    │                │<──────────────────────────             │
    │                │                          │             │
    │                ├─ Query posts from       │             │
    │                │  following accounts     ────────────> │
    │                │<──────────────────────────             │
    │                │                          │             │
    │                ├─ Get likes/comments     ────────────> │
    │                │  for each post          │             │
    │                │<──────────────────────────             │
    │                │                          │             │
    │ ◄──────────────┤ Posts Array              │             │
    │                │ (with metadata)          │             │
    │                │                          │             │
    └─ Render Feed  │                          │             │
       (UI Updates) │                          │             │
```

### 3. Authentication Data Flow

```
Frontend                      Backend
    │                            │
    ├─ Credentials ─────────────> │
    │  {email, password}         │
    │                            │
    │            ┌───────────────┴──────────┐
    │            │                          │
    │            ├─ Find user in DB        │
    │            ├─ Verify password        │
    │            ├─ Generate JWT Token     │
    │            └─ Generate Refresh Token │
    │                            │
    │ ◄───────────────────────────┤
    │  {accessToken, refreshToken}│
    │                            │
    ├─ Store Tokens              │
    │  localStorage & sessionStorage
    │
    ├─ Include JWT in Headers    │
    │  ┌────────────────────────────────────┐
    │  │ Authorization: Bearer {accessToken} │
    │  └────────────────────────────────────┘
    │                            │
    └─ All Requests ────────────> │
                                 │
            ┌──────────────────────┐
            │                      │
      Valid Token           Invalid/Expired
            │                      │
        Continue ◄────────── Refresh Token
```

---

## CI/CD Pipeline

### 1. Deployment Pipeline

```
Developer Push to Main Branch
            │
            ▼
GitHub Actions triggered
            │
    ┌───────┴───────┐
    │               │
   Lint          Build
    │               │
    ├─ ESLint       ├─ Frontend build
    ├─ Prettier     └─ Backend build
    └─ Type check
    │               │
    └───────┬───────┘
            │
            ▼
         Tests
    ├─ Unit tests
    ├─ Integration tests
    └─ E2E tests
    │
    ├─ Success ─────────────────> Deploy
    │                                 │
    │                      ┌──────────┼──────────┐
    │                      │          │          │
    │                   Frontend  Backend   Database
    │                      │          │          │
    │                   Vercel    Railway    Auto
    │                      │          │       migrate
    │                      └──────────┼────────┘
    │                                 │
    └─ Failure ──────────────> Alert & Stop
```

### 2. Staging Deployment

```
Staging Environment
├─ Frontend: Vercel Preview
├─ Backend: Railway Staging
└─ Database: PostgreSQL Staging

QA Testing
├─ Manual testing
├─ Smoke tests
└─ User acceptance testing

If Pass ──> Production Deployment
If Fail ──> Fix & Retry
```

### 3. Production Deployment

```
Production Environment
├─ Frontend: Vercel Production
├─ Backend: Railway Production
└─ Database: PostgreSQL Production

Monitoring
├─ Error tracking (Sentry)
├─ Performance monitoring
├─ Uptime monitoring
└─ Log aggregation

Rollback Plan
├─ Revert to previous version
├─ Database rollback
└─ Notify users
```

---

## Contribution Process

### PR Approval Process

```
Contributor
    │
    ├─ Fork repository
    ├─ Create feature branch
    ├─ Make changes
    ├─ Commit with conventional message
    └─ Push & Create PR
            │
            ▼
    PR Template
    ├─ Description
    ├─ Related Issues
    ├─ Type of change
    └─ Checklist
            │
            ▼
    Automated Checks
    ├─ CI/CD pipeline
    ├─ Code quality
    └─ Tests
            │
        ┌───┴───┐
        │       │
      Pass    Fail
        │       │
        │       └─> Request changes
        │
        ▼
    Code Review
    ├─ Maintainers review
    ├─ Request changes (if any)
    └─ Approve
            │
        ┌───┴───┐
        │       │
    Approved Changes Needed
        │       │
        │       └─> Address feedback
        │           Re-request review
        │
        ▼
    Merge to Main
    ├─ Auto-delete branch
    └─ Close related issues
```

---

## Project Management Workflow

### Development Layers

```
User Requirements
        │
        ▼
Product Manager
├─ Define features
├─ Create user stories
└─ Set priorities
        │
        ▼
Issue Tracking
├─ Create issues
├─ Label & Assign
└─ Set milestones
        │
        ▼
Developer Planning
├─ Estimate effort
├─ Break down tasks
└─ Sprint planning
        │
        ▼
Development Sprint
├─ 1-2 weeks
├─ Daily standup
└─ Code reviews
        │
        ▼
Testing & QA
├─ Functional testing
├─ Performance testing
└─ Security testing
        │
        ▼
Release
├─ Version bumping
├─ Changelog
└─ Deployment
```

---

## Security Workflow

### Security Flow

```
Data Flow Security
├─ HTTPS/TLS encryption
├─ JWT token validation
├─ CORS configuration
└─ Rate limiting

Database Security
├─ SQL injection prevention (Prisma ORM)
├─ Password hashing (bcrypt)
├─ Data encryption
└─ Backup strategy

Authentication Security
├─ Secure password requirements
├─ OAuth2 implementation
├─ Token expiration
└─ Refresh token rotation

Code Security
├─ Dependency scanning
├─ Code review process
├─ Security testing
└─ Vulnerability disclosure
```

---

## Documentation References

| Document | Content |
|----------|---------|
| [README.md](README.md) | Project Overview |
| [DEVELOPMENT.md](DEVELOPMENT.md) | Development Guide |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contributing Guide |

---

## FAQ

**Q: How do I start development?**
A: Check [DEVELOPMENT.md](DEVELOPMENT.md) for environment setup.

**Q: What is the contribution process?**
A: Check [CONTRIBUTING.md](CONTRIBUTING.md) for details.

**Q: What permissions do I need to deploy?**
A: Contact the project administrator to get access to Vercel, Railway, and Supabase.

**Q: How do I report a bug?**
A: Create an issue on GitHub with detailed information about the bug.

---

**Version:** 1.0  
**Last Updated:** May 10, 2026  
**Author:** UmaStarGam Team
