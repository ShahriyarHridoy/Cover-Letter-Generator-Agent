# Cover Letter Agent — Web App / Chrome Extension

An AI-powered simple web app / chrome extension that researches a company, tailors your cover letter for each job application, and sends it directly via Gmail or exports it as a PDF — all without leaving your browser.

---

<img width="1234" height="1390" alt="image" src="https://github.com/user-attachments/assets/b5dd85ff-95d9-48a6-b5fd-cd705a866c24" />

## Demo
 
Open `index.html` in any modern browser. That's it. No build step, no server, no dependencies to install.

## How it works

1. You enter a company name, job title, and paste the job description
2. The extension searches the web for real information about the company (products, tech stack, culture)
3. Groq's AI uses that research to write a tailored cover letter specific to that company and role
4. You review both your template-filled letter and the AI version side by side, edit freely, then send or download

---

## Features

- **Live company research** — searches the web via Serper before writing, so the letter references real facts about the company rather than generic filler
- **AI tailoring** — Groq (llama-3.3-70b) rewrites your letter for each role using the research findings
- **Dual preview** — your editable letter on the left, AI suggestion on the right; copy the AI version across with one click
- **Email mode** — compose To/Subject/From, attach files, send directly via Gmail OAuth
- **PDF mode** — formatted letter preview with a header block, download as PDF via Chrome's print-to-PDF
- **Rich text editing** — bold, italic, underline, lists, and links in both your letter and the AI pane
- **Attachment manager** — upload PDF/DOC/DOCX files, toggle them on or off per send, remove files, set a default CV
- **Template system** — separate Email and PDF templates with `{variable}` substitution for company, position, name, email, phone, and a custom field
- **Serper key tester** — test your API key before generating to confirm it works
- **Open in window** — pop the extension out into a full resizable window for more comfortable editing
- **Persistent storage** — all settings, templates, and documents are saved locally in `chrome.storage.local`

---

## Setup

### 1. Load the extension in Chrome

1. Go to `chrome://extensions`
2. Enable **Developer mode** (toggle in the top-right corner)
3. Click **Load unpacked**
4. Select the `cover-letter-agent` folder

The extension icon will appear in your Chrome toolbar.

### 2. Get a Groq API key (required — free)

Groq is the AI engine that writes the cover letter.

1. Go to [console.groq.com](https://console.groq.com)
2. Sign up or log in
3. Go to **API Keys → Create API Key**
4. Copy the key (starts with `gsk_...`)

### 3. Get a Serper API key (optional — free, recommended)

Serper lets the extension search Google for real information about the company before writing. Without it the AI uses only its training knowledge, which may be outdated.

1. Go to [serper.dev](https://serper.dev)
2. Sign up — you get 2,500 free searches per month
3. Copy your API key from the dashboard
4. You can test it works using the **Test key** button in Settings before generating

### 4. Set up Gmail OAuth (optional — needed only to send emails)

Skip this if you only want to generate and download PDFs.

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project (or use an existing one)
3. Go to **APIs & Services → Library**, search for **Gmail API**, and enable it
4. Go to **APIs & Services → OAuth consent screen**
   - Choose **External**
   - Fill in an app name (e.g. "Cover Letter Agent")
   - Add your Gmail address as a test user
5. Go to **APIs & Services → Credentials → Create Credentials → OAuth Client ID**
   #### for Chrome Extension
   - Application type: **Chrome Extension**
   - Extension ID: open `chrome://extensions`, find Cover Letter Agent, copy the ID shown there
   - Paste it as the Application ID
  #### for Web application
   - Application type: **Web application**
   - Under **Authorised redirect URIs**, add the exact URL you're hosting the app at
     - Local: `http://localhost:5500/index.html` (or wherever your dev server serves it)
     - Hosted: your full production URL, e.g. `https://yourdomain.com/`
7. Copy the **Client ID** (ends with `.apps.googleusercontent.com`)

### 5. Configure the extension

1. Click the extension icon in the Chrome toolbar
2. Click the **⚙ Settings** icon (top right of the popup)
3. Paste your **Groq API key**
4. Paste your **Serper API key** — click **Test key** to verify it works
5. Paste your **Gmail OAuth Client ID** (if using email sending)
6. Click **Save settings**

---

## Usage

### Generate a cover letter

1. Open the extension and go to the **Compose** tab
2. Enter the **company name** and **job title**
3. Paste the job description or key requirements into the text area
4. Optionally expand **More variables** to fill in your name, email, phone, or a custom field
5. Choose **Send Email** or **Generate PDF** mode
6. Click **Research & Preview** — the extension will:
   - Search the web for information about the company (if Serper key is set)
   - Send the research + your background + the job description to Groq
   - Display the result in the dual-preview panel

### In the preview

**Left pane — your letter (editable)**
- Your saved template with variables filled in
- Edit freely using the rich text toolbar (bold, italic, underline, lists, links)
- Fill in To, Subject, and From before sending

**Right pane — AI suggestion (read-only)**
- The AI-written letter based on company research
- Click **← Use this** to copy it into your editable pane

### Attach files

The attachment area sits below the editor. You can:
- **Toggle** any file on (✓) or off (+) per send — files shown in blue are attached, grey ones are not
- **Upload** a new file directly from the preview using the **Upload** button in the attachment header
- **Remove** a file permanently from your library using the × button on each chip
- Files are saved across sessions; the star (★) marks your default CV

### Send via Gmail

1. Fill in the **To** email address and **Subject**
2. Select which files to attach using the chip toggles
3. Click **Send via Gmail** — Chrome will ask you to authorise Gmail access on the first send

### Download as PDF

In PDF mode, click **Download PDF** — a print-ready page opens in a new tab. Click **Save as PDF** or use `Ctrl+P` and choose "Save as PDF" as the destination.

### Templates

Go to the **Email Template** or **PDF Template** tab to edit your base letter. Supported variables:

| Variable | Description |
|---|---|
| `{company}` | Company name |
| `{position}` | Job title |
| `{senderName}` | Your full name |
| `{email}` | Your email address |
| `{phone}` | Your phone number |
| `{custom1}` | Any extra value you define |

### Documents library

The **Documents** tab stores your uploaded files. You can:
- Upload multiple CVs, portfolios, or references
- Mark one as the **Default CV** — it will be pre-selected on every new preview
- Delete files you no longer need

### Open in window

Click the **⤢** icon (next to the settings gear) to pop the extension out into a full resizable window — useful when editing longer letters or working on a larger screen.

---

## File structure

```
cover-letter-agent/
├── manifest.json          # Chrome extension config (MV3)
├── popup.html             # Main UI
├── print.html             # PDF print page
├── src/
│   ├── popup.js           # All UI logic, API calls, storage
│   ├── popup.css          # Styles
│   ├── background.js      # Service worker — handles Serper requests and auth
│   ├── print.js           # PDF page script
│   └── print.css          # PDF page styles
└── icons/
    ├── icon16.png
    ├── icon48.png
    └── icon128.png
```

---

## Privacy & security

- **API keys** are stored in `chrome.storage.local` — they never leave your browser except when making direct calls to Groq and Serper
- **Serper searches** are routed through the background service worker (not the popup) to avoid CORS issues — no third-party proxy is involved
- **Gmail OAuth** uses Chrome's built-in `chrome.identity` API — the extension only requests the `gmail.send` scope and cannot read, search, or delete your emails
- **Documents** (your CV, attachments) are stored locally in `chrome.storage.local` as base64 — they are never uploaded anywhere except as email attachments when you explicitly click Send
- No analytics, no telemetry, no ads

---
