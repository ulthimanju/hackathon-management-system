## Hackathon Management App – Presenter’s Brief

### 1. Elevator Pitch
A full-stack platform to create, manage, and participate in hackathons: streamlined registration, team formation (manual or auto), scheduling, submissions, judging, and results—plus external hackathon discovery (Devpost integration).

### 2. Problem
Organizers struggle with fragmented tools (forms, spreadsheets, email). Participants lack clarity on status, teams, deadlines, and submissions.

### 3. Solution
Unified workflow: lifecycle-driven hackathon entity (draft → upcoming → registration-open → active → judging → completed), structured registrations, team logistics, and submission pipeline with status visibility.

### 4. Primary User Roles
- Guest: Browse public hackathons
- Participant: Register, join/leave teams, submit projects
- Organizer: Create/manage hackathons, approve registrations, form teams, manage schedule, oversee submissions
- Admin: Role management, audit oversight, system governance

### 5. Core Features
- Hackathon CRUD + status transitions
- Registration with approval + deadline logic
- Dynamic Pill indicators (status, urgency, location, venue, registration time left)
- Team management (auto-form option, leader assignment)
- Submission handling (project metadata, links, files placeholders)
- Scheduling (events with times and types)
- Devpost hackathon listing integration
- Consistent UI primitives: Card, Pill, Modal, Button
- Responsive dark-themed layout

### 6. Hackathon Lifecycle (Business Logic)
draft → upcoming → registration-open → active → (judging) → completed  
Branch-out: cancelled (recoverable to draft/upcoming).  
Guards based on dates (startDate, endDate, registrationDeadline).

### 7. Registration Flow
Participant applies → (optional approval) → status: pending / approved / rejected / cancelled → attendance flag post-event.

### 8. Team Flow
- Organizer enables autoFormTeams OR manual creation
- Constraints: minTeamSize / maxTeamSize
- Leader role + membership array
- Submission tied 1:1 to team

### 9. Submission Model (Conceptual)
Project metadata (name, description, tech stack, repo/demo links, media, scoring fields, judge comments placeholder).

### 10. UI Layer Highlights
- HackathonCard: status pill, start/end dates, venue pill, registration urgency pill
- Details Modal (refactored): modular sections (Timeline, Location, Participation, Prizes, Schedule, Organizer)
- DevpostHackathons page: external discovery grid with urgency pills

### 11. Architecture (Implied)
- Frontend: React + custom component library (Card, Pill, etc.)
- Backend (assumed): Node/Express + MongoDB (collections: users, hackathons, registrations, teams, submissions, audit logs)
- Auth: Google OAuth → JWT session
- External API: Devpost fetch (server proxy or backend service)

### 12. Data Model (Key Entities)
User, AcademicProfile, Hackathon, Location, ScheduleItem, Registration, Team, Submission, AdminAuditLog.

### 13. Status & Visual Semantics (Pill Variants)
- success: active / healthy / open
- warning: approaching deadline / judging / pending
- danger: closed / cancelled / expired / rejected
- info: completed / venue / neutral tags
- primary: thematic tags (theme, prizes)
- default: baseline / draft

### 14. Security & Access Control
- Role arrays per user (admin, organizer, participant)
- Guarded transitions (only organizers/admins mutate lifecycle)
- Registration actions constrained by status + deadline
- Teams restricted by capacity & approval state

### 15. Scalability Considerations
- Separate collections prevent document bloat
- Index candidates: hackathon.status, registration.hackathonId, team.hackathonId
- Potential queue (future) for bulk emails (notifications, reminders)

### 16. Extensibility Roadmap
- Judging rubric & weighted scoring
- Certificate generation
- Live leaderboard
- Team matchmaking algorithm (skills + interests)
- Messaging/announcements channel
- Analytics dashboard (conversion, churn, registration funnel)

### 17. Demo Script (Suggested Flow ~6–8 min)
1. Landing (browse hackathons) – show status pills & urgency
2. Open HackathonCard → View Details modal
3. Register as participant (show pending → approved state)
4. Form or auto-form teams (show constraints)
5. Submit a mock project (highlight fields)
6. Transition hackathon to judging → completed
7. Show Devpost integration page
8. Admin view (brief): role management or audit mention
9. Wrap with value recap (clarity, automation, extensibility)

### 18. Value Proposition
- Reduces organizer manual overhead
- Gives participants transparent status + clear deadlines
- Modular, theme-consistent UI (fast iteration)
- Foundation for future scoring/analytics extensions

### 19. Deployment
This application can be deployed to the cloud for free using:
- **Frontend**: Vercel (free tier)
- **Backend**: Render (free tier)
- **Database**: MongoDB Atlas (free tier)

See `DEPLOYMENT.md` for detailed step-by-step deployment instructions, or `DEPLOYMENT_QUICK_REF.md` for a quick reference guide.

### 20. Closing Statement
A lifecycle-centric platform unifying hackathon logistics, participant experience, and extensibility—positioned to evolve into a full competition and innovation management suite.
