# API Documentation

UmaStarGam REST API provides endpoints for authentication, posts, comments, users, and social interactions.

## Base URL

```
http://localhost:3000/api
```

## Authentication

All protected endpoints require JWT token in the Authorization header:

```
Authorization: Bearer <your_jwt_token>
```

## Error Response

```json
{
  "statusCode": 400,
  "message": "Error description",
  "error": "Bad Request"
}
```

## Endpoints

### Authentication

#### Register
```
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword",
  "name": "John Doe"
}
```

Response:
```json
{
  "id": "user_id",
  "email": "user@example.com",
  "name": "John Doe",
  "token": "jwt_token"
}
```

#### Login
```
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword"
}
```

### Users

#### Get Profile
```
GET /users/:userId
Authorization: Bearer <token>
```

#### Update Profile
```
PATCH /users/:userId
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "New Name",
  "bio": "User bio",
  "avatar": "image_url"
}
```

#### Follow User
```
POST /users/:userId/follow
Authorization: Bearer <token>
```

#### Unfollow User
```
DELETE /users/:userId/follow
Authorization: Bearer <token>
```

### Posts

#### Create Post
```
POST /posts
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Post Title",
  "content": "Post content",
  "image": "image_url",
  "category": "technology",
  "tags": ["tag1", "tag2"]
}
```

#### Get All Posts
```
GET /posts?page=1&limit=10
Authorization: Bearer <token>
```

#### Get Post by ID
```
GET /posts/:postId
Authorization: Bearer <token>
```

#### Update Post
```
PATCH /posts/:postId
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Updated Title",
  "content": "Updated content"
}
```

#### Delete Post
```
DELETE /posts/:postId
Authorization: Bearer <token>
```

#### Like Post
```
POST /posts/:postId/like
Authorization: Bearer <token>
```

#### Unlike Post
```
DELETE /posts/:postId/like
Authorization: Bearer <token>
```

### Comments

#### Create Comment
```
POST /posts/:postId/comments
Authorization: Bearer <token>
Content-Type: application/json

{
  "content": "Comment text"
}
```

#### Get Comments
```
GET /posts/:postId/comments?page=1&limit=10
Authorization: Bearer <token>
```

#### Update Comment
```
PATCH /comments/:commentId
Authorization: Bearer <token>
Content-Type: application/json

{
  "content": "Updated comment"
}
```

#### Delete Comment
```
DELETE /comments/:commentId
Authorization: Bearer <token>
```

### Feed

#### Get Feed
```
GET /feed?page=1&limit=20
Authorization: Bearer <token>
```

#### Get Trending Posts
```
GET /posts/trending?limit=10
Authorization: Bearer <token>
```

## Status Codes

- `200` - OK
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Internal Server Error

## Rate Limiting

API requests are limited to 100 requests per 15 minutes per IP address.

## CORS

Frontend requests are allowed from configured origins in `.env`:
```
CORS_ORIGIN="http://localhost:5173"
```

## Pagination

Most list endpoints support pagination:
- `page` - Page number (default: 1)
- `limit` - Items per page (default: 10)

Response includes:
```json
{
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 100
  }
}
```

## Testing API

Use Postman, Insomnia, or curl to test endpoints:

```bash
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password"}'
```

## Need More Info?

Check `/server/src` for detailed implementation or open an issue.
