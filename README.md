# 🧰 LocalKit

A zero-install, zero-dependency local productivity suite that runs entirely in your browser — no server, no account, no setup required.

> **Think of it as a developer's Swiss Army knife — just open `index.html` and start working.**

---

## Tools

| Tool | Status | Description |
|------|--------|-------------|
| ⚡ [API Client](#-api-client) | ✅ Ready | Send HTTP requests, inspect responses, manage collections |
| ✅ [Task Tracker](#-task-tracker) | ✅ Ready | Create tasks with dates, notes, and status tracking — all local |
| 📋 JSON Formatter | 🔜 Coming Soon | Paste, format, validate JSON — tree view, diff mode |
| 🕐 Cron Helper | 🔜 Coming Soon | Build and explain cron expressions visually |
| 🔐 Encoder / Decoder | 🔜 Coming Soon | Base64, URL encoding, JWT decoder, hash generator |
| 📝 Markdown Preview | 🔜 Coming Soon | Split-pane Markdown editor with HTML export |

---

## Getting Started

No installation. No build step. No dependencies.

```
1. Download or clone this repo
2. Open index.html in your browser
3. Pick a tool and start working
```

> **Tip:** If you encounter fetch errors when opening via `file://`, serve through a local HTTP server:
> ```bash
> npx serve .
> # or
> python -m http.server
> ```

---

## ⚡ API Client

A lightweight HTTP client — think Postman, but it's a single HTML file at [`tools/api-client.html`](tools/api-client.html).

### Features

#### Request Builder
- Select HTTP method: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `OPTIONS`, `HEAD`
- URL input with automatic `https://` fallback
- **Query Parameters** editor — key-value pairs appended to the URL
- **Headers** editor — custom request headers
- **Body** editor — supports:
  - `None` — no body
  - `JSON` — raw JSON with `Content-Type: application/json` auto-set
  - `Form` — URL-encoded form fields (`application/x-www-form-urlencoded`)
  - `Raw` — arbitrary text body

#### Response Viewer
- **Pretty** — syntax-highlighted, formatted JSON output
- **Raw** — plain unformatted response text
- **JSON** — interactive collapsible tree view
- **Headers** — response header key-value table
- Status code + status text, response time (ms), and response size display

#### Collections
- Create named collections to group related requests
- Save any request into a collection (method, URL, headers, params, body all stored)
- Load a saved request back into the builder with a single click
- Update (overwrite) an existing saved request in place
- Delete individual requests or entire collections
- Rename collections

#### Import / Export
- Export any collection as a downloadable `.json` file
- Import a `.json` file to restore or merge collections

### Usage

#### Sending a Request

1. Select an HTTP method from the dropdown
2. Enter a URL (e.g. `https://api.example.com/users`)
3. Optionally add **Params**, **Headers**, or a **Body**
4. Click **Send** (or press `Enter` in the URL field)
5. View the response in the **Pretty / Raw / JSON / Headers** tabs

#### Saving a Request

1. Select a collection from the **Save to collection** dropdown (or create one with `＋`)
2. Enter a name for the request
3. Click **Save as new**

To overwrite an existing saved request, load it from the sidebar first, make your changes, then click **Update**.

#### Managing Collections

| Action | How |
|--------|-----|
| Create collection | Click `＋` in the Collections sidebar header |
| Rename collection | Hover over a collection → click `✏` |
| Export collection | Hover over a collection → click `⬇` |
| Delete collection | Hover over a collection → click `✕` |
| Import collection | Click **⬆ Import Collection** at the bottom of the sidebar |

#### Collection File Format

Collections are stored in `localStorage` and exported/imported as `.json` files:

```json
{
  "collections": {
    "<uuid>": {
      "id": "<uuid>",
      "name": "My API",
      "createdAt": "2026-04-17T13:22:43.226Z",
      "requests": [
        {
          "id": "<uuid>",
          "name": "Get Users",
          "method": "GET",
          "url": "https://api.example.com/users",
          "headers": [{ "key": "Authorization", "value": "Bearer token" }],
          "params":  [{ "key": "page", "value": "1" }],
          "body": "",
          "bodyType": "none",
          "bodyForm": []
        }
      ]
    }
  }
}
```

A [`tools/api-sample.json`](tools/api-sample.json) export file is included for reference.

---

## ✅ Task Tracker

A local task management tool at [`tools/task-tracker.html`](tools/task-tracker.html). Create, edit, and track tasks with project grouping, status labels, mandatory due dates, and notes — all saved to `localStorage`.

### Features

- **Add / Edit / Delete** tasks via a clean modal form
- **Project sidebar** — left navigation panel listing all projects; click to filter tasks by project; create new projects with `＋`; delete projects with `✕` (tasks become unassigned)
- **Project select** — when adding or editing a task, select the project from a dropdown populated from the sidebar project list
- **Status labels** — Pending, In Progress, Done, Blocked (colour-coded badges)
- **Target date** — mandatory date picker, defaults to today; overdue tasks highlighted in red with a `⚠` prefix
- **Notes** — multi-line notes per task
- **Done tasks** — visually de-emphasised with strikethrough title
- **List view** — tabular rows with columns: Title · Project · Status · Due Date
- **Filter by status** — All / Pending / In Progress / Done / Blocked
- **Sort by** — Created (newest) · Due date (soonest) · Due date (latest) · Project (A→Z) · Title (A→Z)
- **Export** — download all tasks as a `.json` file (⬇ Export button in header)
- **Import** — load tasks from a `.json` file, merging with existing data (⬆ Import button in header)
- **Persistent** — tasks saved under `localkit_tasks`; projects saved under `localkit_projects`

### Layout

```
┌──────────────────────────────────────────────────────────┐
│  Header: ✅ Task Tracker          [⬇ Export] [⬆ Import]  │
├─────────────────┬────────────────────────────────────────┤
│  PROJECTS       │  Status ▾  Sort by ▾        N tasks    │
│  ─────────────  │  ＋ Add Task                            │
│  All Tasks      │  ──────────────────────────────────    │
│  Backend API    │  Title         Project  Status  Due     │
│  Sprint 5       │  Write tests   Backend  Pending Apr 25  │
│  ＋ New Project  │  Fix bug #42   Sprint5  In Prog Apr 20  │
└─────────────────┴────────────────────────────────────────┘
```

### Usage

1. Open `index.html` → click **Task Tracker**
2. Click `＋` in the **Projects** sidebar to create a project
3. Click **＋ Add Task** (or press `N`) to create a task
4. Fill in title (required), select a project from the dropdown, add notes, set status, and target date (required, defaults to today)
5. Click **Create Task** to save
6. Click any task row (or press `Enter` on a focused row) to edit or delete it
7. Click a project in the sidebar to filter tasks; click **All Tasks** to see everything
8. Use the **Status** filter and **Sort by** dropdown to further organise your view
9. Click **⬇ Export** to download all tasks as JSON; **⬆ Import** to load tasks from a file

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `N` | Open Add Task modal |
| `Enter` | Open edit modal for focused row |
| `Escape` | Close modal |

### Managing Projects

| Action | How |
|--------|-----|
| Create project | Click `＋` in the Projects sidebar header |
| Delete project | Hover over a project → click `✕` (tasks become unassigned) |
| Filter by project | Click any project name in the sidebar |
| Show all tasks | Click **All Tasks** at the top of the sidebar |

### Export / Import File Format

Tasks are exported as a `.json` file with the following schema:

```json
{
  "tasks": [
    {
      "id": "<uuid>",
      "title": "Write unit tests",
      "project": "Backend API",
      "notes": "Cover edge cases for the parser module.",
      "status": "in-progress",
      "targetDate": "2026-04-25",
      "createdAt": "2026-04-18T15:30:00.000Z"
    }
  ]
}
```

### Data Schema

Tasks are stored as a JSON array in `localStorage` under key `localkit_tasks`:

```json
[
  {
    "id": "<uuid>",
    "title": "Write unit tests",
    "project": "Backend API",
    "notes": "Cover edge cases for the parser module.",
    "status": "in-progress",
    "targetDate": "2026-04-25",
    "createdAt": "2026-04-18T15:30:00.000Z"
  }
]
```

---

## Browser Compatibility

| Browser | Minimum Version |
|---------|----------------|
| Chrome  | 92+            |
| Firefox | 95+            |
| Edge    | 92+            |
| Safari  | 15.4+          |

> **CORS Notice:** The API Client uses the browser's native `fetch()` API. Cross-origin APIs must respond with appropriate CORS headers, or you'll need to use a CORS proxy.

---

## Architecture

LocalKit is a **collection of self-contained single-file HTML tools** served from a shared launcher:

```
index.html              ← Tool launcher / hub
tools/
├── api-client.html     ← API Client (complete)
├── api-sample.json     ← Example collection export
└── task-tracker.html   ← Task Tracker (complete)
```

Each tool is fully self-contained:
```
tools/<tool>.html
├── <style>    All CSS — dark theme, layout, components
├── <body>     Application markup
└── <script>   All JavaScript — vanilla ES2020+, no frameworks
```

**Key technical choices:**

| Concern | Approach |
|---------|----------|
| HTTP requests (API Client) | `fetch()` API with `mode: 'cors'` |
| State management | Single `appState` object per tool, plain JS |
| Persistence | `localStorage` under namespaced keys |
| Syntax highlighting | Regex-based colorizer — zero CDN dependencies |
| JSON tree view | Recursive DOM builder with collapse/expand |
| Unique IDs | `crypto.randomUUID()` |
| Export | `Blob` + `URL.createObjectURL()` |
| Import | `FileReader` API |

---

## Known Limitations

- **CORS** — The browser will block requests to APIs that don't allow cross-origin requests. Use a CORS proxy or test against a CORS-enabled API.
- **`file://` protocol** — Some browsers restrict `fetch()` when opening HTML files directly from disk. Use a local HTTP server if you run into this.
- **No WebSocket or SSE** — Only standard HTTP request/response via `fetch()` is supported by the API Client.
- **Storage limit** — `localStorage` is capped at ~5 MB per origin, which is more than enough for all tool data.

---

## License

This project has no external runtime dependencies and ships as plain HTML files. Do whatever you want with it.