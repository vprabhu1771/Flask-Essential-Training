You’re getting this error because **your Flask API is not sending CORS headers**, so your React (or any frontend) request is being blocked by the browser.

✅ **Fix = Install and enable Flask-CORS**

---

# ✅ **Step 1: Install Flask-CORS**

Run this:

```
pip install flask-cors
```

---

# ✅ **Step 2: Add CORS to Your Flask App**

Add this import at the top:

```python
from flask_cors import CORS
```

Then enable it:

```python
CORS(app)
```

Or if you want to allow all origins:

```python
CORS(app, resources={r"/*": {"origins": "*"}})
```

---

# ✅ Updated Working Code (Only CORS part added)

```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from flask_admin import Admin
from flask_admin.contrib.sqla import ModelView
from flask_cors import CORS
import secrets

app = Flask(__name__)

# Enable CORS for all routes
CORS(app)

# Secret key
app.config['SECRET_KEY'] = secrets.token_hex(32)

# Database
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'
db = SQLAlchemy(app)
```

That's it — CORS issue solved! 🎉

---

# 🚀 **Using React? Make sure axios/fetch URL is correct:**

Example:

```js
axios.get("http://192.168.1.175:5000/api/categories")
```

Or fetch:

```js
fetch("http://192.168.1.175:5000/api/categories")
```

---

# If still blocked?

Tell me:

✔ Your frontend framework (React? Vue?)
✔ Your server IP & port
✔ The exact fetch/axios code

I will fix it 100%.
