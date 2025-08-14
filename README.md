🌍 AccessBnB
📌 Executive Summary

AccessBnB is an accessibility-first, community driven web application that helps people with disabilities, older adults, and anyone who benefits from accessible design to discover, review, and contribute detailed accessibility information about venues worldwide (hotels, restaurants, museums, parks, hospitals, transport hubs, shopping, entertainment, etc.).

Built with engineering rigor (IIT-style systems thinking) and accelerated using Base44 AI tools, AccessBnB is designed to be production-ready, modular, and highly extensible while keeping accessibility (WCAG 2.1 AA) and privacy at the core.

🎯 Vision & Goals

Make real accessibility data discoverable and reliable.

Enable community contributions and verifiable moderation.

Ship a performant, mobile-first experience with semantic, accessible UI.

Provide a platform architecture that scales and is easy for contributors to extend.

✨ Highlights & What’s New in This README

Full, professional README written from an IIT engineering perspective.

Complete entity JSON schemas and examples for Base44 SDK integration.

Clear developer workflow, CI/CD sample, and deployment checklist.

Accessibility, security, and testing checklists.

Sample code snippets and UX/component guidelines.

Contribution templates and roadmap prioritized for production adoption.

🚀 Live Demo

🔗 https://app--access-bn-b-c89a66e4.base44.app/

🧩 Project Architecture (High Level)

Frontend (React + Tailwind + shadcn/ui) — componentized UI, pages, routing via Base44 createPageUrl() where possible.

Platform Entities (Base44) — User, Country, City, Venue, Review, Favorite managed by Base44 SDK.

Asset Storage (Supabase) — all category images and user-uploaded images stored in controlled Supabase buckets.

Optional API Layer (Node/Express) — background jobs, heavy processing, webhooks, or third-party integrations.

CI/CD & Hosting — Base44 hosting (primary), optionally Vercel/Netlify for a decoupled frontend.

🗂 Core Entity Schemas (copy-paste ready JSON)
User (extension)
{
  "name":"User",
  "type":"object",
  "properties":{
    "display_name":{"type":"string","description":"User's preferred display name"},
    "bio":{"type":"string","description":"Short bio or about me"},
    "phone":{"type":"string","description":"Contact phone number"},
    "avatar_url":{"type":"string","description":"URL to user's avatar image"},
    "location":{"type":"string","description":"User's city or location"}
  }
}

Country
{
  "name":"Country",
  "type":"object",
  "properties":{
    "name":{"type":"string"},
    "iso_code":{"type":"string"},
    "flag_emoji":{"type":"string"},
    "total_cities":{"type":"integer","default":5},
    "total_venues":{"type":"integer","default":0}
  },
  "required":["name","iso_code"]
}

City
{
  "name":"City",
  "type":"object",
  "properties":{
    "name":{"type":"string"},
    "country_id":{"type":"string"},
    "country_name":{"type":"string"},
    "population_estimate":{"type":"integer"},
    "latitude":{"type":"number"},
    "longitude":{"type":"number"},
    "description":{"type":"string"},
    "accessibility_score":{"type":"number","minimum":0,"maximum":5},
    "venue_count":{"type":"integer","default":0}
  },
  "required":["name","country_id","country_name"]
}

Venue
{
  "name":"Venue",
  "type":"object",
  "properties":{
    "name":{"type":"string"},
    "address":{"type":"string"},
    "city":{"type":"string"},
    "city_id":{"type":"string"},
    "country":{"type":"string"},
    "country_id":{"type":"string"},
    "category":{"type":"string","enum":["hotel","restaurant","museum","park","hospital","shopping","entertainment","transport","other"]},
    "description":{"type":"string"},
    "image_urls":{"type":"array","items":{"type":"string"}},
    "accessibility_features":{
      "type":"array",
      "items":{"type":"string","enum":["wheelchair_accessible","elevator_access","accessible_parking","accessible_restroom","braille_signage","hearing_loop","sign_language","accessible_entrance","tactile_guidance","low_counters","accessible_seating","service_animals_welcome","wide_doorways","ramp_access","accessible_elevator","accessible_bathroom","large_print_menus","audio_description","mobility_equipment_rental"]}
    },
    "contact_phone":{"type":"string"},
    "contact_email":{"type":"string"},
    "website":{"type":"string"},
    "latitude":{"type":"number"},
    "longitude":{"type":"number"},
    "rating":{"type":"number","minimum":0,"maximum":5},
    "accessibility_rating":{"type":"number","minimum":0,"maximum":5},
    "review_count":{"type":"integer","minimum":0},
    "accessibility_score":{"type":"number","minimum":0,"maximum":100},
    "approved":{"type":"boolean","default":false},
    "featured":{"type":"boolean","default":false}
  },
  "required":["name","address","city","country","category"]
}

Review
{
  "name":"Review",
  "type":"object",
  "properties":{
    "venue_id":{"type":"string"},
    "user_name":{"type":"string"},
    "rating":{"type":"integer","minimum":1,"maximum":5},
    "accessibility_rating":{"type":"integer","minimum":1,"maximum":5},
    "comment":{"type":"string"},
    "disability_type":{"type":"string","enum":["mobility","visual","hearing","cognitive","multiple","other","none"]},
    "recommended_features":{"type":"array","items":{"type":"string"}},
    "improvement_suggestions":{"type":"string"},
    "helpful_count":{"type":"integer","default":0}
  },
  "required":["venue_id","user_name","rating","accessibility_rating","comment"]
}

Favorite
{
  "name":"Favorite",
  "type":"object",
  "properties":{
    "venue_id":{"type":"string"},
    "user_email":{"type":"string"},
    "notes":{"type":"string"},
    "visit_planned":{"type":"boolean","default":false},
    "tags":{"type":"array","items":{"type":"string"}}
  },
  "required":["venue_id","user_email"]
}

🔧 Developer Setup (exact steps)
# 1. Clone
git clone https://github.com/<your-org>/accessbnb.git
cd accessbnb

# 2. Install
npm install

# 3. Dev
npm run dev

# 4. Build for production
npm run build


Create .env.local (example):

REACT_APP_BASE44_API_URL=https://api.base44.app
REACT_APP_BASE44_API_KEY=YOUR_BASE44_KEY
REACT_APP_SUPABASE_URL=https://qtrypzzcjebvfcihiynt.supabase.co
REACT_APP_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
REACT_APP_SUPPORT_EMAIL=imransayyed001002@gmail.com


Security note: Use secret managers (Vercel/Netlify/Github Actions) for production credentials.

🔁 Seed Data & Sample Records (summary)

Provide a seed script which inserts:

Countries: IN, AE, DE, BR, MX, AU, ZA, GB, US, CA (with iso_code, flag_emoji, total_venues).

Cities: 2–4 per country (Mumbai, Delhi, Dubai, Berlin, São Paulo…).

20–30 Venues across categories with mix of approved:true and featured:true. Each venue must have accessibility_features and an image_url or fallback.

10–20 Reviews for key venues.

5–10 Favorite records for testing.

Design the seed script so it is idempotent (check existing by unique slug/name before insert).

🧭 Frontend: Page & Component Map (concise)

pages/Home.js — HeroSearch, Featured Venues, ExploreCountryChips, AccessibilityCategories, ResultsList.

pages/Places.js — filtering UI and results grid.

pages/AddVenue.js — 3-step flow (VenueBasicInfo, VenueAccessibility, VenuePreview).

pages/VenueDetail.js — VenueHero, AccessibilityFeatures, ReviewsList.

pages/Favorites.js — My favorites grid (login required).

pages/Profile.js — Profile card, tabs (My Reviews, My Favorites, My Venues).

pages/Contact.js — Contact form (SendEmail -> imransayyed001002@gmail.com).

components/layout/SiteFooter.js — footer with quick links and CTAs.

components/layout/MobileNav.js — slide-in navigation for mobile.

📌 Utility snippet — categoryImages.js (exact single-source mappings)
export const CATEGORY_IMAGE_MAP = {
  hotel: "https://qtrypzzcjebvfcihiynt.supabase.co/storage/v1/object/public/base44-prod/public/8b78f4866_generated-image1.png",
  park: "https://qtrypzzcjebvfcihiynt.supabase.co/storage/v1/object/public/base44-prod/public/626985899_generated-image2.png",
  hospital: "https://qtrypzzcjebvfcihiynt.supabase.co/storage/v1/object/public/base44-prod/public/f3767c72f_generated-image3.png",
  entertainment: "https://qtrypzzcjebvfcihiynt.supabase.co/storage/v1/object/public/base44-prod/public/b8276dbf1_generated-image4.png",
  transport: "https://qtrypzzcjebvfcihiynt.supabase.co/storage/v1/object/public/base44-prod/public/3c881e301_generated-image9.png",
  museum: "https://qtrypzzcjebvfcihiynt.supabase.co/storage/v1/object/public/base44-prod/public/c0e609927_generated-image.png",
  restaurant: "https://qtrypzzcjebvfcihiynt.supabase.co/storage/v1/object/public/base44-prod/public/8b78f4866_generated-image1.png",
  shopping: "https://qtrypzzcjebvfcihiynt.supabase.co/storage/v1/object/public/base44-prod/public/8b78f4866_generated-image1.png",
  other: "https://qtrypzzcjebvfcihiynt.supabase.co/storage/v1/object/public/base44-prod/public/8b78f4866_generated-image1.png"
};

const SYNONYM_MAP = {
  "medical": "hospital", "clinic": "hospital",
  "theater": "entertainment", "cinema": "entertainment",
  "inclusive restaurants": "restaurant", "accessible hotels": "hotel",
  "barrier-free parks": "park", "accessible shopping": "shopping"
};

export function normalizeCategory(raw) {
  const key = String(raw || "").toLowerCase().trim();
  return SYNONYM_MAP[key] || key;
}

export function getCategoryImage(category) {
  const key = normalizeCategory(category);
  return CATEGORY_IMAGE_MAP[key] || CATEGORY_IMAGE_MAP.other;
}

export function getVenueImage(venue) {
  if (!venue) return CATEGORY_IMAGE_MAP.other;
  if (venue.image_url) return venue.image_url;
  if (venue.image_urls && venue.image_urls.length) return venue.image_urls[0];
  return getCategoryImage(venue.category);
}

🔐 Authentication & Security

Authentication: Use Base44 platform auth. Redirect to login via:

User.loginWithRedirect(window.location.href)


Authorization: Protect actions (AddVenue, Favorite, Review) at UI and server levels.

Data validation: Validate incoming venue payloads server-side (when using custom APIs) to match JSON schema.

Secrets: Never store keys in repo. Use hosting secret store.

Uploads: Validate image content type and size before uploading to Supabase.

♿ Accessibility Checklist (developer & QA)

Semantic HTML (landmarks, headings in order).

Keyboard navigation across all pages and modals.

Focus visible styles (tailwind outline utilities).

ARIA roles for complex components (tabs, dialogs, sliders).

Contrast ratio >= 4.5:1 for body text, 3:1 for large text.

Forms must have associated labels and error states.

Images include meaningful alt text; decorative images use empty alt.

Test with screen reader (NVDA / VoiceOver) flows for search -> venue -> review.

Automated checks: axe-core in CI, Lighthouse accessibility audits.

✅ Testing Strategy

Unit tests: component rendering, utils (getCategoryImage), form validation.

Integration tests: AddVenue full flow, VenueDetail + write review.

E2E tests: Cypress for key user journeys (search & favorite).

Accessibility tests: run axe as part of CI for critical pages.

Sample commands:

npm run test       # unit tests
npm run test:e2e   # cypress (if configured)
npm run lint

⚙️ CI/CD (GitHub Actions sample)

Create .github/workflows/ci.yml:

name: CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v2
      - run: npm ci
      - run: npm run lint
      - run: npm run test --if-present
      - run: npm run build
      - name: Upload artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build
          path: build


Add a deploy job to Base44 or Vercel depending on hosting.

📦 Deployment Checklist

Add production environment variables in host (Base44/Vercel).

Ensure Supabase bucket is public for category images (or use signed URLs).

Run seed in staging and verify featured & approved flags.

Confirm OAuth redirect URIs for login.

Run accessibility audit on production build.

🗺 Roadmap & Priorities (12-month)

Q1 — Admin moderation dashboard, verified badges, improved seed data.
Q2 — Multi-language & locale-aware accessibility guidance.
Q3 — Offline caching & PWA support for favorites.
Q4 — Integration with accessibility NGOs / certification & mobile app (Expo).

🤝 Contribution Guidelines & Templates

Follow Conventional Commits (feat, fix, docs, chore).

Add tests for new features.

Small, focused PRs with a clear description.

Provide ISSUE_TEMPLATE.md and PULL_REQUEST_TEMPLATE.md in .github/ (I can generate these if you want).

🧾 Governance & Moderation

Venues added by users are approved:false by default. Moderators can review and set approved:true.

Use flags/reporting for incorrect data. Track moderator actions in an audit log (optional entity).

📄 LICENSE

MIT © 2025 Sayyed Imran — see LICENSE file in repo.

📬 Maintainer & Contact

Name: Imran Sayyed (project maintainer)

Email: imransayyed001002@gmail.com

Project: https://app--access-bn-b-c89a66e4.base44.app/
