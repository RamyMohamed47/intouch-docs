# InTouch Roadmap

## v1.0 - MVP

### Authentication
- [x] Register
- [x] Login
- [x] JWT
- [x] Refresh Tokens
- [x] Google OAuth

### Organizations
- [x] Create Organization
- [x] Invite Members
- [x] Join Organization

### Conversation
- [x] Categories
- [x] Public Channels
- [x] Private Channels

### Messaging
- [x] Real-time messaging
- [x] Direct Messages
- [x] Typing Indicator
- [x] Online Presence
- [x] Read Receipts

---

## v1.1

- [x] Emoji reactions
- [x] File uploads
- [x] Chat Wallpapers
- [x] Edit messages
- [x] Delete messages
- [x] Search
- [x] Notifications
- [x] Mail Service


---

## v2.0

Infrastructure improvements

- [x] Redis
		├── Presence
		 ├── Typing
		  ├── Socket.IO adapter
		   └── Distributed rate limiting (optional)
- [ ] BullMQ
		└── Async email/notification jobs
- [ ] Docker
		├── MongoDB
		 └── Redis
		  └── API/worker containers
- [x] Logging
- [ ] Monitoring
		├── Health checks
		 ├── Metrics
		   └── Error monitoring
- [x] Rate Limiting


---

## v3.0

Advanced Features

- [ ] Voice Channels
- [ ] Video Calls
- [ ] AI Assistant
- [ ] Mobile App
- [ ] Load testing