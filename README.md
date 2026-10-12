# PlotoraAI — Gemini + Cloudflare Workers AI integration

This package updates the white PlotoraAI dashboard prototype and the existing Vercel `/api/generate` endpoint so all eight AI assistants can request text from either Google Gemini or Cloudflare Workers AI.

## What is connected in this build

- Lead Generation
- Sales
- Property Matching
- Marketing
- Follow-up
- Appointment
- Customer Relationship
- Business Intelligence

Each agent can send a task to either provider from the dashboard. This is an AI drafting/analysis integration; it does **not** itself connect a database, capture live leads, read property inventory, send WhatsApp/email messages, access a calendar, or save CRM records. Those require separate integrations and authentication.

## Vercel environment variables

Add these in **Vercel → Project → Settings → Environment Variables**. Select Production and Preview (and Development if you use it). Then redeploy.

Required for Gemini:

- `GEMINI_API_KEY` — your Google AI Studio API key. The code also accepts `GOOGLE_API_KEY` for compatibility.
- Optional: `GEMINI_MODEL` — defaults to `gemini-3-flash`; set this to a model available to your Google AI Studio project if needed.

Required for Cloudflare Workers AI:

- `CLOUDFLARE_ACCOUNT_ID` — your Cloudflare account ID.
- `CLOUDFLARE_API_TOKEN` — a token with Workers AI permissions. The code also accepts `CLOUDFLARE_API_KEY` as an alternate variable name.
- Optional: `CLOUDFLARE_MODEL` — defaults to `@cf/meta/llama-3.3-70b-instruct-fp8-fast`; set to a model enabled for your account if needed.

Keep these secrets only in Vercel environment variables. Never put them in `index.html`, commit them to GitHub, or paste them into chat. If a key was exposed, rotate it.

## Safe deployment steps

1. Download and unzip this package.
2. In your existing GitHub repository, create a new branch such as `integrate-gemini-cloudflare`.
3. Replace `index.html`, `api/generate.js`, and `vercel.json` with the files from this package. Keep the `api` folder path exactly as shown.
4. Commit and push the branch.
5. In Vercel, confirm the repository/branch preview deployment is created.
6. Add the environment variables listed above to Preview, then redeploy the preview.
7. Open the preview URL, open each of the eight agents, select Gemini, run a harmless test, then repeat with Cloudflare Workers AI.
8. Only merge to the production branch after both providers work.

## Test prompts

- Lead Generation: `Create a qualification checklist for a buyer enquiry. Do not claim the enquiry was saved.`
- Sales: `Draft five discovery questions for a buyer seeking a 3 BHK in Mohali.`
- Property Matching: `Explain what inventory fields you need to match a buyer. Do not invent listings.`
- Marketing: `Write an Instagram caption using only these facts: 3 BHK, Mohali, ₹85 lakh.`
- Follow-up: `Draft a polite follow-up message after a site visit; do not claim it was sent.`
- Appointment: `Draft a message proposing three site-visit slots; do not claim a booking was made.`
- Customer Relationship: `Turn this supplied note into needs, preferences, and next steps: [insert fictional note].`
- Business Intelligence: `Explain how to calculate lead-to-visit conversion from supplied counts; do not invent counts.`

## Important production gap

The supplied app is a prototype and has no authentication, tenant isolation, durable database, rate limiting, or quota enforcement. Before public launch, protect `/api/generate` against abuse and add user authentication, per-workspace authorization, rate limits, usage limits, and privacy-safe logging. API keys remain server-side in this code, but a public endpoint can still be abused if left unprotected.
