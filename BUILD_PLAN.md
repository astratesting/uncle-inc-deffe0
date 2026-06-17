# Uncle Inc. — Landing Page Build Plan

## 1. PRODUCT

Uncle Inc. is a pre-build validation workspace for solo founders, indie hackers, and product teams. It replaces the "build it and hope" loop with a guided path from raw idea → AI-generated prototype → live user test session → quantitative signal. The landing page's job is to convert qualified visitors into a waitlist by demonstrating that the product kills the #1 founder pain: **shipping something nobody wants**. The headline **"Validate Before You Build"** and the six feature cards must each prove a concrete step of that loop, and every waitlist field must feel like the first real validation step (email only, but framed as "join builders validating first").

## 2. WHO IT'S FOR

**Primary ICP:** technical and semi-technical solo founders / indie hackers / PMs aged 25–45 building their first or next product. They have shipped or attempted to ship side projects, are time-poor, allergic to fluff, and evaluate tools in under 90 seconds. They skim, not read.

**How that shapes the build:**
- Above-the-fold = headline + 1-line subhead + email field + single CTA. No nav links competing for attention.
- Six feature cards = dense, scannable, each card = icon + 3–6 word title + 1 sentence. No long paragraphs.
- Pricing = three tiers side-by-side, prices large, scope of each tier in 4–6 bullets max.
- FAQ = only the six questions a skeptical founder would actually ask (no generic "What is Uncle?" filler).
- Copy tone: direct, builder-to-builder, mild conviction. Zero hype words ("revolutionize", "supercharge", "10x"). Zero fake stats.

## 3. LOOK & FEEL

### Visual system
- **Vibe:** warm SaaS. Not corporate, not playful — a quiet, confident tool for serious builders. Think Linear meets a coffee shop.
- **Palette (CSS variables in `globals.css`):**
  - `--brand-primary: #7c3aed` (violet-600) — primary actions, links, focus rings
  - `--brand-accent: #f97316` (orange-500) — secondary CTA, highlight badges
  - `--brand-honey: #f59e0b` (amber-400) — gradient stops, decorative dots
  - `--brand-bg: #faf5ff` (off-white, violet-tinted) — page background
  - `--brand-surface: #ffffff` — card backgrounds
  - `--brand-text: #0f172a` (slate-900) — body and headings
  - `--brand-muted: #64748b` (slate-500) — secondary text
  - `--brand-border: #e9e4f5` — subtle dividers
- **Typography:**
  - Headings: `font-family: 'Manrope', sans-serif`, `font-weight: 700–800`, `letter-spacing: -0.02em`, `line-height: 1.15`
  - Body: `font-family: 'Source Sans 3', sans-serif`, `font-weight: 400–500`, `line-height: 1.6`
  - H1: clamp(2.5rem, 5vw, 4.5rem); H2: clamp(1.875rem, 3.5vw, 2.75rem); H3: 1.25rem
- **Spacing & layout:** section vertical padding `py-20 md:py-28`, container `max-w-7xl mx-auto px-6`. Generous whitespace.
- **Surfaces:** `rounded-2xl` for cards, `rounded-xl` for inputs/buttons. `shadow-sm` default, `shadow-lg shadow-violet-600/10` for primary CTA hover.
- **Iconography:** Lucide React icons (already in the design system). 1.5px stroke, `w-6 h-6` in cards, `w-5 h-5` in buttons.
- **Imagery:** No stock photos. Use abstract decorative SVG blobs (violet→amber gradient) behind the hero, and a CSS-drawn dotted grid behind the features section.
- **Motion:** `transition-all duration-200 ease-out` on interactive elements. Cards lift `-translate-y-0.5` on hover. Accordion height animates via `max-height` transition. No scroll-jank libraries.

### Section-by-section layout (top to bottom on `/`)

**`app/layout.tsx` (shell):**
- HTML lang="en", `<body className="bg-[--brand-bg] text-[--brand-text] font-sans antialiased">`
- Loads Manrope (200–800) + Source Sans 3 (300–700) from Google Fonts via `next/font/google`, exposed as CSS variables `--font-manrope` and `--font-sans`.
- Metadata: title "Uncle — Validate Before You Build", description, OG tags, theme-color `#7c3aed`, favicon `/favicon.ico` (placeholder).

**`components/Nav.tsx` (sticky top, `h-16`, `backdrop-blur-md bg-[--brand-bg]/80 border-b border-[--brand-border]`):**
- Left: Uncle wordmark (small violet hexagon SVG + "Uncle" wordmark, links to `#top`).
- Center (desktop only): anchor links "Features", "Pricing", "FAQ" — `text-sm font-medium text-slate-600 hover:text-violet-600`.
- Right: text link "Sign in" (grey, non-functional for now — honest placeholder, no fake button) + primary pill "Join waitlist" that smooth-scrolls to `#waitlist`.
- Mobile (below `md`): hamburger toggles a slide-down panel (`useState` for `isOpen`) with the same three links + CTA.

**`components/Hero.tsx` (`id="top"`, `pt-32 pb-24 md:pt-40 md:pb-32`):**
- Background: two absolutely-positioned decorative blobs — one top-right (`w-[500px] h-[500px] bg-violet-600/10 blur-3xl rounded-full`), one bottom-left (`w-[400px] h-[400px] bg-amber-400/15 blur-3xl rounded-full`).
- Center column, `max-w-3xl`, `text-center`:
  1. Pill badge: `inline-flex items-center gap-2 rounded-full border border-violet-600/20 bg-violet-600/5 px-3 py-1 text-xs font-semibold text-violet-600` — text "Now in private beta · Limited spots".
  2. H1: "Validate Before You Build" (Manrope 800, tracking-tight).
  3. Subhead (Source Sans 3, 1.125rem, slate-600, `max-w-2xl mx-auto`): one sentence stating the value.
  4. Waitlist form (the first validation interaction): `<form>` with email input + submit button side by side on desktop, stacked on mobile. Input: `h-12 rounded-xl border border-[--brand-border] bg-white px-4 text-base placeholder:text-slate-400 focus:border-violet-600 focus:ring-2 focus:ring-violet-600/20`. Button: `h-12 px-6 rounded-xl bg-violet-600 text-white font-semibold hover:bg-violet-700 shadow-sm`.
  5. Helper text under form: "Free during beta. No credit card. Unsubscribe anytime." in `text-xs text-slate-500`.
- Below the form: three small "what you get" chips in a row (no fake numbers, just feature labels): "AI prototype in minutes", "Real user sessions", "Clear go/no-go signal".

**`components/Features.tsx` (`id="features"`, `py-24`, light dotted-grid background):**
- Section header: eyebrow "HOW IT WORKS" (uppercase, `text-xs font-bold tracking-widest text-violet-600`), H2 "From idea to signal in days, not months", one-line sub.
- Grid: `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6`.
- Six cards, each:
  - `rounded-2xl border border-[--brand-border] bg-white p-7 hover:shadow-lg hover:-translate-y-0.5 transition-all duration-200`
  - Icon in a `w-11 h-11 rounded-xl bg-violet-600/10 text-violet-600` flex container (Lucide icon)
  - H3 title (Manrope 700, `text-lg`)
  - One-sentence description (Source Sans 3, slate-600)
  - Card order and exact copy:
    1. `Sparkles` — "AI Rapid Prototyping" — "Describe your idea in a sentence; get a clickable prototype in under ten minutes."
    2. `Users` — "Built-in User Testing" — "Recruit from our network of vetted testers or invite your own; watch real people try it."
    3. `BarChart3` — "Launch Analytics" — "See where testers hesitate, drop off, and come back — with session replays built in."
    4. `Compass` — "Guided Validation" — "Step-by-step prompts run you from hypothesis to a clear go/no-go decision."
    5. `Code2` — "No Code Required" — "If you can write a tweet, you can run a validation. Engineering stays out of the loop until it counts."
    6. `RefreshCw` — "Iterate with Data" — "Tweak the copy, the flow, or the price; ship a new variant in minutes, not sprints."

**`components/Pricing.tsx` (`id="pricing"`, `py-24`):**
- Section header: eyebrow "PRICING", H2 "Simple pricing. Pay only when you launch.", one-line sub.
- **Monthly/Yearly toggle** (client state): pill switch; default "Monthly". Yearly shows 20% off.
- Grid: `grid grid-cols-1 md:grid-cols-3 gap-6`.
- Three cards, the middle one elevated with `border-violet-600 ring-2 ring-violet-600/20` and a "Most popular" badge:
  - **Free — $0/mo** (or $0/yr)
    - For solo founders validating their first idea.
    - Bullets: 1 active validation, Up to 5 tester sessions/mo, Basic analytics (7-day retention), Community support
    - CTA: "Start free" → secondary button `bg-white border border-[--brand-border] text-slate-900 hover:bg-slate-50`
  - **Pro — $29/mo** (or $279/yr) — RECOMMENDED
    - For founders ready to iterate fast.
    - Bullets: Unlimited validations, Unlimited tester sessions, Full session replays + heatmaps, Guided validation playbooks, Email support
    - CTA: "Join Pro waitlist" → primary violet button
  - **Team — $79/mo** (or $759/yr)
    - For small product teams validating together.
    - Bullets: Everything in Pro, Up to 5 seats, Shared validation library, Custom tester recruitment, Priority support
    - CTA: "Join Team waitlist" → secondary button

**`components/FAQ.tsx` (`id="faq"`, `py-24`):**
- Section header: eyebrow "FAQ", H2 "Questions builders actually ask", one-line sub.
- Two-column on `lg` (3 left, 3 right), single column below.
- Six `<details>`-style accordion items built manually with `useState<number | null>` (controlled — not native `<details>`, because the design must match exactly):
  - `border-b border-[--brand-border]` rows, click to expand.
  - Question: `text-base font-semibold text-slate-900` with a chevron icon that rotates 180° when open (`transition-transform`).
  - Answer: collapses with `max-h-0 → max-h-96` transition, `text-slate-600 text-sm`.
  - Exact Q&A:
    1. **"Is this another no-code tool?"** — "No. Uncle is for *before* you build. It helps you decide what to build — and what to kill — using real user behavior, not gut feel."
    2. **"Do I need to code to use Uncle?"** — "Not at all. If you can describe your idea in plain English and click a button, you can run a full validation."
    3. **"How do you get testers?"** — "You can invite your own audience with a share link, or tap into our network of vetted testers who opt in to try new products."
    4. **"What does 'validation' actually mean here?"** — "It means a structured loop: hypothesis, prototype, real-user session, decision. We give you the template and the data; you make the call."
    5. **"Can I export my prototype?"** — "Yes. Every prototype is exportable as clean HTML/CSS, a Figma file, or a Next.js starter repo — whichever your engineer prefers."
    6. **"When does the public beta open?"** — "We're onboarding from the waitlist in small cohorts. Joining the waitlist gets you an invite within 4–6 weeks."

**`components/CTA.tsx` (`id="waitlist"`, `py-24`):**
- Full-width card with `rounded-3xl bg-gradient-to-br from-violet-600 to-amber-400 p-12 md:p-16 text-center text-white shadow-2xl shadow-violet-600/20`.
- H2 (white, Manrope 800): "Stop building in the dark."
- Subhead (white/90, Source Sans 3): "Join builders who test their ideas with real users before writing a line of code."
- Same waitlist form as Hero, but with `bg-white/10 border-white/20` input and white solid submit button.
- After submit: form is replaced by an inline success card ("You're on the list. Check your inbox for a confirmation.") with a small `CheckCircle2` icon — no email is actually sent (honest copy that says "we'll be in touch").

**`components/Footer.tsx` (`border-t border-[--brand-border] py-12`):**
- Three columns on `md+`:
  - Left: Uncle wordmark + one-line description + © 2026 Uncle Inc.
  - Middle: "Product" links (Features, Pricing, FAQ) — anchor links.
  - Right: "Company" links (Privacy, Terms, Contact) — `#` placeholders.
- Mobile: stacked, centered.
- All copy factual, no fake addresses or "trusted by 10,000 teams" badges.

## 4. USER FLOWS

**Flow A — Primary: Visitor joins waitlist**
1. Land on `/` → see Hero with headline, sub, email field.
2. Type email into the input.
3. Click "Join waitlist" → client validation (email regex).
4. Server Action `joinWaitlist(email, source)` runs (no real Supabase required — see Auth).
5. On success: form swaps to a success state ("You're on the list. We'll be in touch."). Button shows a check icon.
6. On error: red helper text under input ("Something went wrong. Try again."). Form remains editable.
7. User can scroll down to Features / Pricing / FAQ / second CTA form (same handler).

**Flow B — Navigation**
1. Click "Pricing" in nav → smooth scroll to `#pricing` section.
2. Click "Join waitlist" in nav → smooth scroll to bottom CTA `#waitlist`.
3. On mobile: tap hamburger → panel slides open, tap a link → panel closes, page scrolls.

**Flow C — Pricing toggle**
1. User on `#pricing` taps the Yearly switch.
2. Prices animate (`transition-all`) from `$29/mo` → `$279/yr` with helper text "Save 20%".
3. Card content stays the same; only the price labels and helper text change.

**Flow D — FAQ**
1. User clicks a question row.
2. Row's answer expands (`max-h` transition 200ms), chevron rotates.
3. Clicking another row closes the previously open one (single-open accordion).

## 5. PAGES / ROUTES

| Route | Purpose | Layout summary |
|---|---|---|
| `/` | Single-page landing | Nav → Hero (`#top`) → Features (`#features`) → Pricing (`#pricing`) → FAQ (`#faq`) → CTA (`#waitlist`) → Footer |
| `/api/waitlist` (POST) | Server route handler | Accepts `{ email, source }`, validates, inserts into Supabase `waitlist` table (if `SUPABASE_URL` + `SUPABASE_SERVICE_ROLE_KEY` are set), otherwise returns success (logged). Always returns `{ ok: true }` to the client. |
| `/privacy` (optional stub) | Plain text page | One-column, `max-w-2xl`, minimal copy: "We collect email and basic analytics. Nothing more." Honest placeholder body. Linked from footer. |
| `/terms` (optional stub) | Plain text page | "Use the service responsibly. Don't be a jerk." Honest placeholder body. Linked from footer. |

Privacy and Terms are simple static routes so the footer links aren't dead, with explicitly honest placeholder copy.

## 6. CORE FEATURES

| # | Feature | What it does | How it works (concretely) |
|---|---|---|---|
| 1 | **Hero waitlist form** | First conversion point | Controlled `<input type="email" required>` + submit button. On submit, calls Server Action `joinWaitlist` with `source: 'hero'`. Shows loading state (button text → "Joining…" + disabled), then swaps to success message on `ok`, error message otherwise. |
| 2 | **Feature grid** | Communicates the 6-step validation loop | Server component, static array of 6 feature objects (`{ icon, title, description }`) rendered into 6 cards. Lucide icons imported per-card. |
| 3 | **Pricing tiers + toggle** | Sets price expectations | Client component. `useState<'monthly' | 'yearly'>('monthly')`. Prices stored as `{ monthly, yearly }` per tier. Toggle pill animates a sliding background. CTA buttons all link to `#waitlist` (since we are pre-launch). |
| 4 | **FAQ accordion** | Removes purchase friction | Client component. `useState<number | null>(null)` tracks open index. Only one row open at a time. `max-h-0` → `max-h-96` via Tailwind transition utilities. Chevron rotates 180° via `transform rotate-180`. |
| 5 | **Bottom CTA + waitlist** | Second conversion point | Same form logic as Hero, different visual treatment (gradient background, white text). Same Server Action with `source: 'footer_cta'`. |
| 6 | **Mobile nav menu** | Mobile usability | Client component. `useState<boolean>(false)`. Hamburger icon toggles a `md:hidden` panel. Clicking a link or the CTA closes the panel (`setIsOpen(false)`). Body scroll lock not needed since panel is short. |
| 7 | **Smooth scroll anchors** | In-page nav | All anchor links use native `scroll-behavior: smooth` set in `globals.css` on `html`. No JS scroll library. |
| 8 | **Server Action: `joinWaitlist`** | Email capture backend | `app/actions/waitlist.ts` exports `async function joinWaitlist(formData: FormData)`. Validates email, checks rate limit by IP (simple in-memory LRU, 5/min), inserts into Supabase if env configured, else logs to console and returns `{ ok: true }`. Always returns success unless malformed. Re-validates email server-side. |
| 9 | **OG / metadata** | Link previews | `layout.tsx` sets `metadata` + `openGraph` + `twitter` cards with title, description, and a generated SVG OG image (`app/opengraph-image.tsx`, optional, default to gradient). |

## 7. DATA MODEL

Only one persistent entity exists at this stage (the rest of the product is post-launch):

**`waitlist` (Supabase table, optional — app works without it)**
- `id` — `uuid`, PK, default `gen_random_uuid()`
- `email` — `text`, NOT NULL, UNIQUE, lowercased, regex-validated
- `source` — `text`, NOT NULL, one of `'hero' | 'footer_cta' | 'pricing_pro' | 'pricing_team'`
- `created_at` — `timestamptz`, default `now()`
- `user_agent` — `text`, nullable
- Index: unique on `email`.

**`PricingTier` (TypeScript const, not persisted)**
- `id: 'free' | 'pro' | 'team'`
- `name: string`
- `price: { monthly: number; yearly: number }` (yearly = full-year price, not per-month)
- `description: string`
- `features: string[]`
- `ctaLabel: string`
- `highlighted: boolean`

**`Feature` (TypeScript const)**
- `icon: keyof typeof LucideIcons`
- `title: string`
- `description: string`

**`FAQItem` (TypeScript const)**
- `question: string`
- `answer: string`

## 8. AUTH

There is **no user auth** on this landing page — only an email waitlist capture. The waitlist is the marketing conversion, not sign-in.

**`joinWaitlist` Server Action:**
- Accepts `FormData` containing `email` and `source`.
- Server-side email validation: `^[^\s@]+@[^\s@]+\.[^\s@]+$`.
- If `process.env.SUPABASE_URL` and `process.env.SUPABASE_SERVICE_ROLE_KEY` are present: insert into `waitlist` table via `@supabase/supabase-js` server client.
- If env is missing: log `[waitlist] { email, source, ts }` to server console and return `{ ok: true }`. **The form must work out of the box without any external config.**
- Rate limiting: simple in-memory `Map<ip, number[]>` — reject if >5 requests in 60s, return `{ ok: false, error: 'Too many requests. Try again in a minute.' }`.
- Always returns JSON `{ ok: boolean; error?: string }` to the client.

**No OAuth, no Clerk, no NextAuth.** A dead social button is explicitly avoided.

**Environment variables (optional, all have fallbacks):**
- `SUPABASE_URL` — if absent, waitlist is logged only.
- `SUPABASE_SERVICE_ROLE_KEY` — if absent, waitlist is logged only.
- `NEXT_PUBLIC_SITE_URL` — used in metadata OG URL. Defaults to `http://localhost:3000`.

## 9. FILES (preview)

The full file tree is enumerated in the JSON at the end of this document. Each file is a concrete deliverable.

## 10. ACCEPTANCE

A working build must pass all of the following:

- [ ] `npm install && npm run dev` starts the app on `http://localhost:3000` with zero runtime errors.
- [ ] `npm run build` completes successfully with no TypeScript errors.
- [ ] Google Fonts (Manrope, Source Sans 3) load without network errors and are visibly applied to headings vs body.
- [ ] `/` renders Nav → Hero → Features → Pricing → FAQ → CTA → Footer, in that order, with anchor IDs `#top`, `#features`, `#pricing`, `#faq`, `#waitlist`.
- [ ] Brand colors match the spec: violet-600 primary button, amber-400 gradient on bottom CTA, `#faf5ff` page background, slate-900 text.
- [ ] Hero and bottom CTA waitlist forms both submit successfully, show a loading state, then a success message. Refreshing the page does not resubmit.
- [ ] Submitting an invalid email shows an inline error and does not call the server.
- [ ] FAQ accordion opens one row at a time, with smooth height and chevron rotation animations.
- [ ] Pricing toggle switches prices between monthly and yearly and animates the sliding pill.
- [ ] Mobile (<768px): nav collapses to hamburger, panels open/close, all sections stack vertically with no horizontal scroll.
- [ ] No fabricated testimonials, no fake metric counters, no invented customer logos, no press quotes.
- [ ] No dead social-sign-in buttons. Only the email waitlist, the in-page anchors, and the honest `Sign in` text link.
- [ ] `app/page.tsx` imports and renders exactly: `Hero`, `Features`, `Pricing`, `FAQ`, `CTA` (plus `Nav` and `Footer` from layout).
- [ ] `tsconfig.json` has `strict: true` and the project type-checks clean.
- [ ] Tailwind's content paths include `app/**` and `components/**`; arbitrary CSS variables work via `theme.extend.colors`.
- [ ] No client component is marked `'use client'` unless it actually uses state/effects (Nav, Hero, Pricing, FAQ, CTA). Features and Footer remain server components.

---

**FILES:** ["package.json", "next.config.js", "tsconfig.json", "postcss.config.js", "tailwind.config.ts", "next-env.d.ts", "app/globals.css", "app/layout.tsx", "app/page.tsx", "app/actions/waitlist.ts", "app/api/waitlist/route.ts", "app/privacy/page.tsx", "app/terms/page.tsx", "app/opengraph-image.tsx", "components/Nav.tsx", "components/Hero.tsx", "components/Features.tsx", "components/Pricing.tsx", "components/FAQ.tsx", "components/CTA.tsx", "components/Footer.tsx", "components/Logo.tsx", "lib/waitlist.ts", "lib/rateLimit.ts", ".env.example", ".gitignore", "README.md"]