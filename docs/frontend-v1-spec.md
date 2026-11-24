# Top Vino frontend V1 specification

Status: local draft for review. Product/design direction approved in the planning interview; nothing has been implemented or deployed.

Planning session: 2026-10-31. Companion: [dependency-linked ticket drafts](./frontend-v1-tickets.md).

## Problem Statement

Students studying WSET Level 3 or Diploma theory need a focused way to create and organize study material, practise recall, and understand their progress and upcoming workload. The existing backend has some useful building blocks but no frontend, and its deployed-environment policies are currently designed for a disposable synthetic-data sandbox.

V1 is a personal, non-commercial, invite-only beta for 5-10 learners. It must retain their decks and progress. It is not an official examination marker, an entire course library, or a promise of improved examination results.

## Solution

V1 is a private flashcard app for invited learners. It runs online, with a phone-first interface that also works on desktop.

1. Accept an invitation and establish an email/password login.
2. Choose an original starter deck or create personal study material.
3. Organize decks into optional, flat Topics.
4. Review due cards and a small daily allowance of new cards.
5. Return later to retained progress, upcoming reviews, and clear activity summaries.

The frontend uses Next.js, TypeScript, TanStack Query, Tailwind, and selectively adopted shadcn/ui components. It deploys independently of the existing API. Vercel Hobby is the preferred frontend host while V1 remains eligible for personal, non-commercial use.

### Non-negotiable constraints

- Strict $0 operating budget, including domains. No automatic paid upgrades or overages.
- Free provider-issued hostnames; no purchased domain required for V1.
- Separate frontend/backend repositories and independently deployable releases.
- No real beta data in the disposable sandbox.
- Target at most 24 hours of data loss after a failure and restoration within 48 hours. These are recovery targets to validate, not a guaranteed free-service SLA.
- Cold-start delays and occasional unavailability are acceptable; silently lost or falsely acknowledged work is not.
- No application scaffold, provisioning, credentials, GitHub repository creation, or issue publication during this planning task.

## Full MVP and next milestone

The full MVP study loop is **recall → explain → revise**. V1 provides the recall step through spaced-repetition flashcards.

The next milestone adds checklist-based written practice, starting with a small set of original short-answer prompts on the starter deck’s topic. Learners write an explanation, compare it with a reviewed example answer and checklist, then revise their answer to add a missing point or clarify a weak link. Written practice runs separately from scheduled flashcard Reviews. The milestone succeeds when beta learners who return for due cards can also point to a concrete improvement in a revised answer.

### Deferred decisions

- **AI creation.** Whether to generate draft flashcards or practice prompts from learner-supplied materials.
- **AI feedback.** Whether to give targeted feedback on a written answer against its checklist.
- **Paid access.** Whether to charge for access and which features any paid access would cover.

## User Stories

1. As an invited learner, I want to establish my own password without receiving a shared or operator-known password, so that my account is personal.
2. As a learner, I want to sign in, return to my session, and sign out, so that access is convenient and controlled.
3. As a learner, I want self-service password-reset emails, so that losing my password does not permanently lock me out.
4. As a learner, I want to change my password and confirm my timezone, so that my account and study dates behave predictably.
5. As a learner, I want to create and rename Topics, so that I can organize my theory study.
6. As a learner, I want to move decks between a Topic and Ungrouped, and remove a Topic without deleting its decks, so that reorganizing is safe.
7. As a learner, I want to create, view, and edit my decks, so that I can build a personal library.
8. As a learner, I want to create and edit text prompt/answer cards, so that I can practise factual recall.
9. As a learner, I want to select text to hide in a Cloze Card and preview it, so that I can create valid fill-the-gap material without writing markup.
10. As a learner, I want to copy a reviewed starter deck into my library, so that I can begin studying immediately and customize my own copy.
11. As a learner, I want ordinary card edits to preserve scheduling and history, so that corrections do not erase my work.
12. As a learner, I want to archive and restore cards/decks, so that I can remove material from study without destroying its history.
13. As a learner, I want to study my whole library, a Topic, or one Deck through the same reviewer, so that the interaction stays familiar.
14. As a learner, I want due cards before new cards, so that existing commitments take priority.
15. As a learner, I want a consistent allowance of 10 new cards (default) per day across all study scopes, so that switching decks does not multiply the workload. A learner should be able to configure this to increase or decrease.
16. As a learner, I want to see the prompt before deliberately revealing the answer, so that I can attempt recall first.
17. As a learner, I want clear recall-rating buttons, so that I can record a review without understanding scheduler mathematics.
18. As a learner, I want interrupted or retried submissions to be safe, so that unreliable mobile connections do not lose or duplicate my reviews.
19. As a learner, I want optional ungraded practice of forgotten cards, so that I can revisit them without distorting my schedule or statistics.
20. As a learner, I want due-now counts, so that I know what needs attention.
21. As a learner, I want to see reviews completed today, so that I can see today's work.
22. As a learner, I want 14 days of daily review activity, so that I can build a picture of my study habits.
23. As a learner, I want the next seven days of currently scheduled workload, so that I can anticipate upcoming study.
24. As a learner, I want a Study Streak based on my local study dates, so that it reflects my actual routine.
25. As a learner, I want comfortable phone controls and keyboard-accessible desktop interactions, so that I can use the app on either device.
26. As a learner, I want understandable loading, empty, expired-session, and error states, so that I know what happened and what to do next.
27. As a learner, I want my library and progress isolated from other learners, so that invitations do not imply shared access.
28. As the operator, I want to author and review a small original starter set, so that the first experience is useful without building a whole course.
29. As the operator, I want beta and sandbox environments kept separate, so that sandbox resets cannot erase learners' work.
30. As the operator, I want failed email delivery or revoked sender credentials to be visible, so that recovery does not fail silently.
31. As the operator, I want encrypted, independently stored backups and a rehearsed restore, so that I can recover retained data after a serious failure.
32. As the operator, I want usage limits and billing controls checked, so that the beta cannot silently exceed its $0 budget.
33. As the maintainer, I want independent frontend/API releases with a tested browser-facing contract, so that deployment independence does not break login or studying.
34. As a learner, I want consistent, restrained visual hierarchy and obvious actions, so that the interface does not compete with learning.
35. As a learner, I want an explained way to request account/data removal and understand the beta's recovery limitations, so that I can make an informed decision about using it.

## Implementation Decisions

### 1. Domain and ownership

The shared domain glossary remains authoritative; do not create a competing frontend vocabulary.

| Concept | V1 meaning and behavior |
| --- | --- |
| Topic | Flat, user-owned grouping of decks. A deck has zero or one Topic. Topic membership grants no access. Removing a Topic ungroups its decks, including archived decks, without deleting content. |
| Deck | User-owned collection of cards; the existing unit of access control. Ungrouped is a library view, not a shared system-owned deck or Topic. |
| Starter Deck | Operator-reviewed original offered for copying. A copy has new identities, belongs to the learner, and includes content but no other learner's reviews/progress. Subsequent template changes do not update copies. |
| Basic Card | Nonempty text prompt and answer. The answer is deliberately revealed before rating. |
| Cloze Card | Text with one or more valid, nonempty selected blanks. All blanks hide/reveal together, with one card identity, rating, and schedule. No separately scheduled blanks. |
| New Card | A card without a saved Review by that learner. Its creation date is irrelevant. |
| Review | One persisted self-graded attempt. History and scheduling changes belong to the authenticated learner. |
| Practice Repeat | Optional ungraded replay after forgotten cards; not a Review and never a scheduling/statistics event. |
| Study Streak | Consecutive local calendar dates with at least one saved Review. It is not a consecutive-correct-answer count. |

Personal starter copies avoid introducing shared-library membership or collaborative editing into V1. Existing backend Deck Access protections remain in force; the frontend must not weaken them.

Archive/restore preserves history. An archived deck hides its cards from study without erasing each card's independent archive status. Restoring a deck must not accidentally restore a card previously archived individually. Archived material is excluded from current due counts and future workload; its actual past Reviews remain in historical activity.

Ordinary card edits retain scheduling/history. A substantially different concept should be a new card, with the old one archived. Card-type conversion and manual progress-reset controls are deferred. Do not imply that historical activity is a versioned record of old card wording.

### 2. Modules and their interfaces

| Technology | V1 responsibility |
| --- | --- |
| React | Renders screens and manages local interaction state such as unsaved edits, revealing an answer, choosing a rating, and Practice Repeats. Use React state or reducers. The reviewer keeps the current card visible until the API confirms the saved Review. |
| TanStack Query | Fetches and caches API data in the browser. Manages mutations and request status, and invalidates or refetches affected queries after the API confirms a write. |
| Express | Hosts the API and Better Auth integration. The backend validates requests, enforces sessions, Deck Access, and beta learner isolation, and persists learner data. Its services determine study eligibility and daily allowances, apply SM-2, handle duplicate or concurrent submissions, save Reviews and scheduling updates atomically, and calculate progress aggregates. |

Next.js owns routing and layouts and provides the narrow same-origin forwarding path for application and auth requests to Express. Reuse existing module boundaries.

| Module | Interface and responsibility |
| --- | --- |
| Account access | Sign-in/out, session restoration, invited onboarding, reset/change password, and saved timezone. Reuse existing Better Auth identity; no second auth provider or browser-stored bearer token scheme. |
| Library | Work with owned Topics, decks, cards, starter copies, and archive state. Authorization and transactional copy/removal rules stay in the backend. |
| Study | Obtain eligible work and save a graded Review safely. Backend owns eligibility, SM-2, daily allowance, idempotency, and concurrency. Frontend owns transient prompt/reveal/saving/completion/practice interaction. |
| Progress | Return authorized, bounded aggregates for counts, local-day activity/streaks, and scheduled workload. The frontend presents them rather than downloading all history and reinventing the calculations. |
| Browser delivery | Forward narrowly allowed application/auth requests to one configured backend. It is not another business backend, generic proxy, or alternate identity system. |
| Recovery operations | Produce encrypted consistent backups, report freshness/failure, and restore a matching application/database release under operator control. |

Use the existing OpenAPI contract to keep transport types synchronized; do not invent a parallel hand-maintained domain model. Pin a compatible contract/release and test backward compatibility rather than relying on independently deployed `latest` versions agreeing by chance.

### 3. Exact study behavior

- Start at whole-library, Topic, or Deck scope using the same reviewer.
- Return accessible, active due cards first, oldest due first with deterministic ties. Then offer New Cards in a stable order.
- A card due outside the selected scope does not prevent studying eligible cards inside it.
- New-card allowance is default 10 per learner per saved-timezone calendar day, shared across all scopes. It does not carry over.
- Consume one allowance only when that card's first graded Review is successfully saved, including a Forgot rating. Loading, revealing, abandonment, and retries consume none.
- Enforce allowance on the server atomically, including concurrent tabs. Do not rely on a client counter.
- Exclude inaccessible/archived content. Recheck access and eligibility when saving, not only when initially displaying a card.
- Rating is available only after reveal: Forgot = 0, Hard = 3, Good = 4, Easy = 5.
- Keep existing SM-2 calculations for V1. FSRS and graded short-interval learning remain deferred.
- A failed Review keeps the existing next-day scheduling behavior. Optional Practice Repeats at the end never submit another grade.
- History, scheduling, and first-review allowance consumption must commit together or not at all.
- A logical submission has a stable retry identity. A retry returns the original outcome without an additional Review, allowance consumption, or schedule advance.
- Protect against stale/concurrent attempts, not only duplicate HTTP requests. A conflict must be explicit and recoverable.
- Keep the current card/rating visible while saving. Do not advance or show success before confirmation.
- If acknowledgement is lost, reconcile/retry the same submission; do not blindly create another one.
- Saved Reviews survive navigation/reload. Transient reveal/practice state need not survive browser closure; reload rebuilds eligible work from the server.
- Learners may stop at any time. No hard cap on due Reviews.

The study implementation ticket must settle and test bounded queue pagination, submission conflict semantics, and pending-request recovery. Avoid loading an entire large library into the browser.

### 4. Progress definitions

Store a validated learner timezone, initially suggested by the browser and confirmed by the learner. Keep event timestamps as instants; group them into calendar dates using the saved timezone rather than the API server's local timezone.

| Display | Definition |
| --- | --- |
| Due now | Accessible, active cards with a scheduled review time at or before now. Does not include New Cards merely because they have no schedule. |
| Reviews today | Count of successfully saved graded Reviews on the learner's current local date, including Forgot ratings. |
| Recent activity | Daily Review counts for the last 14 local dates, including today; missing dates display zero. |
| Upcoming workload | Cards currently scheduled in the next seven local calendar dates, including today's not-yet-due work; show overdue work separately as due now. This is a current schedule snapshot, not a forecast of future rating outcomes. |
| Study Streak | Consecutive dates with a Review. A streak last continued yesterday remains current until today's opportunity to study has passed; no extra freeze/grace system. |

Provide whole-library and Topic/deck views where meaningful; the headline Study Streak is learner-wide. Historical activity includes archived content and excludes Practice Repeats. Topic-scoped history follows current deck membership in V1; there is no historical Topic-membership audit model.

Do not use the backend's SM-2 `reviewCount` as lifetime review totals, or `correctStreak` as consecutive study days. Aggregate persisted Review history. Do not invent a mastery percentage, examination-readiness score, or recall-accuracy claim from these counts.

New/unreviewed counts were not selected as a required dashboard metric. Study-queue explanations may still explain eligibility and remaining allowance.

### 5. Interface and visual direction

- Main destinations: Study, Library, Progress; account settings through a profile entry.
- Phone bottom navigation and equivalent desktop sidebar; a focused reviewer with an obvious exit.
- Topics, decks, starter selection, and archive live within Library.
- Full-page phone editing rather than nested, cramped dialogs.
- Light-first neutral/pastel surfaces, restrained editorial influence, readable typography, generous spacing, and clear action hierarchy.
- Pastel backgrounds must not produce low-contrast text or ambiguous state indicators. Do not communicate correctness or availability through color alone.
- Tailwind plus a small shared token system for color, spacing, type, borders, and focus.
- Adopt only needed shadcn/ui controls. Their source becomes maintained application code, not a CDN dependency or a reason to import every example block.
- Native controls where sufficient; custom interactions must preserve labels, focus, keyboard operation, and comfortable touch targets.
- Cloze authoring uses text selection and preview. Validate empty/overlapping/out-of-range selections and render text safely rather than trusting HTML. Canonical storage representation is a decision inside the cloze slice.
- Every journey includes loading, empty, validation, permission-denied, expired-session, saving, and retry/error states as applicable.
- Warn before discarding unsaved editor work. Do not introduce offline drafts, autosync, or optimistic "saved" messages as a shortcut.
- No mandatory dark mode, heavy animation, rich-text editor, paid fonts, or media pipeline in V1.

### 6. Hosting and authentication

Preferred topology: the browser talks to one Vercel-hosted Next.js origin. A narrow same-origin forwarding path reaches the independently deployed API. User-created detail URLs require a normal Next.js runtime/routing solution; do not bake private deck IDs into static build output.

Start with native hosting/proxy facilities where they satisfy the contract. Add custom forwarding code only for a demonstrated requirement. Preserve method, body, required cookie headers, status, and redirects; prohibit arbitrary upstream destinations, authentication-response caching, and accidental forwarding of unrelated secrets.

The API's current production mode is sandbox-restricted. Introduce an explicit, fail-closed distinction between disposable sandbox and retained-data beta without disabling production security controls or changing the sandbox's existing contract.

Before accepting the topology, verify:

- Canonical frontend/auth origin and reset/callback URLs are consistent.
- Secure, HttpOnly session cookies work through sign-in, reload, sign-out, password reset, and protected requests on target browsers.
- No reliance on third-party cookies, wildcard trusted origins, weakened CSRF checks, or disabled Fetch Metadata checks.
- Proxy identity and rate limiting are revalidated for the actual additional hop; existing single-hop assumptions are not reused blindly.
- Authenticated responses cannot leak through shared caches.
- Preview deployments never gain access to real beta data automatically.
- Cold-start timeouts and retry UX are tested against the real API host.
- Independent frontend/API releases and rollback combinations satisfy their pinned contract.

Real-user onboarding is operator-controlled and distinct from synthetic tester tooling. Public registration stays unavailable. Use supported account/invitation/password-establishment mechanisms; never send a shared password or silently remove synthetic-only safeguards.

Self-service reset must use expiring, single-use links and non-enumerating responses. Delivery errors require safe logging/operator visibility. Account settings include password change, timezone, sign-out, and an operator contact for data removal; no billing or social login.

#### Password-reset email candidate

Use a dedicated free Gmail mailbox through the Gmail HTTPS API. This is only a mail-sending integration: learners still sign in with the app's email/password system.

The operator authorizes the sender with the minimum required scope. Before launch, prove eligibility for the documented OAuth arrangement, refresh-token operation beyond seven days, delivery to actual target inbox providers, revocation/error reporting, and the continued $0 billing configuration. Publishing an OAuth consent configuration is not equivalent to launching the beta publicly.

Render Free blocks standard outbound SMTP ports, so a Gmail SMTP password alone is not a solution from that host. Do not replace working authentication merely to obtain an email feature.

### 7. Backups and Google Drive, in plain English

**Google Drive would hold emergency spare copies. It would not become the app's database.**

During normal use, a learner's decks, cards, account, and progress stay in the beta database. The app reads and writes that database. It does not load a learner's progress from Google Drive, and learners do not need Google accounts.

Saving data in a database protects it from ordinary app restarts. It does not, by itself, undo a bad migration, an accidental deletion, a damaged database, or loss of access to the database provider. A backup is a separate copy that can be used to rebuild the app's data after one of those failures.

**Why a separate location?** If the only spare copy lives alongside the thing that failed, the same mistake or outage might affect both. A private Drive folder is a candidate independent destination we can investigate without buying another service. It is not the only possible backup destination, and it is not yet provisioned or proven.

**Why encryption as well as a private folder?** A full recovery copy can contain email addresses, password hashes, study material, and progress. Password hashes are not plaintext passwords, but they are still sensitive. "Private folder" controls who can download a file; encryption also prevents someone who obtains the file from reading its contents without the recovery key.

Think of the process as:

1. Make a consistent recovery copy of the database.
2. Put that copy in a locked digital box before uploading it.
3. Upload only the locked box to a private folder.
4. Keep the unlocking key separately under the operator's control.
5. Practise unlocking a copy and restoring it into an isolated test database.

The automated backup job should have the public encryption key, which can lock a box, but not the private recovery key, which unlocks it. Google Drive receives encrypted files, not readable tables. Plaintext dumps must not appear in Git, GitHub artifacts, workflow logs, or email.

**What do the recovery targets mean?** If a failure happens today, we aim to have a usable backup less than 24 hours old and to restore service within 48 hours. Some work since the last usable backup could still be lost. This is not the same as "no data can ever be lost."

**Why not just run a backup inside the app?** The free app service may sleep. A scheduled job outside that service is a better candidate, but free scheduling can also be delayed or skipped. The operator must check backup freshness and failures rather than assume a timer always ran.

#### Candidate mechanism and proof

- An independently scheduled GitHub Actions job makes an encrypted logical database export and uploads it to a private Google Drive folder.
- Authenticate to personal Drive as the authorized operator using narrowly scoped OAuth access to app-created files; do not assume a service account owns free personal storage.
- Keep Drive-upload and email-sending privileges distinct. Neither credential belongs in the frontend, and neither requires access to the entire personal Drive.
- Proposed operational defaults for the proof: attempt a backup every six hours, alert/check when no successful backup is younger than 12 hours, retain a rolling seven days subject to measured capacity, and never delete the last known-good copy because a newer upload failed.
- The operator must hold a recoverable private key, own failure response, and rehearse restoration. Losing that key can make otherwise healthy encrypted backups unusable.
- Verify actual Google storage capacity, shared usage, API pricing, GitHub compute quotas, and hard billing controls. Do not assume a new account has any particular free allowance.
- GitHub cron is best-effort: frequent attempts and monitoring reduce risk but do not create a guaranteed 24-hour recovery point.
- Test a restore into an isolated database, with matching schema/application version, before admitting real learners. Invalidate restored sessions and outstanding recovery/invitation tokens so restoration cannot resurrect revoked access.
- Document how account removal interacts with backup expiry and later restoration.

If this cannot meet the recovery targets and $0 policy in practice, the backup decision remains blocked. Do not quietly call the disposable sandbox "persistent," upload unencrypted data, or promise guaranteed retention on the strength of a successful upload.

### 8. Current backend gaps

The inspected source baseline has:

- Authenticated deck/card CRUD, existing Deck Access checks, cookie authentication, and self-rated SM-2 Reviews.
- A due-card endpoint that excludes New Cards and lacks the complete scoped/bounded V1 queue.
- No Topic model or grouping contract; free-text topic metadata is not a Topic.
- Cloze accepted as a type, without a working cloze parser/editor contract.
- Per-card progress and stored Review history, but no dashboard/history-aggregate interface.
- Separate history/progress writes and no logical submission idempotency; these must be fixed for safe mobile reviewing.
- Deletion paths that cannot be assumed to handle studied/populated content safely; archive/restore is new work.
- Sandbox-only production account restrictions and synthetic-only provisioning.
- No deployed frontend bundle/proxy or proven beta backup mechanism.

Treat these as implementation prerequisites. Glossary terms and advertised auth capabilities do not establish working product behavior.

### 9. Options considered

| Choice | Outcome |
| --- | --- |
| Sibling frontend repository vs monorepo migration | Separate sibling repository chosen; avoid restructuring the backend during sandbox testing. Coordinate API compatibility explicitly. |
| React/Vite SPA vs Next.js | User chose Next.js with TypeScript and TanStack Query. Do not add a duplicate routing library or move existing business logic into it. |
| Combined app/API deployment vs independent releases | Independent releases required; retain one browser-facing origin through narrow forwarding. |
| Cloudflare static frontend/proxy vs Vercel for Next.js | Cloudflare was investigated before the Next.js choice. Vercel Hobby is preferred for eligible personal V1; Cloudflare would require separate runtime/compatibility evaluation, not an assumed drop-in. |
| Entire UI kit vs selective shadcn/ui | Selective maintained source components and a small Tailwind theme chosen. |
| Shared starter originals vs personal copies | Independent editable copies chosen; no content synchronization in V1. |
| Exact Anki behavior vs familiar interaction | Keep SM-2 and explicit V1 rules; defer imports, FSRS, and graded short-interval relearning. |
| Manual recovery vs self-service email | Self-service email required; Gmail HTTPS sender remains subject to live proof. |
| Permanent library deletion vs archive | Archive/restore chosen; explicit account/data-removal process remains separate. |

## Testing Decisions

Test behavior through module interfaces and real integration seams, not internal hook calls or mocked implementation structure.

- Preserve the backend's existing authentication, Deck Access, sandbox-boundary, logging, deployment, and schema-validation tests.
- Add focused database-backed tests for Topics, starter copying, archive filtering, cloze validation, new-card allowance, SM-2 persistence, idempotency, concurrency, and aggregation.
- Reuse existing backend test factories and isolated database practices. Never test against retained learner data.
- Give the reviewer deterministic tests covering prompt/reveal/save/retry/complete/practice transitions and double-click/network-acknowledgement failures.
- Browser tests cover invite/login/reset, library editing, study, archive/restore, progress, deep-link reload, and expired sessions.
- Exercise narrow-phone layouts, keyboard-only use, focus handling, screen-reader names, contrast, and touch interaction. Automated checks do not replace a real-phone review.
- Include local-date boundaries, daylight-saving transitions, empty libraries, zero due cards, exhausted new allowance, archived parents/children, and stale concurrent work.
- Backend acceptance must test the actual threshold: first 10 New Cards accepted, an 11th rejected safely, the same submission counted once, and allowance shared across Deck/Topic/library scopes.
- Verify chart output using persisted history, not the scheduler's success counters. Include days without activity and archive/restore effects.
- Live provider checks are separate HITL gates: cookie/proxy behavior, email after more than seven days, encrypted restore, observed resource use, and billing controls.
- Pin compatible tooling versions when implementing. Use the existing backend runner; select a supported, minimal Next.js browser/component test setup rather than introducing overlapping test stacks.

## Out of Scope for V1

- Checklist-based written practice, scheduled for the next milestone.
- AI creation, AI feedback, and paid access, each listed under [Deferred decisions](#deferred-decisions).
- Automatic examination scoring and exam-readiness scores, which are also outside the full MVP.
- Anki import/export compatibility, scheduler replacement, FSRS, graded same-day learning steps, and arbitrary schedule resets.
- Multiple-choice or separately typed open-ended review flows.
- Images/maps, audio, uploads, rich-text authoring, and separately scheduled cloze blanks.
- Offline mode, native apps, background synchronization, or cross-device transient-session continuation.
- Nested/multi-membership Topics, a fixed curriculum hierarchy, broad subject-platform features.
- Collaborative editing, public discovery, automatic updates to starter copies, public registration, social login.
- Billing, subscriptions, advertisements, marketing/SEO pages, and compulsory dark mode.
- A complete WSET course library, official affiliation, or promised examination outcomes.

## Further Notes

### Delivery and release gates

The [ticket drafts](./frontend-v1-tickets.md) sequence narrow, demoable end-to-end slices. Feasibility probes use isolated synthetic data; normal development can continue on independent slices, but no real-user launch precedes successful hosting/auth/email/backup proofs.

The owner supplies and reviews a small original starter set. The beta includes a plain-language notice about personal data, availability, recovery limitations, and account-removal contact. Reassess provider eligibility before commercial use.

Native issue numbers and cross-repository dependencies are a later publication step. This local draft creates none.

### Evidence and sources

Backend inspection baseline: `474b8956cdccbbde1bc58b1bd215a3a658fa0d9d`; observed source capability is not proof of live deployment.

- [Shared domain glossary](../../server/docs/GLOSSARY.md)
- [Browser-session contract](../../server/docs/browser-boundary.md)
- [Sandbox deployment runbook](../../server/docs/deployment-runbook.md)
- [Existing API documentation](../../server/docs/api-reference.md)
- [Review interface](../../server/src/routes/review/review.router.ts), [scheduling behavior](../../server/src/services/review.service.ts), and [persistence](../../server/src/model/reviewModel.ts)
- [Card validation](../../server/src/utils/cardSchema.ts) and [database schema](../../server/prisma/schema.prisma)
- [Next.js deployment guidance](https://nextjs.org/docs/app/getting-started/deploying) and [static export limitations](https://nextjs.org/docs/app/guides/static-exports)
- [shadcn/ui ownership model](https://ui.shadcn.com/docs)
- [Vercel Hobby limits](https://vercel.com/docs/plans/hobby) and [commercial-use policy](https://vercel.com/docs/limits/fair-use-guidelines#commercial-usage)
- [Render Free limitations, including SMTP and sleeping services](https://render.com/docs/free)
- [Gmail sending](https://developers.google.com/workspace/gmail/api/guides/sending), [OAuth scopes](https://developers.google.com/workspace/gmail/api/auth/scopes), and [OAuth token lifecycle](https://developers.google.com/identity/protocols/oauth2)
- [Gmail API quota/pricing policy](https://developers.google.com/workspace/gmail/api/reference/quota)
- [Drive API authorization](https://developers.google.com/workspace/drive/api/guides/api-specific-auth), [limits/pricing policy](https://developers.google.com/workspace/drive/api/guides/limits), and [account storage](https://support.google.com/drive/answer/9312312)
- [GitHub scheduled-workflow limitations](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule) and [Actions billing](https://docs.github.com/en/billing/concepts/product-billing/github-actions)

Provider documentation was researched during planning. Actual account eligibility, quotas, current pricing, deployed behavior, and restore evidence must be rechecked before launch.
