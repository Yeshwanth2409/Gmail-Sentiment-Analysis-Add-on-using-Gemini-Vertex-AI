# Gmail Sentiment Analysis Add-on (Gemini + Vertex AI + Apps Script)

A Google Workspace **Gmail add-on** that analyzes the sentiment of email content using **Gemini on Vertex AI** and surfaces a quick label (e.g., *Negative / Neutral / Positive*) right inside Gmail. Built as a personal project with **your own Google account** and **your own GCP project**.

---

## ✨ Features

- One-click **sentiment analysis** for the currently opened Gmail message
- Powered by **Vertex AI (Gemini)** via REST
- Secure OAuth with **your** Google account (no shared lab creds)
- Optional logging to **BigQuery** for analytics

---

## 🧱 Architecture (High Level)

Gmail ↔ **Gmail Add-on (Apps Script)** → **Vertex AI (Gemini)**  
                                            ↘ *(optional)* → **BigQuery**

---

## 🔧 Prerequisites

1. **Google account** (use your own; this is a personal project).
2. **Google Cloud Project** you own (or create one at console.cloud.google.com).
3. **APIs to enable** in your GCP project:
   - Vertex AI API
   - Cloud Resource Manager API (usually enabled)
   - *(Optional)* BigQuery API (if you want to log results)
4. A **billing account** attached to your GCP project *(Vertex AI/Gemini usage may incur small costs; stay within free tiers where available)*.

---

## 🗂️ Project Structure

This is a **Google Apps Script** project with a minimal file set:
apps-script/
├─ Code.gs # Main add-on code (UI + Gmail event + Vertex AI call)
└─ appsscript.json # Manifest (add-on config, Gmail trigger, scopes, etc.)


> You will **create an Apps Script project** and paste `Code.gs` and `appsscript.json` into it (instructions below).

---

## 🚀 Setup — Apps Script & Manifest

1. Open **script.google.com** → `New project`.
2. **Rename** the default file to `Code.gs` and paste the **Code.gs** content from this README’s code section.
3. In the Apps Script editor, go to **View → Show manifest file**.  
   - This opens `appsscript.json`. Replace its contents with the **appsscript.json** from this README’s code section.
4. Click **Project Settings** (gear icon) → set your **Google Cloud Platform (GCP) project** number (use an existing project with Vertex AI API enabled).

> **Where to use each file**
> - `Code.gs` → paste into the default server-side file in the Apps Script editor.
> - `appsscript.json` → paste into the **manifest** (View → Show manifest file).

---

## 🔐 OAuth Scopes

The manifest already includes:
- `https://www.googleapis.com/auth/gmail.addons.execute`
- `https://www.googleapis.com/auth/gmail.addons.current.message.readonly`
- `https://www.googleapis.com/auth/cloud-platform`
- `https://www.googleapis.com/auth/script.external_request`

These let the add-on read the current email and call Vertex AI.

---

## ⚙️ Configuration (Vertex AI)

Update these constants in `Code.gs`:

```js
const PROJECT_ID   = 'YOUR_GCP_PROJECT_ID';
const LOCATION     = 'us-central1'; // or your chosen Vertex AI region
const MODEL_NAME   = 'gemini-1.5-flash'; // or another available Gemini model
