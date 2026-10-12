# PlotoraAI UI redesign preview

## Included
- `index.html`: minimal white SaaS workspace with colourful charts and eight agent cards.
- `api/generate.js`: preserved Groq-backed generation endpoint from the uploaded project.
- `vercel.json`: routes `/` to the static page and `/api/generate` to the function.

## Deploy
1. Back up your current GitHub repository.
2. Copy these files into the repository, preserving `api/generate.js` in the `api/` folder.
3. Ensure `GROQ_API_KEY` is set in Vercel Project Settings → Environment Variables.
4. Deploy a preview branch before merging to production.

## Important limitations
- The dashboard chart figures and sample leads are illustrative demo data.
- Lead additions exist only in the current page session; no shared database is connected.
- Only Marketing Studio calls the existing `/api/generate` endpoint.
- The other seven agents are interface placeholders until their backend workflows and integrations are implemented.
- Review API access controls before production. The existing handler uses wildcard CORS; the UI redesign has not changed that handler's security behaviour.
