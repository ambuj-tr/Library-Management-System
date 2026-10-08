# 📚 Library Management System

### MDM TA3

**Name:** Ambuj Tripathi
**Roll No:** 05
**Section:** B
**Department:** ENCS

---

## 📌 Project Overview

The **Library Management System** is a full-stack web application designed to simplify and manage day-to-day library operations.

The system allows librarians to:

* 📖 Add, search, edit and delete books
* 👨‍🎓 Manage library members
* 📤 Issue books
* 📥 Return books
* 📊 View live library statistics
* 🔍 Search and filter books and members
* 📋 Track complete transaction history
* ⚠️ Monitor overdue books

The application follows a **REST API-based architecture** with a Flask backend, SQLite database and a responsive vanilla JavaScript frontend.

---

## 🛠️ Technology Stack

| Technology       | Purpose                       |
| ---------------- | ----------------------------- |
| **HTML5**        | Frontend structure            |
| **CSS3**         | Styling and responsive design |
| **JavaScript**   | Frontend logic and Fetch API  |
| **Python Flask** | Backend and REST API          |
| **SQLite**       | Database                      |
| **REST + JSON**  | Client-server communication   |

> **Note:** Only HTML5, CSS3, Vanilla JavaScript, Python Flask and SQLite are used in this project.

---

## ✨ Features

### 📊 Dashboard

* Total number of books
* Available copies
* Currently issued copies
* Total members
* Overdue book warnings
* Shelf status
* Recently added books
* Recent transactions

### 📚 Book Management

* Add new books
* Edit existing books
* Delete books
* Search books by:

  * Title
  * Author
  * ISBN
  * Category
* Filter by category
* Filter by availability
* Availability badges
* Duplicate ISBN validation

### 👥 Member Management

* Add members
* Edit members
* Delete members
* Search by:

  * Name
  * Email
  * Member ID
* Duplicate email validation
* Email and phone number validation

### 🔄 Issue & Return

* Select a member
* Select an available book
* Set issue date
* Set due date
* Issue books
* Return books
* View currently issued books
* Confirmation dialogs for important operations

### 📋 Transaction Management

* Complete transaction history
* Filter transactions by:

  * All
  * Active Issues
  * Returned
* Search transaction records
* Track issue and return information

### 📱 User Experience

* Loading indicators
* Empty states
* Success notifications
* Error messages
* Confirmation dialogs
* Responsive layout for:

  * Desktop
  * Tablet
  * Mobile

---

# 🚀 Quick Start

## 1. Clone the Repository

```bash
git clone https://github.com/your-username/library-management-system.git
cd library-management-system
```

Open the project folder in **VS Code**.

---

## 2. Create a Virtual Environment

### PowerShell

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

### Command Prompt

```bat
python -m venv venv
venv\Scripts\activate.bat
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 4. Initialize the Database

```bash
python database.py
```

This creates the SQLite database and inserts the demo data.

---

## 5. Start the Flask Server

```bash
python app.py
```

The application will be available at:

**http://127.0.0.1:5000**

> ⚠️ Always open the application through Flask. Do not double-click `index.html`, because the frontend uses the Flask REST API through the same server.

---

# 🗂️ Project Structure

```text
library-management-system/
│
├── app.py
│   └── Flask application and REST API endpoints
│
├── database.py
│   └── SQLite connection, schema creation and demo data
│
├── validators.py
│   └── Server-side input validation
│
├── schema.sql
│   └── Database table definitions
│
├── requirements.txt
│   └── Python dependencies
│
├── test_api.py
│   └── Automated API tests
│
├── library.db
│   └── SQLite database
│
├── README.md
│   └── Project documentation
│
├── docs/
│   └── SUBMISSION.md
│       └── Project explanation, architecture,
│           test cases and viva questions
│
└── frontend/
    ├── index.html
    │   └── Page structure, forms and dialogs
    │
    ├── style.css
    │   └── Application styling and responsive design
    │
    └── script.js
        └── Fetch API calls, validation and UI rendering
```

---

# 🏗️ System Architecture

```text
┌─────────────────────────────┐
│        User / Browser       │
│       HTML + CSS + JS       │
└──────────────┬──────────────┘
               │
               │ Fetch API
               │ REST + JSON
               ▼
┌─────────────────────────────┐
│       Python Flask          │
│       REST API Layer        │
│                             │
│  Books | Members | Issues   │
│  Returns | Transactions     │
│  Dashboard | Validation     │
└──────────────┬──────────────┘
               │
               │ SQL Queries
               ▼
┌─────────────────────────────┐
│          SQLite             │
│                             │
│  Books                      │
│  Members                    │
│  Transactions               │
└─────────────────────────────┘
```

---

# 🔌 REST API

All API responses are returned in **JSON format**.

### Success Response

```json
{
  "success": true,
  "message": "Operation successful",
  "data": {}
}
```

### Error Response

```json
{
  "success": false,
  "error": "Invalid input",
  "errors": {}
}
```

---

## 📚 Books API

| Method | Endpoint                      | Description         |
| ------ | ----------------------------- | ------------------- |
| GET    | `/api/books`                  | Get all books       |
| GET    | `/api/books/search?q=keyword` | Search books        |
| POST   | `/api/books`                  | Add a book          |
| PUT    | `/api/books/<id>`             | Update a book       |
| DELETE | `/api/books/<id>`             | Delete a book       |
| GET    | `/api/categories`             | Get book categories |

### Supported Filters

```text
q
category
availability
```

---

## 👥 Members API

| Method | Endpoint            | Description     |
| ------ | ------------------- | --------------- |
| GET    | `/api/members`      | Get members     |
| POST   | `/api/members`      | Add a member    |
| PUT    | `/api/members/<id>` | Update a member |
| DELETE | `/api/members/<id>` | Delete a member |

---

## 🔄 Transaction API

| Method | Endpoint                   | Description             |
| ------ | -------------------------- | ----------------------- |
| POST   | `/api/transactions/issue`  | Issue a book            |
| POST   | `/api/transactions/return` | Return a book           |
| GET    | `/api/transactions`        | Get transaction history |
| GET    | `/api/transactions/active` | Get active issues       |

---

## 📊 Dashboard API

| Method | Endpoint               | Description              |
| ------ | ---------------------- | ------------------------ |
| GET    | `/api/dashboard/stats` | Get dashboard statistics |

---

# 📝 Example API Requests

### Add a Book

```http
POST /api/books
```

```json
{
  "title": "Clean Code",
  "author": "Robert C. Martin",
  "isbn": "9780132350884",
  "category": "Programming",
  "total_copies": 3
}
```

### Issue a Book

```http
POST /api/transactions/issue
```

```json
{
  "member_id": 1,
  "book_id": 2,
  "issue_date": "2026-10-03",
  "due_date": "2026-10-17"
}
```

### Return a Book

```http
POST /api/transactions/return
```

```json
{
  "transaction_id": 5
}
```

---

# ⚙️ Business Rules

The backend enforces the following rules:

1. A book cannot be issued if no copies are available.
2. The selected member must exist.
3. The selected book must exist.
4. The due date cannot be earlier than the issue date.
5. Issuing a book decreases available copies by one.
6. Issuing and transaction creation occur inside one database transaction.
7. Returning a book increases available copies by one.
8. A book cannot be returned twice.
9. A book cannot be deleted while a copy is issued.
10. A member cannot be deleted while holding a book.
11. Books and members with transaction history cannot be deleted.
12. Total copies cannot be less than currently issued copies.
13. Stock can never become negative.
14. Duplicate ISBNs are rejected.
15. Duplicate member emails are rejected.
16. Invalid email, phone and book information is rejected by server-side validation.

---

# 🧪 Testing

The project includes automated API tests.

Run:

```bash
python test_api.py
```

Expected output:

```text
Ran 21 tests ... OK
```

The tests use a **temporary database**, so the actual `library.db` file is not modified.

---

# 🗄️ Database

The application uses **SQLite** as its database.

Main tables:

```text
┌──────────────────┐
│      books       │
├──────────────────┤
│ id               │
│ title            │
│ author           │
│ isbn             │
│ category         │
│ total_copies     │
│ available_copies │
└──────────────────┘

┌──────────────────┐
│     members      │
├──────────────────┤
│ id               │
│ name             │
│ email            │
│ phone            │
└──────────────────┘

┌──────────────────┐
│   transactions   │
├──────────────────┤
│ id               │
│ member_id        │
│ book_id          │
│ issue_date       │
│ due_date         │
│ return_date      │
│ status           │
└──────────────────┘
```

---

# 🔐 Data Integrity

The project uses both **application-level validation** and **database constraints** to maintain data integrity.

Examples:

* Unique ISBN
* Unique member email
* Valid copy counts
* Foreign key relationships
* Stock constraints
* Transaction consistency
* Safe database transactions

---

# 🧹 Reset Demo Database

To remove existing data and restore the original demo data:

```bash
python database.py --reset
```

Then restart the Flask application:

```bash
python app.py
```

---

# 🛠️ Troubleshooting

### Python is not recognized

Install Python and enable **Add Python to PATH** during installation.

You can also try:

```bash
py
```

instead of:

```bash
python
```

---

### PowerShell blocks virtual environment activation

Run:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

Then:

```powershell
.\venv\Scripts\Activate.ps1
```

---

### Flask is not installed

Make sure the virtual environment is active:

```bash
pip install -r requirements.txt
```

You should see:

```text
(venv)
```

in the terminal.

---

### Port 5000 is already in use

Use another port.

PowerShell:

```powershell
$env:PORT=5001
python app.py
```

Command Prompt:

```bat
set PORT=5001
python app.py
```

Then open:

```text
http://127.0.0.1:5001
```

---

### Cannot reach the server

Make sure Flask is running:

```bash
python app.py
```

Then access the application using:

```text
http://127.0.0.1:5000
```

Do not open `index.html` directly.

---

# 📌 Demo Data

The project includes demonstration data:

* **16 books**
* **7 members**
* **8 transactions**

One book is intentionally configured as **overdue** so that the dashboard warning can be demonstrated.

---

# 📈 HTTP Status Codes

| Status Code | Meaning                        |
| ----------- | ------------------------------ |
| `200`       | Request successful             |
| `201`       | Resource created               |
| `400`       | Invalid input                  |
| `404`       | Resource not found             |
| `405`       | HTTP method not allowed        |
| `409`       | Conflict                       |
| `500`       | Internal server/database error |

---

# 🎓 Academic Information

**Project:** Library Management System
**Assessment:** MDM TA3

**Student:** Ambuj Tripathi
**Roll No:** 05
**Section:** B
**Department:** ENCS

**University:** Ramdeobaba University, Nagpur

---

# 👨‍💻 Author

**Ambuj Tripathi**

B.Tech — Electronics and Computer Science (ENCS)
Ramdeobaba University, Nagpur

---

## ⭐ Project Highlights

* Full-stack web application
* REST API architecture
* CRUD operations
* SQLite database
* Flask backend
* Vanilla JavaScript frontend
* Server-side validation
* Database transactions
* Responsive UI
* Automated API testing
* Real-time dashboard statistics


