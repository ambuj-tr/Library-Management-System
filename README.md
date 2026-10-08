# Library-Management-System

# Library Management System

A full-stack web application for managing a library: add, search, edit and delete books, manage members, issue and return books, and see live statistics.

**Stack (only these):** HTML5, CSS3, vanilla JavaScript (Fetch API) | Python Flask | SQLite | REST + JSON

---

## 1. Quick start on Windows (VS Code)

Open the `library-management-system` folder in VS Code, then open a terminal (**Terminal > New Terminal**).

**PowerShell**

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
python database.py
python app.py
```

**Command Prompt (cmd)**

```bat
python -m venv venv
venv\Scripts\activate.bat
pip install -r requirements.txt
python database.py
python app.py
```

Then open **http://127.0.0.1:5000** in your browser.

What each command does:

| Command | Purpose |
|---|---|
| `python -m venv venv` | Creates an isolated Python environment (once) |
| `.\venv\Scripts\Activate.ps1` | Switches the terminal to that environment (every new terminal) |
| `pip install -r requirements.txt` | Installs Flask |
| `python database.py` | Creates `library.db` with all tables and demo data (optional: `python app.py` also does this automatically) |
| `python app.py` | Starts the server. Press `Ctrl + C` to stop it |

To wipe all data and go back to the demo data: `python database.py --reset`

> **Important:** always open the app through Flask (`http://127.0.0.1:5000`). Do not double-click `index.html`. Flask serves the page and the API from the same address, which is what lets the Fetch API calls work.

## 2. Running the automated tests

```powershell
python test_api.py
```

The tests use a temporary database, so your real `library.db` is never touched. Expected result: `Ran 21 tests ... OK`.

## 3. Project structure

```
library-management-system/
├── app.py            Flask app: all REST endpoints and business rules
├── database.py       SQLite connection, schema creation, safe transactions, demo data
├── validators.py     Server-side input validation
├── schema.sql        CREATE TABLE statements (books, members, transactions)
├── requirements.txt  Python dependencies (Flask)
├── test_api.py       Automated API tests
├── library.db        SQLite database (created automatically; demo data included)
├── README.md         This file
├── docs/
│   └── SUBMISSION.md Project explanation, architecture, test cases, viva Q&A
└── frontend/
    ├── index.html    Page layout, forms and dialogs
    ├── style.css     Navy / white / teal theme, responsive layout
    └── script.js     Fetch API calls, rendering, validation
```

## 4. Features

- **Dashboard:** total books, available copies, issued copies, total members, overdue warning, shelf status bar, recently added books and recent transactions. All numbers come from the backend.
- **Books:** add, edit, delete (with confirmation), live search by title, author, ISBN or category, filters for category and availability, availability badges. Duplicate ISBNs and invalid copy counts are rejected.
- **Members:** add, edit, delete, search by name, email or member ID. Duplicate emails and invalid email or phone formats are rejected.
- **Issue / Return:** pick a member and an available book, set issue and due dates, see a confirmation. Return books from the "Currently issued" list (with a confirmation dialog).
- **Transactions:** full history with filters for all, active issues and returned.
- **Everywhere:** loading indicators, empty states, success toasts, readable error messages, and a layout that works on laptop, tablet and phone.

## 5. Business rules enforced by the backend

- A book cannot be issued when no copies are available.
- The member and book must exist; the due date cannot be earlier than the issue date.
- Issuing decreases available copies by one and records the transaction, in one database transaction (both happen or neither).
- Returning increases available copies by one only the first time. A second return is rejected with `409`.
- A book cannot be deleted while any copy is issued. A member cannot be deleted while holding a book.
- **Design decision:** a book or member that has *any* past transaction also cannot be deleted. This keeps transaction history intact (the foreign keys would block it anyway). Books and members with no history can be deleted freely.
- Total copies cannot be set below the number of copies currently issued.
- Stock can never go negative (checked in code and by a `CHECK` constraint in SQLite).

## 6. REST API

All responses are JSON. Success: `{"success": true, "message": "...", "data": ...}`. Failure: `{"success": false, "error": "...", "errors": {"field": "..."}}`.

### Books

| Method | Endpoint | Description | Success | Errors |
|---|---|---|---|---|
| GET | `/api/books` | List all books (optional `q`, `category`, `availability` filters) | 200 | 400 |
| GET | `/api/books/search?q=keyword` | Search by title, author, ISBN or category (also accepts `category` and `availability=available/unavailable`) | 200 | 400 |
| POST | `/api/books` | Add a book | 201 | 400, 409 duplicate ISBN |
| PUT | `/api/books/<id>` | Update a book | 200 | 400, 404, 409 |
| DELETE | `/api/books/<id>` | Delete a book | 200 | 404, 409 issued or has history |
| GET | `/api/categories` | List distinct categories (feeds the filter dropdown) | 200 | |

### Members

| Method | Endpoint | Description | Success | Errors |
|---|---|---|---|---|
| GET | `/api/members` | List members (optional `q` for name, email or ID) | 200 | |
| POST | `/api/members` | Add a member | 201 | 400, 409 duplicate email |
| PUT | `/api/members/<id>` | Update a member | 200 | 400, 404, 409 |
| DELETE | `/api/members/<id>` | Delete a member | 200 | 404, 409 holds books or has history |

### Issue, return and history

| Method | Endpoint | Description | Success | Errors |
|---|---|---|---|---|
| POST | `/api/transactions/issue` | Issue a book | 201 | 400, 404, 409 no copies |
| POST | `/api/transactions/return` | Return a book | 200 | 400, 404, 409 already returned |
| GET | `/api/transactions` | History (optional `status=issued/returned`, `q`) | 200 | 400 |
| GET | `/api/transactions/active` | Currently issued books (optional `q`) | 200 | |

### Dashboard

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/dashboard/stats` | Totals, overdue count, recent books and recent transactions |

### Example requests

```json
POST /api/books
{ "title": "Clean Code", "author": "Robert C. Martin", "isbn": "9780132350884",
  "category": "Programming", "total_copies": 3 }

POST /api/transactions/issue
{ "member_id": 1, "book_id": 2, "issue_date": "2026-10-03", "due_date": "2026-10-17" }

POST /api/transactions/return
{ "transaction_id": 5 }
```

Status codes used: `200` OK, `201` Created, `400` invalid input, `404` record not found, `405` wrong HTTP method, `409` conflict (duplicate, no stock, already returned, blocked delete), `500` unexpected server or database error.

## 7. Troubleshooting

| Problem | Fix |
|---|---|
| `'python' is not recognized` | Install Python from python.org and tick **Add Python to PATH**. Or try `py` instead of `python`. Restart VS Code afterwards. |
| PowerShell says *running scripts is disabled* when activating the venv | Run `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`, then activate again. Or use Command Prompt instead. |
| `ModuleNotFoundError: No module named 'flask'` | The virtual environment is not active. Activate it (you should see `(venv)` in the prompt), then run `pip install -r requirements.txt`. |
| `Address already in use` / port 5000 busy | Use another port. PowerShell: `$env:PORT=5001; python app.py`. cmd: `set PORT=5001` then `python app.py`. Then open `http://127.0.0.1:5001`. To find what uses 5000: `netstat -ano \| findstr :5000`. |
| Page shows "Cannot reach the server" | Flask is not running or you opened the file directly. Start `python app.py` and use the `http://127.0.0.1:5000` address. |
| `sqlite3.OperationalError: database is locked` | Close any SQLite viewer that has `library.db` open, then retry. |
| Want to start over with clean demo data | Stop the server, run `python database.py --reset`, start the server again. |
| Fonts look different | The page loads two Google Fonts. Offline, it falls back to Georgia and Segoe UI. Everything still works. |
| Windows Firewall pop-up | Choose **Allow**; the server only listens on `127.0.0.1` (your own computer). |

## 8. Notes

- The demo data (16 books, 7 members, 8 transactions) is inserted only when the database is empty. One issued book is deliberately overdue so the warning appears.
- Sample ISBNs and people are for demonstration. ISBNs are checked for format (10 or 13 digits), not for the check digit.
- Flask runs in debug mode for development (auto-reload on code changes). Do not deploy it like this.
