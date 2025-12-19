# GraphScheduler - Course Scheduling with Graph Coloring

## Overview

GraphScheduler is a university course scheduling application that uses graph coloring algorithms to solve scheduling and resource allocation problems. The system models courses as graph nodes and conflicts (same professor, same room, same students, overlapping times, prerequisites) as edges, then applies greedy graph coloring to assign optimal time slots and rooms without conflicts.

The core problem solved: assign minimum colors (time slots) to course nodes such that no two adjacent nodes (conflicting courses) share the same color, then dynamically allocate rooms to avoid double-booking.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight client-side routing)
- **State Management**: TanStack React Query for server state, local React state for UI
- **Styling**: Tailwind CSS with CSS variables for theming (light/dark mode support)
- **Component Library**: Shadcn/ui (Radix UI primitives with custom styling)
- **Build Tool**: Vite with React plugin

The frontend follows a component-based architecture with:
- Feature components in `client/src/components/` (CourseForm, ConflictForm, GraphVisualization, etc.)
- Reusable UI primitives in `client/src/components/ui/`
- Custom hooks in `client/src/hooks/`
- Shared utilities in `client/src/lib/`

### Backend Architecture
- **Framework**: Express.js with TypeScript
- **API Pattern**: RESTful JSON API under `/api/*` prefix
- **Storage**: In-memory storage with interface abstraction (`IStorage`) for easy database migration
- **Schema Validation**: Zod schemas shared between client and server

Key backend modules:
- `server/routes.ts`: API endpoint definitions for courses, conflicts, rooms, and schedule generation
- `server/graph-coloring.ts`: Core algorithm implementing greedy graph coloring with adjacency lists
- `server/storage.ts`: Data persistence layer with `MemStorage` implementation
- `server/color-utils.ts`: Time slot color assignment utilities

### Graph Coloring Algorithm
The scheduling algorithm uses a greedy coloring approach:
1. Build adjacency list from courses and conflicts
2. Sort courses by degree (number of conflicts) in descending order
3. Assign minimum available color (time slot) to each course that doesn't conflict with neighbors
4. Dynamically allocate rooms based on time slot assignments

### Data Models
Defined in `shared/schema.ts` with Zod validation:
- **Course**: id, code, name, professor, students array, preferred room
- **Conflict**: links two courses with conflict type (same_professor, same_room, same_students, same_timeslot, prerequisite)
- **Room**: id, name, capacity
- **ScheduleResult**: array of assignments with courseId, timeSlot, roomId, color

### API Endpoints
- `GET/POST /api/courses` - List/create courses
- `GET/PATCH/DELETE /api/courses/:id` - Course CRUD
- `GET/POST /api/conflicts` - Conflict management
- `GET/POST /api/rooms` - Room management
- `POST /api/schedule/generate` - Run graph coloring algorithm
- `DELETE /api/schedule/reset` - Clear all data

## External Dependencies

### Database
- **Drizzle ORM** configured for PostgreSQL (via `drizzle.config.ts`)
- Currently using in-memory storage; database schema ready for migration
- `drizzle-zod` for schema-to-validation integration

### UI Component Dependencies
- **Radix UI**: Full primitive set (dialog, select, tabs, toast, tooltip, etc.)
- **Shadcn/ui**: Pre-configured component styling with class-variance-authority
- **Lucide React**: Icon library
- **React Hook Form**: Form state management with Zod resolver
- **Embla Carousel**: Carousel component support

### Build & Development
- **Vite**: Development server with HMR
- **esbuild**: Production bundling for server
- **TypeScript**: Strict mode enabled with path aliases (@/, @shared/)
- **Tailwind CSS**: Utility-first styling with custom theme configuration

### Session & Security
- `connect-pg-simple`: PostgreSQL session store (for future auth)
- `express-session`: Session middleware infrastructure

### Replit-Specific
- `@replit/vite-plugin-runtime-error-modal`: Error overlay in development
- `@replit/vite-plugin-cartographer`: Development tooling
- `@replit/vite-plugin-dev-banner`: Development environment indicator