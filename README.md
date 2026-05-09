# UmaStarGam

UmaStarGam is a modern social blogging platform inspired by Instagram, Medium, and Tumblr.

The platform allows users to create posts, upload images, interact with communities, follow creators, and share their stories in a clean and modern environment.

---

<img src="https://i.pinimg.com/736x/9b/66/67/9b66674874d39b85c3a1cdacfa5b23e3.jpg" width="24px" height="24px" >

# Overview

UmaStarGam combines social media interaction with long-form content creation.

Users can:
- Share moments through images and posts
- Write blogs and personal stories
- Follow creators and communities
- Interact through likes, comments, and bookmarks
- Build their own digital profile and audience

The project is designed with scalability, clean architecture, and modern web technologies in mind.

---

# Features

## Authentication
- Register / Login
- JWT Authentication
- OAuth Support
- Password Hashing
- Protected Routes

## Posts
- Create, edit, and delete posts
- Rich text / Markdown editor
- Image uploads
- Post categories and tags

## Social Features
- Like system
- Comment system
- Follow / unfollow users
- Save or bookmark posts

## Feed System
- Latest posts feed
- Following feed
- Trending posts

## User Profiles
- Avatar upload
- Bio customization
- Followers and following
- User activity history

---

# Tech Stack

## Frontend
- ReactJS
- React Router DOM
- Tailwind CSS
- Axios
- Zustand / Redux Toolkit
- Framer Motion
- Vite

## Backend
- NestJS
- Prisma ORM
- PostgreSQL
- Passport.js
- JWT Authentication
- REST API

## Storage & Media
- Cloudinary
- Supabase Storage

## Deployment
- Vercel
- Railway
- Render
- Docker

---

# Architecture Stack

## Client Side
The frontend is built using ReactJS with a component-based architecture.

Responsibilities:
- UI Rendering
- State Management
- API Communication
- Authentication Handling
- Responsive Design

## Server Side
The backend is built with NestJS following modular architecture principles.

Responsibilities:
- REST API
- Authentication & Authorization
- Database Management
- Business Logic
- Media Handling

## Database
PostgreSQL is used as the primary relational database.

Main entities:
- Users
- Posts
- Comments
- Likes
- Followers
- Notifications

---

# Project Structure

```
UmaStarGam/
  client/                 # ReactJS frontend
    src/
    components/
    pages/
    hooks/
    services/
    store/
  
  server/                 # NestJS backend
    src/
    auth/
    users/
    posts/
    comments/
    prisma/
  
  README.md
```
# Installation

## Clone Repository

```bash
git clone https://github.com/your-username/UmaStarGam.git
cd UmaStarGam
```

## Frontend Setup

```bash
cd client
npm install
npm run dev
```

Frontend server:

http://localhost:5173

## Backend Setup

```bash
cd server
npm install
```

Create `.env` file:

```env
DATABASE_URL=""
JWT_SECRET=""
PORT=3000
```

## Database Migration

```bash
npx prisma migrate dev
```

## Start Backend Server

```bash
npm run start:dev
```

Backend server:

http://localhost:3000

# Future Plans

- Realtime notifications
- Chat system
- AI-generated captions
- Reels / short videos
- Mobile application
- Recommendation system
- Creator analytics dashboard

# Goals

UmaStarGam aims to become a platform where users can combine creativity, blogging, and social interaction in one modern ecosystem.

The project prioritizes:

- Clean UI/UX
- Performance
- Scalability
- Community interaction
- Creator freedom

# License

MIT License