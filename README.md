# AAU Forum

AAU Forum is a real-time discussion platform built with Node.js, Express, MongoDB, and Socket.io. It was originally developed as a group project during the 2nd semester of Software Engineering at Aalborg University Copenhagen and was later continued independently with the group's permission. This repository contains the continued version.

The application provides posts, comments, polls, events, direct messages, notifications, profiles, and dark mode in a single system.

## Live Demo

A live version of the application is available at:

[https://aau-forum.onrender.com](https://aau-forum.onrender.com)

The demo is hosted on Render's free tier. If the instance has been inactive, the first request may take 20–30 seconds while it starts.

### Test Account

- **UserID:** `test`

The test account provides access to all features without registration. Content created with this account is public.

## Features

- Login with a unique UserID (no password required)
- Public user profiles with bio and activity overview
- Role selection (student semesters, professor, recruiter, and more)
- Posts with tags and comments
- Events with images and live attendance counts
- Polls with deadlines and previous poll history
- Real-time direct messaging
- Notification dropdown with clickable links
- Dark mode / light mode toggle
- Responsive design
- Image upload with validation
- Socket.io for live updates and notifications

## Technologies

- Node.js
- Express
- MongoDB (Mongoose)
- EJS (templating)
- Multer (file uploads)
- express-session (authentication)
- Socket.io (real-time features)
- Vanilla JavaScript and CSS

## Security

This project is a prototype, but security-relevant behaviour is handled explicitly in the following areas.

### Session handling

- Authentication uses `express-session` with a server-side session store. The browser only receives an opaque session cookie; user data is kept on the server.
- Session cookies are configured with `httpOnly` (inaccessible to client-side JavaScript), `sameSite: 'lax'` (mitigates cross-site request forgery), and a seven-day expiry.
- The session signing secret is supplied through the `SESSION_SECRET` environment variable and is never committed to the repository. A development fallback exists only to simplify local setup.
- Protected routes are guarded by a `requireLogin` middleware, and owner-only actions (deleting posts, events, and polls) verify ownership before making changes.
- Socket.io connections reuse the same Express session middleware. Conversation rooms can only be joined by authenticated users who are participants in the conversation.

### Input validation

- Request bodies are trimmed and checked for required fields before they reach the database.
- Mongoose schemas enforce required fields and length limits (for example, comments are capped at 1,000 characters and bios at 500).
- Uploaded images are restricted to JPEG, PNG, GIF, and WebP, capped at 5 MB, and stored with sanitised filenames. Uploaded files are served from a dedicated directory rather than executed.
- EJS templates escape rendered values by default, which prevents untrusted content from being interpreted as HTML.
- Invalid MongoDB identifiers are checked before queries, and malformed requests receive a generic error page instead of internal details.

### Known security limitations

This is not a hardened production deployment. Before real-world use, it should add CSRF tokens, login and request rate limiting, a persistent session store, security headers (for example via Helmet), and automated dependency and security testing. The UserID-only login is intentionally simplified for the demo and is not a substitute for password or federated authentication.

## Getting Started

### Prerequisites

Before running the project locally, make sure you have the following installed:

- **Node.js** (version 18 or later) – download from [https://nodejs.org](https://nodejs.org)
- **MongoDB** – either a local installation or a free cloud cluster from [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/Zaiim23/AAU-Forum.git
   ```

2. Navigate into the project folder:
   ```bash
   cd AAU-Forum
   ```

3. Install dependencies:
   ```bash
   npm install
   ```

4. Create a `.env` file in the project root with the following content:
   ```env
   MONGODB_URI=your_mongodb_connection_string
   SESSION_SECRET=replace-this-with-a-long-random-value
   PORT=3000
   ```

   Replace `your_mongodb_connection_string` with your own MongoDB connection string. A free connection string is available from [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).

5. Start the application:
   ```bash
   npm start
   ```

6. Open [http://localhost:3000](http://localhost:3000) in your browser.

For development with automatic restarts, use:
```bash
npm run dev
```

## Project Background

AAU Forum originated as a second-semester group project at Aalborg University Copenhagen. The project addressed a common problem: students had to use multiple platforms to stay informed about events, ask academic questions, and connect with peers, which made relevant information difficult to find.

The group researched the problem through surveys, interviews, and design sprints, then built a working forum as the semester project.

After the course ended, the project was continued independently with the group's permission. Group members are not named in this public repository.

## Independent Development

The later development of this repository includes the following changes:

- Restructured the project from a single folder into an MVC-like architecture
- Replaced password authentication with UserID-only login
- Added sessions and user authentication middleware
- Implemented public user profiles with bio and activity overview
- Added role selection with options for students, staff, and external partners
- Built a notification system with Socket.io and a clickable dropdown
- Added real-time messaging, comments, poll votes, and event attendance
- Implemented dark mode with persistent user preference
- Added image upload validation and default avatar fallback
- Added event and poll deletion for owners
- Fixed bugs discovered during testing
- Translated the user interface to English
- Documented the project for public use

## Privacy

Personal and confidential information has been removed. University reports, process analyses, private communication, and personal group information are not included. Collaborator names are omitted for privacy.

## Known Limitations

This is a prototype. Current limitations include:

- In-memory session store (sessions reset when the server restarts)
- Uploaded images are stored on the server filesystem and may not persist between deploys on free hosting
- No password recovery or email verification
- No moderation tools
- No persistent notification storage across sessions

## License

This project is released under the MIT License. See [LICENSE](LICENSE) for the full text.
