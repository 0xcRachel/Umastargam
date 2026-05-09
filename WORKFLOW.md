# Workflow - Hoạt động của Dự án UmaStarGam

Tài liệu này mô tả chi tiết quy trình hoạt động của dự án UmaStarGam từ phía người dùng, phía lập trình viên, cho đến quá trình triển khai.

---

## Mục lục

1. [Tổng quan kiến trúc](#tổng-quan-kiến-trúc)
2. [Quy trình người dùng](#quy-trình-người-dùng)
3. [Quy trình phát triển](#quy-trình-phát-triển)
4. [Quy trình dữ liệu](#quy-trình-dữ-liệu)
5. [Quy trình CI/CD](#quy-trình-cicd)
6. [Quy trình đóng góp](#quy-trình-đóng-góp)

---

## Tổng quan kiến trúc

### Kiến trúc 3 tầng

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

### Thành phần chính

| Thành phần | Vai trò | Công nghệ |
|-----------|--------|----------|
| **Frontend** | Giao diện người dùng | ReactJS, Tailwind CSS, Vite |
| **Backend** | Xử lý logic, API | NestJS, Passport.js |
| **Database** | Lưu trữ dữ liệu | PostgreSQL, Prisma ORM |
| **Storage** | Lưu trữ hình ảnh | Cloudinary, Supabase |
| **Auth** | Xác thực người dùng | JWT, OAuth |

---

## Quy trình người dùng

### 1. Quy trình Đăng ký (Registration)

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

### 2. Quy trình Đăng nhập (Login)

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

### 3. Quy trình Tạo Bài Viết (Create Post)

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

### 4. Quy trình Tương tác Xã hội (Social Interaction)

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

### 5. Quy trình Xem Feed

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

## Quy trình phát triển

### 1. Quy trình Thiết lập Môi trường

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
   Cả 2 service chạy
   - Frontend: http://localhost:5173
   - Backend: http://localhost:3000
```

### 2. Quy trình Phát triển Tính năng

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

### 3. Quy trình Build & Test

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

## Quy trình dữ liệu

### 1. Luồng dữ liệu Tạo Bài Viết

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

### 2. Luồng dữ liệu Lấy Feed

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

### 3. Luồng dữ liệu Authentication

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

## Quy trình CI/CD

### 1. Pipeline Deployment

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

### 2. Deployment Staging

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

## Quy trình đóng góp

### Quy trình Phê duyệt PR

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

## Quy trình Quản lý Dự án

### Tầng lớp Phát triển

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

## Quy trình Bảo mật

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

## Tài liệu tham khảo

| Tài liệu | Nội dung |
|---------|---------|
| [README.md](README.md) | Tổng quan dự án |
| [DEVELOPMENT.md](DEVELOPMENT.md) | Hướng dẫn phát triển |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Hướng dẫn đóng góp |

---

## FAQ

**Q: Làm thế nào để bắt đầu phát triển?**
A: Xem [DEVELOPMENT.md](DEVELOPMENT.md) để thiết lập môi trường.

**Q: Quy trình đóng góp như thế nào?**
A: Xem [CONTRIBUTING.md](CONTRIBUTING.md) để biết chi tiết.

**Q: Tôi cần quyền gì để deploy?**
A: Liên hệ quản trị viên dự án để được cấp quyền trên Vercel, Railway và Supabase.

**Q: Làm thế nào để báo lỗi?**
A: Tạo issue trên GitHub với chi tiết đầy đủ về lỗi.

---

**Phiên bản:** 1.0  
**Cập nhật lần cuối:** May 10, 2026  
**Tác giả:** UmaStarGam Team (0xcRachel and Team)
