SpendWise — Expense Tracker

   1. Project Title
SpendWise — A dynamic single-page expense tracking web application.

---

   2. Problem Statement
Managing personal finances is a common challenge for students and working professionals. Most people struggle to keep track of where their money goes each month, making it difficult to identify spending patterns or cut unnecessary costs. SpendWise solves this by providing a clean, intuitive web interface where users can log, categorise, edit, and delete their expenses — and instantly see visual breakdowns of their spending through charts and summary statistics on a live dashboard.

---

   3. Technical Stack

| Layer       | Technology                          |
|-------------|-------------------------------------|
| Frontend    | React 18 (Vite), Recharts           |
| Styling     | Plain CSS with inline React styles  |
| Routing     | React state-based SPA (no page reloads) |
| Backend     | FastAPI (Python)                    |
| Database    | MongoDB (via Motor async driver)    |
| API         | RESTful JSON API on port 8000       |
| Dev Server  | Vite (port 5173) with proxy to API  |
| Deployment  | Run locally — uvicorn + vite dev    |

---

   4. Feature List

- Dashboard overview — Summary cards showing this month's spending, last month's spending, and total spending across all time
- Monthly trend bar chart — Visual bar chart showing spending across the last 6 months
- Category donut chart — Doughnut chart breaking down spending by category for the current month
- Recent transactions list — Five most recent expenses shown on the dashboard with a "View All" link
- Full expense table — Complete paginated list of all expenses sorted by date
- Dynamic search filtering — Live search bar that filters expenses by title or description as you type
- Category filtering — Dropdown to filter expenses by category (Food & Dining, Health, Shopping, etc.)
- Date filtering — Date picker to filter expenses by a specific date
- Add expense — Modal form to create a new expense with title, description, amount, date, and category
- Edit expense — Inline edit via modal, pre-populated with existing values
- Delete expense — One-click delete with confirmation prompt
- Full CRUD operations — All four database operations (Create, Read, Update, Delete) connected to MongoDB
- Single-page application — No full page reloads; all navigation and data updates happen dynamically
- Error handling — API failure shows a user-friendly error screen with a retry button
- Sample data seeding — seed.py script to pre-populate the database with 17 realistic sample expenses

---

   5. Folder Structure

```
spendwise/
├── README.md                        ← This file
│
├── backend/
│   ├── main.py                      ← FastAPI app: all API routes (CRUD + stats)
│   ├── seed.py                      ← Script to populate MongoDB with sample data
│   └── requirements.txt             ← Python dependencies
│
└── frontend/
    ├── index.html                   ← Single HTML entry point
    ├── package.json                 ← Node dependencies and scripts
    ├── vite.config.js               ← Vite config + proxy to backend
    └── src/
        ├── main.jsx                 ← React root mount
        ├── App.jsx                  ← Root component: state, data fetching, routing
        ├── index.css                ← Global base styles
        ├── services/
        │   └── api.js               ← All fetch() calls to the FastAPI backend
        └── components/
            ├── Sidebar.jsx          ← Left nav sidebar + New Expense button
            ├── Dashboard.jsx        ← Stats cards, bar chart, donut chart, recent tx
            ├── ExpensesPage.jsx     ← Full expense table with search/filter/edit/delete
            └── ExpenseModal.jsx     ← Add / Edit expense modal form
```

---

   6. Challenges Overcome

One of the first challenges was handling the Windows PowerShell execution policy, which blocked virtual environment activation scripts by default. This was resolved by setting the execution policy to `RemoteSigned` for the current user, allowing local scripts to run without compromising system security.

Connecting FastAPI to MongoDB asynchronously required using the Motor driver rather than the standard PyMongo library, since FastAPI is built on async Python. Configuring Motor with the correct async context and ensuring database operations did not block the event loop took careful structuring of the route handlers.

On the frontend, synchronising the dashboard summary statistics with live MongoDB data — rather than hardcoding values — required building a dedicated `/expenses/stats/summary` endpoint that computes monthly totals, category breakdowns, and transaction counts dynamically on each request.

Integrating Recharts for the bar and donut charts inside a responsive React layout required managing container sizing carefully, as Recharts needs explicit width and height constraints to render correctly inside CSS grid cells.

Finally, ensuring all CRUD operations immediately reflected in both the Expenses table and the Dashboard without a full page reload required lifting state to the root `App.jsx` component and re-fetching both the expense list and summary stats after every create, update, or delete action.

---

   Setup Instructions

  # Prerequisites
- Python 3.10+
- Node.js 18+
- MongoDB running locally on port 27017

Backend
bash
cd backend
python -m venv venv
venv\Scripts\activate        # Windows
pip install -r requirements.txt
python seed.py               # optional: load sample data
python -m uvicorn main:app --reload --port 8000
-----

Frontend
bash
cd frontend
npm install
npm run dev
----

Open http://localhost:5173 in your browser.

  API Endpoints
| Method | Endpoint                  | Description        |
|--------|---------------------------|--------------------|
| GET    | /expenses                 | List all expenses  |
| POST   | /expenses                 | Create expense     |
| GET    | /expenses/{id}            | Get one expense    |
| PUT    | /expenses/{id}            | Update expense     |
| DELETE | /expenses/{id}            | Delete expense     |
| GET    | /expenses/stats/summary   | Dashboard stats    |
