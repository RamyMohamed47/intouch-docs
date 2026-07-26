# System Architecture

## High-Level Overview

Global User

↓

Organizations (Tenants)

↓

Categories

↓

Channels

↓

Messages

---

## Multi-Tenancy Strategy

Strategy:

Single Database

Shared Collections

Tenant Isolation using organizationId.

Every organization owns its own:

- Categories
- Channels
- Chats
- Messages

Users exist globally.

---

## Technology Stack

Backend

- Node.js
- Express.js
- MongoDB
- Socket.IO

Authentication

- JWT
- Refresh Tokens
- Google OAuth

Deployment

- Railway

Future Infrastructure

- Redis
- BullMQ
- Cloudinary

## Folder Architecture

src/

├── api/
│   ├── controllers/
│   ├── routes/
│   └── middleware/
│
├── socket/
│   ├── handlers/
│   └── events/
│
├── services/
│
├── repositories/
│
├── models/
│
├── utils/
│
├── config/
│
└── app.ts