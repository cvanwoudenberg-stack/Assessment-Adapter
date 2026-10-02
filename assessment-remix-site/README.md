# Assessment Remix

A deployable prototype for adapting high-school assessments. The browser reads DOCX/TXT files and retains their main structure in the preview. Adaptation requests go through a Vercel serverless function to the Gemini API.

## Gemini model selection

The server uses Google's `gemini-flash-latest` alias by default. Google documents this as an alias that switches to the latest release of the Flash model family. The current Gemini API pricing page lists free-tier access for Gemini Flash models. The app does not pin a numbered model release. An optional `GEMINI_MODEL` server environment variable can override the alias; leave it unset to follow Google's alias.

For a no-charge prototype, create a Gemini API key for a Google AI Studio project that is using the unpaid tier, and do not link a paid Cloud Billing account to that project. If Google's latest alias is unavailable to the unpaid project, the request will fail instead of the code automatically switching to a paid model. Free quotas and model availability can change.

## Deploy on Vercel

1. Put the contents of this folder in a GitHub repository. The `index.html` file should be at the repository root (or select this folder as the Vercel project root).
2. Import the repository at Vercel and deploy it as a static site. No build command or package installation is needed.
3. Create a Gemini API key in Google AI Studio for an unpaid-tier project.
4. In Vercel, open the project settings and add `GEMINI_API_KEY` as an environment variable for Preview and Production. Do not add the key to `index.html`, commit it to GitHub, or expose it in a browser variable.
5. Redeploy so the server function receives the environment variable.
6. Open the deployment, upload a sample DOCX or TXT file, choose adaptations, and generate a draft. The API call is made only after the teacher presses the generate button.

The function is at `api/adapt.js`; Vercel exposes it as `/api/adapt`.

## Local setup

Use Vercel's local development command after installing its CLI, and provide `GEMINI_API_KEY` through a local environment file that is excluded from Git. Never commit a real API key. The production deployment only needs the `GEMINI_API_KEY` environment variable; `GEMINI_MODEL` is optional.

## Privacy and prototype limits

- The unpaid Gemini API terms allow Google to use submitted prompts and responses to improve Google products. The page warns teachers not to include student-identifying information. Check local school policy before sending assessment content.
- Assessment files are read in the browser. Text is sent to Gemini only when Generate is clicked. The app does not save the uploaded file or draft.
- The public endpoint has request-size and input-count checks, but no user sign-in or durable per-user rate limit. Share only with trusted testers; add access control and abuse protection before broader use.
- AI output is a draft. Teachers must verify accommodations, formatting, content accuracy, and that the intended skill is still being assessed.
