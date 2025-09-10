# LearnChain.ai — Full-Stack Development Roadmap

## Architecture Overview
- **Frontend (Web)**: Next.js 14 (App Router), React 18, TypeScript, Tailwind CSS 4.x, ShadCN UI, Framer Motion, Zustand/Redux Toolkit.
- **Mobile**: React Native + Expo (EAS builds), shared UI primitives where practical.
- **Auth**: Supabase Auth (JWT, OAuth providers). Optional WalletConnect for EVM wallet linking.
- **Backend**: Supabase (PostgreSQL, RLS policies, Edge Functions, Realtime, Storage). Optional server middleware for AI orchestration.
- **API Layer**: REST-first with typed clients; GraphQL gateway in Phase 2. Edge Functions for AI validation and PoL issuance.
- **AI Services**: GPT-5 API for validation, mentor chat, block enhancement. Safety & citation checker pipeline.
- **Blockchain**: Polygon PoS/EVM for PoL anchoring. Contract minimalism: hash registry (Merkle root batching) in MVP.
- **Observability**: Sentry (frontend/backend), LogRocket (web), Supabase logs.
- **CI/CD**: GitHub Actions → Vercel (web) + EAS (mobile) + Supabase migrations.

## Data Model (Initial)
- `users(id, role, display_name, age_band, bio, skills, interests, wallet_address, created_at)`
- `knowledge_blocks(id, title, content, type, tags, language, created_by, ai_status, ai_scores, version, published, created_at, updated_at)`
- `chains(id, title, description, blocks, created_by, created_at, updated_at)`
- `ratings(id, block_id, user_id, score, comment, created_at)`
- `pol_ledger(id, subject_user_id, block_or_chain_id, kind, content_hash, assessment_data, tx_hash, created_at)`

## Repos & Packages
- `apps/web` (Next.js 14)
- `apps/mobile` (Expo)
- `packages/ui` (shared UI components; tailwind + shadcn)
- `packages/types` (TypeScript types; Zod schemas)
- `packages/api` (generated clients; SDK wrappers)
- `infra` (supabase migrations, RLS, edge functions, contracts)

## Phase 0 — Bootstrap (Week 0–1)
- Initialize monorepo (pnpm/turbo), TS config, linting (ESLint, Prettier), husky.
- Set up Vercel + Supabase projects; env management (Vercel/Supabase secrets).
- Create Supabase schema, RLS policies, seed scripts.
- Skeleton apps for web/mobile; CI pipeline skeleton.

## Phase 1 — MVP Foundations (Week 2–6)
- Auth: email/password + OAuth, profiles, wallet link UI; basic roles (`student`, `mentor`).
- Knowledge Block Creator: rich editor (MDX + upload), autosave drafts to Supabase, media via Supabase Storage.
- AI Validation (Edge Function): submit block → validate (accuracy, clarity, safety), scores + suggestions; queues + retries; caching.
- Publish/View: block pages, search/filter, tags, ratings CRUD with RLS.
- Chain Builder: drag-drop UI with ordered list; save chain structure; basic progress tracking.
- PoL Ledger: hashing (SHA-256), anchor via simple registry contract; store `tx_hash`.
- Observability: Sentry + LogRocket wiring; structured logging.

## Phase 2 — Social & Personalization (Week 7–12)
- Reputation model: weighted ratings, creator reputation score, anti-spam throttles.
- Recommendations: similar blocks/chains via embeddings + usage signals.
- Mentorship: AI mentor chat with context from a block/chain; prompt safety guardrails.
- Mobile app parity: create, view, rate; offline drafts for creator.
- GraphQL gateway (optional) for client-optimized queries.

## Phase 3 — Classrooms & Marketplace (Quarter 2)
- Live P2P classrooms (WebRTC): group sessions, whiteboard, attendance to PoL.
- Wisdom tokens: non-transferable rep points; tipping via Stripe; marketplace listings.
- Collaborative editing: real-time presence and conflict resolution (CRDT/Y.js) for blocks.

## Phase 4 — Governance & 3D (Year 2+)
- Knowledge DAO: proposal/voting for curation and grants.
- Tokenomics dashboard: issuance, reuse, reputation.
- Metaverse classrooms: Unity/Unreal bridge; wallet login; content ingestion.

## Security & Compliance
- Supabase RLS everywhere; least privilege; row owners only.
- Age-gated features; parental consent flows (<13) with verified contact method.
- PII minimization; encryption at rest/in transit; DSR endpoints.
- Abuse detection: content moderation queue; anomaly detection jobs.

## DevOps & Environments
- Envs: `dev`, `staging`, `prod`. Branch-based deploys via Vercel previews.
- GitHub Actions: test → typecheck → lint → build → e2e (Playwright) → deploy.
- Database migrations via Supabase CLI; migrate on merge to `main`.
- Feature flags via Supabase config table.

## API Surface (MVP)
- `POST /api/blocks` create; `GET /api/blocks/:id`; `GET /api/blocks?filter=...`
- `POST /api/blocks/:id/validate` triggers AI validation
- `POST /api/chains` create; `GET /api/chains/:id`
- `POST /api/ratings` create rating
- `POST /api/pol/issue` store ledger + anchor hash; `GET /api/pol/verify/:id`

## AI Validation Pipeline (MVP)
1. Normalize content → chunk if needed (MDX to plain).
2. Safety filter (toxicity, PII, age-inappropriate) → block if failed.
3. Accuracy/clarity scoring with GPT-5; expect citations for non-original claims.
4. Suggest edits; return deltas and structured `ai_scores`.
5. Cache results; allow re-validate on change with diff prompts.

## Smart Contract (MVP)
- Minimal `ProofOfLearningRegistry`:
  - `function anchor(bytes32 contentHash) returns (bytes32 txId)`
  - Optional Merkle batch: `function anchorRoot(bytes32 merkleRoot)`
- Deploy to Polygon PoS; store address in config.

## Milestones & Deliverables
- Week 3: Auth + Profiles + block drafts; first AI validation response.
- Week 6: Publish blocks; basic chains; ratings; PoL anchor; web launch on Vercel.
- Week 10: Recommendations + mobile parity; mentor chat beta.
- Quarter 2: Classrooms + marketplace; collaborative editing alpha.

## Monitoring & KPIs
- Time-to-first-block, validation acceptance rate, publish-to-view ratio.
- p95 API/edge latency; error rates; cache hit ratio; AI cost per block.
- PoL anchoring cost; verification volume; contract uptime.

## Documentation
- `/docs` for PRD, Context, Roadmap, API reference, and runbooks.
- Developer onboarding: `pnpm i`, env setup, Supabase CLI, local emulators. 