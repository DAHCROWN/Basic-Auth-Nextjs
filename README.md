# Authentication Proof-of-Concept (Next.js, JWT, Cookies)

This project demonstrates a minimal, secure authentication system using Next.js, TypeScript, and the `jose` library for JWT handling. The focus is on backend authentication logic, session management, and secure cookie practices.

## Key Authentication Features

- **JWT-Based Sessions**: User sessions are represented as JSON Web Tokens (JWTs), signed and verified using HMAC (HS256) via the `jose` library.
- **HTTP-Only Cookies**: Sessions are stored in HTTP-only cookies, protecting them from client-side JavaScript access and XSS attacks.
- **Short-Lived Sessions**: JWTs are issued with a short expiration (10 seconds, for demonstration), and can be refreshed on each request via middleware.
- **Login/Logout Flow**:
  - On login, a JWT is created with user info and expiry, then set as a cookie.
  - On logout, the session cookie is destroyed.
- **Session Refresh Middleware**: Every request passes through middleware that can transparently refresh the session’s expiry, keeping active users logged in.
- **No Passwords (Demo Only)**: For simplicity, the demo uses only an email field and a hardcoded user object. In production, you would verify credentials securely.

## Core Authentication Logic

- `lib.ts`: Contains all authentication logic:
  - `encrypt(payload)`: Signs and returns a JWT.
  - `decrypt(token)`: Verifies and decodes a JWT.
  - `login(formData)`: Authenticates user, creates session JWT, sets cookie.
  - `logout()`: Destroys session cookie.
  - `getSession()`: Reads and verifies session from cookie.
  - `updateSession(request)`: Refreshes session expiry and updates cookie.
- `middleware.ts`: Runs on every request, calling `updateSession` to keep sessions alive.

## Security Considerations

- **JWT Secret**: Uses a hardcoded secret for demo; use environment variables in production.
- **Cookie Security**: Cookies are HTTP-only and can be set to `secure` in production.
- **Session Expiry**: Short expiry demonstrates session invalidation; adjust as needed for real apps.
