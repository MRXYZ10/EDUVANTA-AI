EduVanta — AI-Powered Learning Platform
> \*\*Learn smarter. Practice adaptively. Understand your mistakes.\*\*
>
> EduVanta is an AI-powered learning platform built around \*learning intelligence\*: helping learners understand what they know, identify gaps, and decide what to study next.
🌐 Website: edu-vanta.in  
🚀 Live app: EduVanta on Vercel
---
Contents
What is EduVanta?
Features
How the learning loop works
Technology stack
Application routes
Getting started
Environment variables
Database setup
Useful commands
Architecture and documentation
Security and responsible use
Deployment checklist
Project status
What is EduVanta?
Most learning platforms follow a simple pattern: course → content → test → score.
EduVanta aims to make learning more responsive:
Learn → Practice → Understand mistakes → Measure mastery → Adapt → Improve
The platform combines adaptive practice, an AI tutor named Nova, study planning, exam simulation, course-material retrieval, and progress analytics in one experience.
Features
🧠 Nova AI Tutor
Conversational AI tutoring with persisted conversation history.
Uses the learner's current topic and relevant uploaded learning material when available.
Supports configurable AI providers.
Includes retry-friendly error handling so a failed response does not silently lose the learner's message.
Applies usage quotas and rate limits to AI-powered operations.
🎯 Adaptive practice
Topic-based question practice with adaptive difficulty.
Learners can continue practising instead of being restricted to a short fixed session.
Server-side answer checking; the client does not decide whether an answer is correct.
Feedback and mistake analysis after submissions.
Keyboard support: `1`–`4` to choose an answer and `Enter` to submit/continue.
Validated question-generation workflow when a configured AI provider is available, with safe fallback behaviour.
📚 Learning Hub and course materials
Topic lesson pages with core ideas, examples, checklists, and direct practice actions.
Course catalogue and enrolment flows.
Upload PDF, TXT, and Markdown learning material (up to 15 MB per file).
Text extraction, chunking, embeddings, and relevance-filtered retrieval to ground Nova's answers in uploaded material when relevant.
Material deletion permissions for the uploader and authorised staff.
🗓️ Personal study planner
Onboarding captures learning goals, subjects, level, study time, and exam target when applicable.
Creates a study plan from the learner's selected subjects and topics.
Prioritises weak or unattempted topics and schedules revision.
Reschedules missed sessions based on priority rather than endlessly pushing every missed task forward.
📝 Exam simulator
Timed exams with question navigation and mark-for-review.
Answers are saved as the learner progresses.
In-progress exams can resume after a refresh using server-stored state and deadlines.
Results include score and topic/difficulty breakdowns.
A report highlights patterns that may have cost marks; numerical scoring is calculated by application logic, not invented by the AI.
🧩 Mistake Vault and mastery tracking
Stores recurring mistakes and groups them by concept.
Tracks mistake occurrence and resolution.
Offers a targeted mini-lesson and additional practice for recurring mistakes.
Mastery combines signals such as accuracy, recency, difficulty, consistency, and repeated mistakes rather than relying only on a raw correct-answer percentage.
Exam readiness is an internal learning indicator, not a guarantee of exam results.
👩‍🏫 Teacher tools
Teacher dashboard with class-level topic mastery insights.
Identifies class-wide weak topics using group-level signals rather than reacting to one student's result.
Creates assignments within courses the teacher owns.
Generates remedial practice through the same question validation pipeline used elsewhere in the app.
📈 Progress, focus, and engagement
Dashboard with study activity, recommendations, mastery, and learning progress.
Analytics and profile pages.
Focus Mode with a session summary.
Daily streaks, achievement signals, and in-app notifications with duplicate prevention.
🛡️ Admin and platform operations
Role-based student, teacher, and admin access.
Admin views for users, learning content, payments, and system-health signals.
Package and payment flows with manual UPI/UTR verification and an admin review step.
Audit records for selected sensitive actions, including payment decisions and assignment creation.
Health endpoint and production-oriented security/rate-limit controls.
> Feature availability depends on correct environment configuration, database migrations, AI provider credentials, and role permissions.
How the learning loop works
Set a goal — complete onboarding and choose subjects, target, and study availability.
Learn a concept — use topic lessons and, where useful, uploaded course material.
Practise — answer questions whose difficulty can respond to recent performance.
Review mistakes — inspect feedback and revisit recurring concepts in the Mistake Vault.
Measure progress — mastery, streaks, analytics, and exam reports provide feedback.
Adapt the plan — use recommendations and the planner to focus on the next highest-value topic.
Technology stack
Area	Technology
Application	Next.js 14 App Router, React 18, TypeScript
Styling and UI	Tailwind CSS, Lucide icons
Authentication	Supabase Auth with `@supabase/ssr`
Database	PostgreSQL
ORM and migrations	Prisma 5
AI providers	Anthropic, OpenAI, Gemini; Groq support is also represented in environment configuration
Retrieval	Text chunking, embeddings, PostgreSQL `pgvector` retrieval, lexical fallback
Validation	Zod
Visualisations	Recharts
Maths and content rendering	KaTeX, React Markdown, remark/rehype plugins
Testing	Vitest
Hosting	Vercel or another compatible Node.js host
Application routes
Route	Purpose
`/`	Entry point and onboarding-aware landing flow
`/onboarding`	Learner setup and initial study plan
`/dashboard`	Learning overview and recommendations
`/learn`	Enrolled learning content
`/learn/catalog`	Course catalogue
`/learn/\[topicId]`	Topic lesson
`/practice`	Practice hub

`/practice/\[topicId]`	Topic-based adaptive practice
`/tutor`	Nova AI Tutor
`/planner`	Study planning and rescheduling
`/exam/\[examId]`	Timed exam experience
`/exam/result/\[examAttemptId]`	Exam report
`/focus`	Focus Mode
`/mistakes`	Mistake Vault
`/materials`	Uploaded learning materials
`/analytics`	Learning analytics
`/profile`	Learner profile
`/packages`	Plans and payment flow
`/teacher`	Teacher tools
`/admin`	Admin dashboard
`/admin/payments`	Payment review tools
`/login`, `/signup`	Authentication
`/forgot-password`, `/reset-password`	Password recovery
Getting started
Prerequisites
Node.js 18.18 or newer
npm
A PostgreSQL database
A Supabase project for authentication
At least one supported AI provider key for real AI responses
1. Install dependencies
```bash
npm install
```
2. Configure environment variables
```bash
cp .env.example .env
```
On Windows PowerShell:
```powershell
Copy-Item .env.example .env
```
Fill in the values for your own development environment. Do not commit `.env` or paste secret keys into issues, screenshots, or public repositories.
3. Configure Supabase Auth
Create or open your Supabase project.
Enable Email authentication.
Add your local and production callback URLs in Supabase Authentication URL Configuration.
Set the Supabase URL and anon key in `.env`.
4. Prepare the database
Ensure PostgreSQL is reachable and the `vector` extension is available if you want pgvector-based material retrieval. Then run:
```bash
npx prisma generate
npx prisma migrate deploy
```
For a brand-new local database where you are actively developing schema changes, use the Prisma workflow appropriate to that environment. Do not reset or push schema changes to a production database without reviewing the migration plan.
Optional local/staging seed:
```bash
npm run db:seed
```
Seeded records are intended for development/staging; review the seed script before using it with any database containing real user data.
5. Start the development server
```bash
npm run dev
```
Open http://localhost:3000.
6. Configure AI
Set `AI\_PROVIDER` to the provider you want to use and add its corresponding key. The project includes a development mock fallback when provider credentials are absent in development; production should have valid credentials configured.
For semantic retrieval of uploaded material, configure `OPENAI\_API\_KEY` for embeddings. If embeddings are unavailable, the application has a local lexical fallback, so retrieval behaviour may differ.
Environment variables
Use `.env.example` as the source of truth; it documents the variables currently supported by the codebase.
Variable	Purpose
`DATABASE\_URL`	PostgreSQL connection used by the application
`DIRECT\_URL`	Direct PostgreSQL connection for migration/admin operations where needed
`NEXT\_PUBLIC\_SUPABASE\_URL`	Supabase project URL
`NEXT\_PUBLIC\_SUPABASE\_ANON\_KEY`	Supabase public/anon key
`AUTH\_SECRET`	Application authentication-related secret
`AI\_PROVIDER`	Selects the AI provider
`ANTHROPIC\_API\_KEY`	Anthropic API access
`OPENAI\_API\_KEY`	OpenAI access; also used for embeddings when configured
`GEMINI\_API\_KEY`	Gemini API access
`GROQ\_API\_KEY`	Groq credentials where enabled by the configured provider implementation
`NEXT\_PUBLIC\_EDUVANTA\_UPI\_ID`	UPI ID displayed in the payment flow
`CRON\_SECRET`	Protects scheduled notification endpoint access
`UPSTASH\_REDIS\_REST\_URL`	Shared Redis endpoint for distributed rate limiting
`UPSTASH\_REDIS\_REST\_TOKEN`	Shared Redis authentication token
`NOVA\_FREE\_TOKENS`, `NOVA\_STARTER\_TOKENS`, `NOVA\_PRO\_TOKENS`, `NOVA\_PREMIUM\_TOKENS`	Nova token quota settings
`CSP\_MODE`	Controls the Content Security Policy rollout mode
`DEV\_UNLOCK\_PASSWORD`, `DEV\_MEGA\_UNLOCK\_PASSWORD`	Optional developer-only unlock controls; keep unset in production
Some settings are optional or only relevant to specific features. Check comments in `.env.example` and the deployment guide before configuring production.
Database setup
EduVanta uses Prisma migrations to evolve its PostgreSQL schema. The repository includes migrations for learning data, packages/payments, exam curriculum, mastery snapshots, performance indexes, Nova billing, material content hashes, payment credits, audit logs, and related features.
Typical production migration flow:
```bash
npx prisma generate
npx prisma migrate deploy
```
Do not use `prisma migrate reset` on a production database. Review new migrations and back up important data before applying schema changes.
Useful commands
Command	Description
`npm run dev`	Start the local development server
`npm run build`	Create a production build
`npm run start`	Serve the production build
`npm run lint`	Run ESLint
`npm run typecheck`	Run TypeScript checks
`npm test`	Run the Vitest test suite
`npm run verify`	Run lint, typecheck, tests, and build
`npm run db:generate`	Generate Prisma Client
`npm run db:migrate`	Run the development migration workflow
`npm run db:seed`	Seed development/staging data
`npm run db:studio`	Open Prisma Studio
`npm run db:fill-bank`	Populate supported built-in question banks
Before releasing, run:
```bash
npm ci
npx prisma generate
npx prisma migrate deploy
npm run verify
```
Run these commands in a normal networked development environment or CI. A successful local build should be confirmed in the actual deployment environment as well.
Architecture and documentation
The repository contains additional implementation and operations documentation:
`ARCHITECTURE.md` — system design and core flows
`API.md` — API route reference
`DATABASE.md` — database model and data relationships
`DEPLOYMENT.md` — environment and deployment guidance
`SECURITY.md` — authentication, authorisation, validation, and security considerations
`AUDIT.md` — audit and review notes
`PRODUCTION\_HARDENING.md` — production hardening checklist
`PRODUCTION\_UPGRADE\_SUMMARY.md` — recent platform upgrades
`PAYMENT\_SETUP\_UPI.md` — UPI payment setup notes
`DEV\_MODE\_AND\_QUOTA.md` — development mode and Nova quota details
Security and responsible use
Supabase validates sessions; server routes enforce authentication and role/ownership checks.
API request bodies and generated structured content are validated before being trusted.
Correct answers are checked on the server; answer keys are not sent to learners during an active practice or exam.
AI operations have per-user rate limits. Configure Upstash Redis for shared rate limiting across serverless instances.
Uploaded files are type/size validated, and PDF parsing includes checks for risky embedded content; this is not a substitute for a full malware-scanning service.
Configure strong secrets, least-privilege access, and production-safe environment variables.
Never expose database URLs, service-role keys, AI keys, payment credentials, or developer unlock passwords.
Review the security and deployment documents before exposing a new public production deployment.
AI-generated explanations and recommendations can be wrong. Learners should verify important academic information against trusted course materials. Exam-readiness indicators are estimates, not promises of results.
Deployment checklist
[ ] Set production environment variables in the hosting provider.
[ ] Use production Supabase Auth callback/redirect URLs.
[ ] Confirm PostgreSQL connectivity and required extensions.
[ ] Apply reviewed migrations with `npx prisma migrate deploy`.
[ ] Configure a real AI provider and its API key.
[ ] Configure Upstash Redis for shared production rate limits.
[ ] Set `CRON\_SECRET` and verify the scheduled notification configuration.
[ ] Set the correct UPI ID only if enabling the payment flow.
[ ] Keep developer unlock passwords unset in production unless deliberately required and secured.
[ ] Run `npm run verify` in CI or a networked environment.
[ ] Smoke-test authentication, tutor, practice, exams, uploads, payment review, and role-protected routes.
[ ] Check hosting logs and `/api/health` after deployment.
Project status
EduVanta is an actively evolving early-stage product. This README describes the capabilities and configuration represented in the current repository; external providers, credentials, database migrations, scheduled jobs, and production settings still need to be configured and verified for each deployment.
---
Made with a focus on more personalised, evidence-informed learning.
