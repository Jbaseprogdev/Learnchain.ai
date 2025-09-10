# LearnChain.ai — Product Requirements Document (PRD)

## 1. Vision
LearnChain.ai transforms education into a peer-driven, wisdom-centric ecosystem where students create modular knowledge blocks, connect them into learning chains, and share them with peers. Lessons are validated by AI, owned by their creators, and secured with verifiable Proof-of-Learning (PoL) credentials.

## 2. Objectives & Success Metrics (Year 1)
- **Create**: 50k+ published knowledge blocks with >90% AI quality score.
- **Learn**: 100k+ active learners; median session length >10 minutes.
- **Teach**: 25k+ student-teachers with at least one block used by peers.
- **Verify**: 200k+ PoL records anchored on-chain with <2s issuance latency.
- **Engage**: Average block rating ≥4.3/5 with ≥30% peer review coverage.
- **Retain**: 30-day retention ≥35% for creators, ≥25% for learners.

## 3. Core Concepts
- **Knowledge Blocks**: Student-created micro-lessons (text, video, interactive widgets). AI validates clarity, accuracy, age-appropriateness, and bias.
- **Learning Chains**: Adaptive sequences linking blocks into personalized micro-courses; progress-aware and competency-aligned.
- **Proof-of-Learning (PoL)**: Tokenized credentials recording creation, mastery, and impact; cryptographically hashed to a public blockchain (Polygon/EVM).
- **Peer-to-Peer Teaching**: Age-level peers teach peers; social and collaborative by design.
- **AI Validation & Mentorship**: AI validates, enhances content, and guides student-teachers with actionable feedback.
- **Wisdom Economy**: Students earn reputation, tokens, and opportunities for meaningful contributions.

## 4. Users & Roles
- **Student**: Create blocks, learn through chains, rate and review, earn PoL.
- **Mentor**: Curate, validate edge cases, offer feedback, host sessions.
- **AI Agent**: Validate content, recommend improvements, personalize chains, mentor.
- **Institution/Employer (viewer)**: Verify PoL, browse portfolios, discover talent.

## 5. Differentiators
- **Decentralized Ownership**: Blocks and PoL are user-owned; exportable and verifiable.
- **Age-Level Wisdom**: Peer-to-peer pedagogy among similar age/level encourages leadership.
- **Dynamic Validation**: Continuous AI checks (facts, clarity, bias, safety, sources).
- **Gamified Economy**: Reputation, ratings, streaks, and tokens drive participation.
- **Global Marketplace**: Worldwide exchange of knowledge blocks and chains.

## 6. Feature Scope
### 6.1 MVP (Year 1)
- **Authentication & Profiles** (Supabase Auth): JWT-based login; profile (name, age-band, avatar, bio, wallet address, skills/interests).
- **Knowledge Block Creator**: Rich editor with AI assistance for structure, examples, visuals, quizzes; media upload; block preview; versioning.
- **Chain Builder**: Drag-and-drop interface; branching; prerequisite tags; adaptive recommendations.
- **AI Validation**: Real-time checks for accuracy, clarity, toxicity/safety, age fit; citation enforcement; feedback loop.
- **Peer Rating & Review**: 1–5 rating, comments, tags; reputation weighting.
- **Proof-of-Learning Ledger**: PostgreSQL ledger with content + assessment proofs; on-chain hash anchoring on Polygon.
- **Publish/View**: Public block pages; embeddable viewers; search and discovery by topic/age/level.

### 6.2 Phase 2 (Year 2–3)
- **Peer-to-Peer Classrooms**: Live rooms, co-learning sessions, whiteboards.
- **Wisdom Tokens**: Recognition/rewards; staking pools for curation.
- **Knowledge Marketplace**: Listing, bundles, tipping; Stripe integration.
- **AI Mentor Avatars**: Persona-based mentors with domain expertise.
- **Collaborative Block Building**: Real-time co-editing and suggestions.

### 6.3 Phase 3 (Year 4–5)
- **Immersive Metaverse Classrooms**: 3D shared spaces; voice; spatial boards.
- **Knowledge DAO**: Governance for curation, grants, and policy.
- **Cross-Age Mentorship**: High-school mentors guiding younger students.
- **Global Wisdom Economy**: Expanded tokenomics and partner ecosystems.

## 7. Functional Requirements (MVP)
1. **User Accounts & Profiles**
   - Sign up/in with email/password, OAuth (Google/Apple), and wallet linking.
   - Profile editing with skills/interests taxonomy; privacy controls.
2. **Create Knowledge Blocks**
   - Editor supports text, images, video, code snippets, quizzes, and interactive embeds.
   - AI assistant provides structure, examples, and clarity improvements.
   - Version history and draft autosave.
3. **Validate with AI**
   - Submit block for validation; receive scores (accuracy, clarity, safety) and actionable suggestions.
   - Must pass threshold scores or provide citations to publish.
4. **Publish & Discover**
   - Public block page with metadata, tags, ratings, author, and version.
   - Search/filter by topic, age-band, level, language, and rating.
5. **Build Chains**
   - Compose sequences; drag blocks; define prerequisites and branching logic.
   - Adaptive suggestions based on learner profile and outcomes.
6. **Proof-of-Learning**
   - Record creation/mast 3; issue PoL entries with content hash, assessment data, and signatures.
   - Anchor to Polygon via merkle root or per-record hash; store tx id.
7. **Rate & Review**
   - Rate 1–5, comment; display aggregate rating; anti-spam throttling; reputation-weighted scores.
8. **Security & Privacy**
   - Supabase RLS; JWT; age-appropriate privacy defaults; content moderation.
9. **Observability**
   - Sentry for errors, LogRocket for sessions, Supabase logs for DB.

## 8. Non-Functional Requirements
- **Performance**: p95 API <300ms (read), <500ms (write); AI validation <4s end-to-end.
- **Availability**: 99.9% uptime; graceful degradation of AI/blockchain.
- **Scalability**: Support 1M MAU; horizontal scaling via Vercel/Supabase.
- **Security**: SOC2-ready practices; encryption at rest/in transit; least-privilege RLS.
- **Compliance**: COPPA/GDPR-friendly; parental consent for under 13 where required.
- **Internationalization**: i18n-ready; Unicode, RTL, locale/date formats.
- **Accessibility**: WCAG 2.1 AA; keyboard and screen-reader support.

## 9. Data Model (MVP)
- **users**: `id`, `role`, `display_name`, `age_band`, `bio`, `skills`, `interests`, `wallet_address`, `created_at`.
- **knowledge_blocks**: `id`, `title`, `content`, `type`, `tags`, `language`, `created_by`, `ai_status`, `ai_scores`, `version`, `published`, `created_at`, `updated_at`.
- **chains**: `id`, `title`, `description`, `blocks` (ordered array of block ids + branching metadata), `created_by`, `created_at`, `updated_at`.
- **ratings**: `id`, `block_id`, `user_id`, `score`, `comment`, `created_at`.
- **pol_ledger**: `id`, `subject_user_id`, `block_or_chain_id`, `kind` (creation|mastery|teaching), `content_hash`, `assessment_data`, `tx_hash`, `created_at`.
- **follows**: `follower_id`, `followed_id`, `created_at`.

## 10. AI Validation & Mentorship (Initial Policy)
- **Accuracy**: Require citations for non-original claims; cross-check with web search APIs.
- **Clarity**: Grade reading level, structure, examples; suggest tighter phrasing and visuals.
- **Safety**: Prevent harmful, explicit, or age-inappropriate content; detect bias.
- **Feedback Loop**: Inline suggestions; single-click apply; iterative re-check.
- **Mentorship**: Persona mentor offers scaffolding and project ideas tailored to age/level.

## 11. Gamification & Reputation (MVP)
- **Reputation Score**: Derived from ratings, reviews, reuse in chains, completion rates.
- **Streaks & Badges**: Creation streaks; badges for milestones (first publish, 100 ratings, etc.).
- **Leaderboards**: Topic- and age-band leaderboards with anti-gaming safeguards.

## 12. Proof-of-Learning (PoL) Design
- **Off-chain First, On-chain Anchor**: Store detailed proof in Postgres; anchor content hash (SHA-256) to Polygon. Keep gas minimal via batched merkle roots.
- **Verification**: Public verifier endpoint returns proof, merkle path, and on-chain tx. Portable JSON credential compliant with W3C VC where possible.
- **Privacy**: Hashes and minimal metadata on-chain; PII stays off-chain with consent controls.

## 13. Risks & Mitigations
- **Quality & Misinformation**: Strict AI validation thresholds; community flags; mentor overrides.
- **Underage Safety**: Parental consent flows; limited social graphs for <13; strong moderation.
- **Gaming the System**: Reputation-weighted ratings; anti-Sybil checks; velocity limits.
- **Cost Control**: Batch AI and on-chain operations; cache results; tiered usage.
- **Regulatory**: COPPA/GDPR readiness; data minimization; export/delete tooling.

## 14. Analytics & KPIs
- DAU/WAU/MAU; creator vs learner split
- Block publish rate and validation acceptance rate
- Median AI validation time
- Chain completion rates and drop-off points
- PoL issuance volume and verification requests
- NPS and support tickets

## 15. Launch Plan (MVP)
- **Private Alpha (Month 3)**: 300 students, 20 mentors; STEM focus.
- **Closed Beta (Month 5)**: 5k users; add marketplace waitlist; publish API docs.
- **Public Launch (Month 8)**: 50k users; school partnerships; educator program.

## 16. Taglines
- "Students Own the Future of Knowledge."
- "Chain Learning. Grow Wisdom."
- "From Learners to Leaders." 