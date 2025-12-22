# Money Transfer System - Assignment 2

A peer-to-peer money transfer system with real-time transaction tracking and audit logging.

## 🎯 Features

- ✅ User Authentication (JWT-based)
- ✅ Real-time Money Transfers
- ✅ ACID-compliant Database Transactions
- ✅ Immutable Audit Log System
- ✅ Transaction History with Sorting
- ✅ Real-time Balance Updates

## 🛠️ Technology Stack

**Backend:**
- Python 3.x
- Flask (Web Framework)
- Flask-SQLAlchemy (ORM)
- Flask-JWT-Extended (Authentication)
- SQLite (Database)

**Frontend:**
- HTML5
- CSS3
- Vanilla JavaScript

## 📦 Setup Instructions

### Prerequisites
- Python 3.8 or higher
- pip (Python package manager)

### Step 1: Clone/Navigate to the Project

```bash
cd d:\lenden-club-assign
```

### Step 2: Set Up Backend

1. Navigate to the backend folder:
```bash
cd backend
```

2. Create a virtual environment:
```bash
python -m venv venv
```

3. Activate the virtual environment:
```bash
# Windows
venv\Scripts\activate
```

4. Install dependencies:
```bash
pip install -r requirements.txt
```

5. Run the backend server:
```bash
python app.py
```

The backend will start on `http://localhost:5000`

### Step 3: Set Up Frontend

1. Open a new terminal and navigate to the frontend folder:
```bash
cd d:\lenden-club-assign\frontend
```

2. Open `index.html` in your web browser, or use a simple HTTP server:
```bash
# Python 3
python -m http.server 8000
```

3. Access the application at `http://localhost:8000`

## 📚 API Documentation

### Authentication Endpoints

#### Register User
- **POST** `/register`
- **Body:**
```json
{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "securepassword"
}
```
- **Response (201):**
```json
{
  "message": "User registered successfully",
  "user_id": 1
}
```

#### Login
- **POST** `/login`
- **Body:**
```json
{
  "username": "john_doe",
  "password": "securepassword"
}
```
- **Response (200):**
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "user_id": 1,
  "username": "john_doe",
  "balance": 1000.0
}
```

### Transaction Endpoints

#### Get Balance
- **GET** `/balance`
- **Headers:** `Authorization: Bearer <token>`
- **Response (200):**
```json
{
  "balance": 1000.0
}
```

#### Transfer Money
- **POST** `/transfer`
- **Headers:** `Authorization: Bearer <token>`
- **Body:**
```json
{
  "receiver_username": "jane_doe",
  "amount": 100.50
}
```
- **Response (200):**
```json
{
  "message": "Transfer successful",
  "new_balance": 899.50,
  "transaction_id": 1
}
```

#### Get Transaction History
- **GET** `/transactions/<user_id>`
- **Headers:** `Authorization: Bearer <token>`
- **Response (200):**
```json
{
  "transactions": [
    {
      "id": 1,
      "type": "SENT",
      "other_party": "jane_doe",
      "amount": 100.50,
      "status": "SUCCESS",
      "timestamp": "2025-12-22T10:30:00",
      "description": "Transfer completed"
    }
  ]
}
```

#### Get All Users
- **GET** `/users`
- **Headers:** `Authorization: Bearer <token>`
- **Response (200):**
```json
{
  "users": [
    {"id": 2, "username": "jane_doe"},
    {"id": 3, "username": "bob_smith"}
  ]
}
```

## 🗄️ Database Schema

### Users Table
| Column | Type | Constraints |
|--------|------|-------------|
| id | Integer | Primary Key |
| username | String(80) | Unique, Not Null |
| email | String(120) | Unique, Not Null |
| password_hash | String(200) | Not Null |
| balance | Float | Default: 1000.0 |
| created_at | DateTime | Default: UTC Now |

### AuditLog Table
| Column | Type | Constraints |
|--------|------|-------------|
| id | Integer | Primary Key |
| sender_id | Integer | Foreign Key (User), Not Null |
| receiver_id | Integer | Foreign Key (User), Not Null |
| amount | Float | Not Null |
| status | String(20) | Not Null (SUCCESS/FAILED) |
| timestamp | DateTime | Default: UTC Now |
| description | String(200) | Optional |

## 🔒 ACID Compliance

The transfer endpoint implements ACID properties:
- **Atomicity:** Both debit and credit operations happen together or not at all
- **Consistency:** Balance constraints are maintained (no negative balances)
- **Isolation:** SQLite transactions prevent race conditions
- **Durability:** Committed transactions are permanently stored

Implementation:
```python
try:
    sender.balance -= amount
    receiver.balance += amount
    db.session.commit()  # Both succeed
except:
    db.session.rollback()  # Both fail
```

## 🤖 AI Tool Usage Log

### Tasks Where AI Was Used:

1. **Database Transaction Implementation**
   - Used AI to generate Flask-SQLAlchemy transaction wrapper with proper rollback handling
   - Generated error handling for edge cases (insufficient balance, receiver not found)

2. **JWT Authentication Middleware**
   - Generated Flask-JWT-Extended configuration and decorators
   - Created token validation and user identity extraction logic

3. **Sortable Transaction Table**
   - Generated JavaScript sorting logic for transaction history
   - Created dynamic table rendering with type-based styling

4. **API Error Handling**
   - Generated comprehensive error responses for all endpoints
   - Created consistent error message format across API

5. **Frontend Form Validation**
   - Generated client-side validation logic
   - Created real-time error display functionality

### Effectiveness Score: 4.5/5

**Justification:**
- Saved approximately 4-5 hours on boilerplate code generation
- Authentication setup was nearly perfect and required minimal modifications
- Database transaction logic was solid and handled edge cases well
- Frontend sorting algorithm was efficient and bug-free
- Only minor refinements needed for UI styling and error messages

## 🎥 Demo Instructions

To record your demo video, show:

1. **Registration:** Create 2-3 user accounts
2. **Login:** Log in with one account
3. **Transfer:** Send money to another user
4. **Balance Update:** Show balance decreasing in real-time
5. **Transaction History:** View the transaction list
6. **Sorting:** Demonstrate sorting by date, amount, and type
7. **Failed Transfer:** Try sending more than available balance
8. **Logout and Switch:** Log in as the receiver to see received funds

## 📝 Notes

- Default starting balance for new users: $1000
- All timestamps are in UTC
- Passwords are hashed using Werkzeug's security module
- JWT tokens expire after 24 hours
- The audit log is immutable (no DELETE or UPDATE operations)

## 🚀 Future Enhancements

- Add pagination for transaction history
- Implement transaction filtering by date range
- Add email notifications for received transfers
- Create admin dashboard for monitoring
- Add two-factor authentication

## 📄 License

This project is created for educational purposes as part of Assignment 2.
