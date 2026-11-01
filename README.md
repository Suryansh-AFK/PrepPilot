# PrepPilot

A responsive interview preparation workspace. It connects real projects and a target job to a learning path, study notes, question practice, answer reflection, and a group discussion room.

## Features

- Responsive overview, roadmap, practice room, study notes, GD room, and materials editor.
- Sample EPAM-style profile and SpareParts project for demo review; replace it with your own materials.
- Question rotation, answer capture, session history, confidence milestones, editable topics/projects, GD topic editor, and a working 60-second timer.
- Browser-local data storage works without an account.
- Google sign-in creates a private, user-scoped MongoDB workspace once configured.
- Resume PDFs are parsed in memory; the PDF file itself is not retained.
- Optional server-side OpenAI structured generation for roadmaps, questions, explanations, feedback, and GD briefs.

## Local setup

1. Install Node.js 20.19+ or 22.12+.
2. Open this folder in VS Code and run `npm install`.
3. Copy `.env.example` to `.env`. Set `MONGODB_URI`, `JWT_SECRET` (at least 32 random characters), `GOOGLE_CLIENT_ID`, and `VITE_GOOGLE_CLIENT_ID` (same Google OAuth Web Client ID).
4. In Google Cloud, add `http://localhost:4173` to the OAuth client's authorized JavaScript origins.
5. To enable AI features, set server-side `OPENAI_API_KEY` and optionally `OPENAI_MODEL`. Never expose the API key in a `VITE_` variable. OpenAI API calls are billed to the API-key owner.
6. Run `npm run dev` and open `http://localhost:4173`.

Without MongoDB/Google/AI configured, the demo data, practice flow, confidence markers, notes, timer, and browser-local saving work. Accounts, cross-device sync, and AI generation require the matching credentials.

For local MongoDB, use `mongodb://127.0.0.1:27017/preppilot`. Atlas also works; use a dedicated least-privilege database user and store its connection string only in the server environment.

## Vercel

The Vite frontend builds to `dist/`; `api/[...path].js` exposes the Express API as a Vercel Function. Configure these Production variables:

- Server-only encrypted: `MONGODB_URI`, `JWT_SECRET`, `GOOGLE_CLIENT_ID`, and (optional) `OPENAI_API_KEY`, `OPENAI_MODEL`.
- Build-time: `VITE_GOOGLE_CLIENT_ID`.

Set the project's Node.js version to 22.x. Add the production domain to Google OAuth's authorized JavaScript origins. Redeploy after changing build-time variables.

## Data model and privacy

- `User`: verified Google subject, normalized email, display name, and profile image URL.
- `Workspace`: one learner's profile/materials, projects, topics, and practice history.
- Resume PDF bytes exist only in memory during extraction. Extracted resume text stays in browser storage until the learner signs in and syncs it.
- The demo answer signal is a lightweight answer-length/keyword heuristic, not AI grading. AI feedback is available only when configured and clearly labeled.

## Main folders

- `src/`: responsive React app
- `server/`: Express API, MongoDB models, Google sign-in, PDF parsing, AI routes
- `api/`: Vercel Function entry point
- `.env.example`: environment variable names only, no credentials
