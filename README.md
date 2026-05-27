# TED Staff Reviews — Preview Application

A secure, standalone single-page application (SPA) built with React for managing TED employee performance reviews, goals, and confidential counseling logs. 

This file is a self-contained prototype designed to be either run locally for instant previewing or deployed to a lightweight web host to integrate with monday.com.

---

## 🚀 Quick Start (Local Preview)

Because the application is completely self-contained in a single HTML file, you do not need to install Node.js, npm, or run a complex build process.

1. **Download** the `TED-Staff-Reviews-Preview.html` file.
2. **Double-click** the file to open it directly in any modern web browser (Chrome, Safari, Edge, Firefox).
3. **Start using it!** > 💡 **Data Persistence:** Data you enter (adding staff, scheduling reviews, creating performance logs) is saved directly to your browser's **Local Storage**. It will persist even if you refresh the page or close the browser tab.

---

## 🛠️ Features Implemented

* **Roster Management:** View, add, and archive/terminate staff members from the active roster.
* **Performance Form Engine:** Dynamic performance review modal mapping across four core sections (Efficiency, Quality, Dependability, Teamwork) with 1–5 scale metrics and qualitative notes fields.
* **Peer & Reviewer Input:** Prominent fields to aggregate multi-source feedback across the studio team.
* **Goal Tracking:** Set, carry forward, or mark complete individual employee goals across review periods.
* **🔒 Confidential Counseling Log:** A restricted-view incident tracking workspace for attendance, conduct, and policy notes with automated upcoming follow-up trackers.
* **Live Form Designer:** An administrative layout view to add, remove, or modify default questions and rating criteria on the fly.
* **Data Portability:** Includes a **JSON Data Export** tool in the Settings panel to backup configurations or share database snapshots.

---

## 🗂️ Project Architecture & Tech Stack

This file uses an in-browser compilation stack to stay ultra-portable:
* **Frontend Library:** React 18.2 (Loaded via CDN)
* **Compiler:** Babel Standalone (Compiles JSX directly inside the browser)
* **Styling:** Custom CSS Custom Properties leveraging variable design tokens (`--color-background-primary`, etc.) matching TED brand guidelines.
* **Typography:** Google Fonts (`DM Sans` and `DM Mono`).

---

## 🔄 Moving to Production: monday.com Integration

To transform this local file into a collaborative team utility connected to your **monday.com** workspace, follow these steps:

### Step 1: Host the Frontend (Free Options)
Because monday.com cannot host and execute raw HTML application code natively, drag and drop this file folder onto a free static web hosting provider:
* [Netlify](https://www.netlify.com/) (Highly recommended: zero setup drag-and-drop)
* [Vercel](https://vercel.com/)
* [GitHub Pages](https://pages.github.com/)

### Step 2: Swap Local Storage for API Pipelines
Currently, the application reads/writes via the `storeGet` and `storeSet` wrapper functions inside the code. To sync with monday.com:

1. Create your target boards inside **monday.com** (e.g., a "Staff Board", a "Reviews Board", and a "Counseling Log Board").
2. Set up an automation webhook in **Make.com** or **n8n.io** to act as a receiver.
3. Modify the JavaScript script block at the top of the HTML file to shift data via a standard `fetch()` payload to your webhook or directly via monday’s GraphQL API:

```javascript
// Replace LocalStorage logic with live API integration
async function storeSet(key, val) {
  await fetch("YOUR_AUTOMATION_WEBHOOK_URL", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ key, data: val })
  });
}
