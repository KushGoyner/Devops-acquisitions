# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is a Node.js Express API for a DevOps acquisitions system, built with modern JavaScript (ES modules) and featuring authentication, database management with Drizzle ORM, and comprehensive logging.

## Development Commands

### Core Development
```bash
# Start development server with auto-reload
npm run dev

# Install dependencies
npm install
```

### Code Quality
```bash
# Run ESLint
npm run lint

# Fix ESLint issues automatically
npm run lint:fix

# Format code with Prettier
npm run format

# Check code formatting
npm run format:check
```

### Database Operations
```bash
# Generate database migrations
npm run db:generate

# Run database migrations
npm run db:migrate

# Open Drizzle Studio for database management
npm run db:studio
```

## Architecture Overview

### Project Structure
The project follows a modular MVC architecture with clear separation of concerns:

- **Entry Point**: `src/index.js` → `src/server.js` → `src/app.js`
- **Path Aliases**: Uses import maps for clean imports (e.g., `#config/*`, `#controllers/*`)
- **Database**: PostgreSQL with Neon serverless, managed via Drizzle ORM
- **Authentication**: JWT-based with bcrypt password hashing
- **Logging**: Winston logger with file and console transports

### Key Architectural Patterns

**Layered Architecture**:
- **Controllers** (`src/controllers/`): Handle HTTP requests/responses
- **Services** (`src/services/`): Business logic and data processing
- **Models** (`src/models/`): Database schema definitions (Drizzle)
- **Routes** (`src/routes/`): Express route definitions
- **Middleware**: Built-in Express middleware (helmet, cors, morgan, cookie-parser)

**Validation Strategy**:
- Zod schemas for request validation (`src/validations/`)
- Custom error formatting utilities (`src/utils/format.js`)

**Authentication Flow**:
- JWT tokens stored in HTTP-only cookies
- Role-based access (user/admin roles)
- Password hashing with bcrypt (salt rounds: 10)

**Database Architecture**:
- Drizzle ORM with Neon PostgreSQL
- Schema-first approach with TypeScript-like type safety
- Migration system with SQL files in `drizzle/` directory

### Configuration Management

**Environment Variables** (`.env`):
- `PORT`: Server port (default: 3000)
- `NODE_ENV`: Environment (development/production)
- `DATABASE_URL`: PostgreSQL connection string
- `JWT_SECRET`: JWT signing secret (has fallback but should be set)
- `LOG_LEVEL`: Winston logging level

**Import Aliases**:
The project uses Node.js import maps for clean module imports:
- `#config/*` → `./src/config/*`
- `#controllers/*` → `./src/controllers/*`
- `#middleware/*` → `./src/middleware/*`
- `#models/*` → `./src/models/*`
- `#routes/*` → `./src/routes/*`
- `#services/*` → `./src/services/*`
- `#utils/*` → `./src/utils/*`
- `#validations/*` → `./src/validations/*`

### Current Implementation Status

**Completed Features**:
- User registration (`/api/auth/sign-up`)
- Database connection and user model
- JWT authentication utilities
- Request validation with Zod
- Logging infrastructure
- Development tooling (ESLint, Prettier)

**API Endpoints**:
- `GET /` - Basic health check
- `GET /health` - Detailed health status with uptime
- `GET /api` - API status check
- `POST /api/auth/sign-up` - User registration

**Incomplete Features**:
- User sign-in endpoint (placeholder exists)
- User sign-out endpoint (placeholder exists)
- Authentication middleware
- Protected routes

### Development Notes

**Database Schema**:
Currently has a single `users` table with fields: id, name, email, password, role, created_at, updated_at.

**Security Considerations**:
- Passwords are hashed with bcrypt
- JWT tokens use HTTP-only cookies
- Helmet.js for security headers
- CORS enabled globally
- Input validation with Zod schemas

**Error Handling**:
- Centralized logging with Winston
- Custom error formatting for validation errors
- Try-catch blocks in controllers and services

**Code Style**:
- ES modules with Node.js import maps
- ESLint with strict rules (unix line endings, single quotes, 2-space indent)
- Prettier for consistent formatting
- Arrow functions preferred over function declarations

### Testing
Currently no test suite is configured. The `npm test` script shows "Error: no test specified".