# Interview AI

An AI-powered interview preparation tool built on the MERN stack. Paste in a job description plus your resume (or a quick self-description), and it generates a tailored interview report — likely technical/behavioral questions, skill gaps, and a day-by-day prep plan — using the Gemini API. It can also generate and download a tailored, AI-written resume as a PDF.

## Tech Stack

- **Frontend:** React 19 + Vite, React Router, Axios, Sass
- **Backend:** Node.js + Express 5, MongoDB (Mongoose), JWT auth (cookie-based)
- **AI:** Google Gemini API (`@google/genai`)
- **PDF:** `pdf-parse` (reading uploaded resumes), Puppeteer (generating downloadable resume PDFs)

## Project Structure

```
interview-ai-yt-main/
├── Backend/     # Express API, MongoDB models, auth, AI service
└── Frontend/    # React app (Vite)
```

## Prerequisites

- Node.js v18+ (v20+ recommended)
- MongoDB running locally, or a free MongoDB Atlas cluster
- A Google Gemini API key ([get one here](https://aistudio.google.com/apikey))

## Setup

### 1. Clone and install

```bash
git clone <your-repo-url>
cd interview-ai-yt-main

cd Backend
npm install

cd ../Frontend
npm install
```

### 2. Configure environment variables

Create `Backend/.env` (see `Backend/.env.example`):

```env
MONGO_URI=mongodb://localhost:27017/interview-ai
JWT_SECRET=some_long_random_secret_string
GOOGLE_GENAI_API_KEY=your_gemini_api_key_here
```

### 3. Install Puppeteer's Chrome binary

The resume-PDF-download feature uses Puppeteer, which needs its own Chromium browser downloaded separately (this can get skipped by npm's install-script safety checks):

```bash
cd Backend
npx puppeteer browsers install chrome
```

> If this repeatedly fails to leave a working `chrome.exe` behind, your antivirus may be silently deleting it right after extraction. Add an exclusion for `%USERPROFILE%\.cache\puppeteer` in Windows Defender (or your AV) and try again.

### 4. Run it

In two separate terminals:

```bash
# Terminal 1
cd Backend
npm run dev        # http://localhost:3000

# Terminal 2
cd Frontend
npm run dev        # http://localhost:5173
```

Open `http://localhost:5173`, register an account, and try it out.

## Notes

- CORS on the backend is currently hardcoded to allow only `http://localhost:5173`.
- The frontend's API base URL is hardcoded to `http://localhost:3000`. Update both if you deploy or change ports.
- The AI model in `Backend/src/services/ai.service.js` is set to `gemini-3.1-flash-lite` (a stable, non-preview model) to avoid the frequent `503 UNAVAILABLE` overload errors seen on preview models.
- Only PDF resumes are actually parsed server-side; the upload UI also accepts `.docx` but that path isn't implemented yet.

## License

ISC
