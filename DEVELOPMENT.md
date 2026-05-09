# Development Guide

This guide helps you set up your development environment and understand the project structure.

## Prerequisites

- Node.js (v16+)
- npm or yarn
- PostgreSQL (v12+)
- Git

## Environment Setup

### 1. Clone and Install

```bash
git clone https://github.com/your-username/UmaStarGam.git
cd UmaStarGam
```

### 2. Backend Setup

```bash
cd server
npm install
```

Create `.env` file:

```env
DATABASE_URL="postgresql://user:password@localhost:5432/umastar_db"
JWT_SECRET="your-secret-key-here"
JWT_EXPIRE="7d"
PORT=3000
CLOUDINARY_NAME="your-cloudinary-name"
CLOUDINARY_API_KEY="your-api-key"
CLOUDINARY_API_SECRET="your-api-secret"
```

Initialize database:

```bash
npx prisma migrate dev
npx prisma db seed
```

Start backend:

```bash
npm run start:dev
```

### 3. Frontend Setup

```bash
cd ../client
npm install
```

Create `.env` file:

```env
VITE_API_URL="http://localhost:3000"
VITE_APP_NAME="UmaStarGam"
```

Start frontend:

```bash
npm run dev
```

## Development Workflow

### Running Tests

Backend:
```bash
cd server
npm test
```

Frontend:
```bash
cd client
npm test
```

### Building for Production

Backend:
```bash
cd server
npm run build
npm start
```

Frontend:
```bash
cd client
npm run build
```

### Database Migrations

Create a new migration:
```bash
npx prisma migrate dev --name add_new_feature
```

## Project Folders

### Backend (`/server`)

- `/src/auth` - Authentication & JWT logic
- `/src/users` - User management
- `/src/posts` - Post creation & management
- `/src/comments` - Comment system
- `/src/prisma` - Database schema & migrations

### Frontend (`/client`)

- `/src/components` - Reusable React components
- `/src/pages` - Page components
- `/src/hooks` - Custom React hooks
- `/src/services` - API calls & external services
- `/src/store` - State management (Zustand/Redux)

## Debugging

### Backend Debugging

```bash
cd server
npm run start:debug
```

Then attach your debugger to `localhost:9229`

### Frontend Debugging

Open DevTools in your browser (F12) and use React Developer Tools extension.

## Common Issues

### Database Connection Errors

- Verify PostgreSQL is running
- Check DATABASE_URL in `.env`
- Ensure database exists

### CORS Errors

- Check backend CORS settings
- Verify frontend API URL in `.env`

### Port Already in Use

```bash
# Change port in .env or stop other services
lsof -i :3000  # Check what's using port 3000
kill -9 <PID>  # Kill the process
```

## Need Help?

- Check existing issues: https://github.com/your-username/UmaStarGam/issues
- Read documentation: `/docs`
- Open a discussion for questions

Happy coding! 🚀
