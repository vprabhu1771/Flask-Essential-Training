```
https://flask-sqlalchemy.palletsprojects.com/en/3.1.x/
```

# Installation

Flask-SQLAlchemy is available on PyPI and can be installed with various Python tools. For example, to install or update the latest version using pip:

```
pip install -U Flask-SQLAlchemy
```

Here are **SQLAlchemy `create_engine` examples for ALL major databases**: **SQLite, MySQL, PostgreSQL, and SQL Server (MSSQL)** — with and without drivers.

---

# ✅ **1. SQLite**

### **In-memory database**

```python
from sqlalchemy import create_engine

engine = create_engine("sqlite:///:memory:", echo=True)
```

### **File-based SQLite**

```python
engine = create_engine("sqlite:///mydatabase.db", echo=True)
```

---

# ✅ **2. MySQL**

## **Using PyMySQL (recommended)**

```python
engine = create_engine("mysql+pymysql://username:password@localhost:3306/mydb")
```

## **Using MySQLClient**

```python
engine = create_engine("mysql+mysqldb://username:password@localhost/mydb")
```

---

# ✅ **3. PostgreSQL (psql)**

## **Using psycopg2 (most common)**

```python
engine = create_engine("postgresql+psycopg2://username:password@localhost:5432/mydb")
```

## **Using asyncpg**

```python
engine = create_engine("postgresql+asyncpg://username:password@localhost/mydb")
```

---

# ✅ **4. Microsoft SQL Server (MSSQL)**

## **Using pyodbc**

```python
engine = create_engine(
    "mssql+pyodbc://username:password@SERVERNAME/DATABASE?driver=ODBC+Driver+17+for+SQL+Server"
)
```

## **Using pymssql**

```python
engine = create_engine("mssql+pymssql://username:password@localhost:1433/mydb")
```

---

# 📌 **Driver Installation Summary**

| Database   | Driver                                 |
| ---------- | -------------------------------------- |
| SQLite     | built-in                               |
| MySQL      | `pip install pymysql` or `mysqlclient` |
| PostgreSQL | `pip install psycopg2`                 |
| SQL Server | `pip install pyodbc` or `pymssql`      |

---

# 📌 Example: Test Your Connection

```python
with engine.connect() as conn:
    result = conn.execute("SELECT 1")
    print(result.scalar())
```

---

If you need **ORM Base, session maker, or CRUD examples**, tell me — I’ll generate them!
