Nice work on the Flask API! 🎯
Now you want a **React (Vite) CRUD frontend** using **class-based components** to connect with those Flask Category APIs. Below is a clean and working setup.

---

## 📌 Project Structure (React + Vite)

```
src/
 ├── api/
 │    └── categoryApi.js   → API calls
 ├── components/
 │    ├── CategoryList.jsx → List + Delete
 │    ├── CategoryForm.jsx → Create + Update
 │    └── CategoryDetail.jsx (optional)
 ├── App.jsx
 └── main.jsx
```

---

## 🔌 Install & Create Vite App

```bash
npm create vite@latest category-crud -- --template react
cd category-crud
npm install
```

> Convert `.jsx` files inside `src`.

---

## 🌍 API Helper File — `src/api/categoryApi.js`

```javascript
const API_URL = "http://localhost:5000/api/categories";

export const getCategories = () => fetch(API_URL).then(res => res.json());
export const getCategory = (id) => fetch(`${API_URL}/${id}`).then(res => res.json());

export const createCategory = (data) =>
    fetch(API_URL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
    }).then(res => res.json());

export const updateCategory = (id, data) =>
    fetch(`${API_URL}/${id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
    }).then(res => res.json());

export const deleteCategory = (id) =>
    fetch(`${API_URL}/${id}`, { method: "DELETE" }).then(res => res.json());
```

---

## 📋 Category List Component — `CategoryList.jsx` (Class Component)

```jsx
import React, { Component } from "react";
import { getCategories, deleteCategory } from "../api/categoryApi";
import CategoryForm from "./CategoryForm";

class CategoryList extends Component {
    state = { categories: [], selected: null };

    componentDidMount() {
        this.load();
    }

    load() {
        getCategories().then(res => {
            this.setState({ categories: res.data });
        });
    }

    handleEdit = (category) => {
        this.setState({ selected: category });
    };

    handleDelete = (id) => {
        deleteCategory(id).then(() => this.load());
    };

    handleSaved = () => {
        this.setState({ selected: null });
        this.load();
    };

    render() {
        return (
            <div style={{width:"500px", margin:"auto"}}>
                <h2>📌 Categories</h2>
                <CategoryForm
                    selected={this.state.selected}
                    onSaved={this.handleSaved}
                />

                <ul>
                    {this.state.categories.map(cat => (
                        <li key={cat.id}>
                            {cat.name}
                            <button onClick={() => this.handleEdit(cat)}>✏ Edit</button>
                            <button onClick={() => this.handleDelete(cat.id)}>🗑 Delete</button>
                        </li>
                    ))}
                </ul>
            </div>
        );
    }
}

export default CategoryList;
```

---

## 📝 Create + Update Form — `CategoryForm.jsx`

```jsx
import React, { Component } from "react";
import { createCategory, updateCategory } from "../api/categoryApi";

class CategoryForm extends Component {
    state = { id: null, name: "" };

    componentDidUpdate(prevProps) {
        if (prevProps.selected !== this.props.selected && this.props.selected) {
            this.setState({
                id: this.props.selected.id,
                name: this.props.selected.name
            });
        }
    }

    handleSubmit = (e) => {
        e.preventDefault();
        const data = { name: this.state.name };

        if (this.state.id) {
            updateCategory(this.state.id, data).then(() => {
                this.setState({ id: null, name: "" });
                this.props.onSaved();
            });
        } else {
            createCategory(data).then(() => {
                this.setState({ name: "" });
                this.props.onSaved();
            });
        }
    };

    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <input
                    type="text"
                    placeholder="Category Name"
                    value={this.state.name}
                    onChange={(e) => this.setState({ name: e.target.value })}
                />
                <button type="submit">
                    {this.state.id ? "Update Category" : "Add Category"}
                </button>
            </form>
        );
    }
}

export default CategoryForm;
```

---

## 🚀 App.jsx

```jsx
import React from "react";
import CategoryList from "./components/CategoryList";

function App() {
    return (
        <div>
            <h1>Flask + React CRUD</h1>
            <CategoryList />
        </div>
    );
}

export default App;
```

---

## ▶ Run the App

❗ Start Flask first

```bash
python app.py
```

Start React:

```bash
npm run dev
```

Open → [http://localhost:5173](http://localhost:5173)

---

## 🔥 Notes & Fixes for your Flask Code

You have small typos:

| Wrong    | Correct    |
| -------- | ---------- |
| `_name_` | `__name__` |
| `_init_` | `__init__` |
| `_main_` | `__main__` |

✔ Updated version:

```python
app = Flask(__name__)

def __init__(self, name):
    self.name = name

if __name__ == '__main__':
    with app.app_context():
        db.create_all()
    app.run(debug=True, port=5000)
```

Also add **CORS** for React:

```bash
pip install flask-cors
```

```python
from flask_cors import CORS
CORS(app)
```

---

## 🎯 Result

✔ Fully Working CRUD
✔ Class-based React Components
✔ Flask API Integration

---

If you want, I can also provide:
⬜ Bootstrap UI version
⬜ Routing for “Category Detail” view
⬜ Toast notifications after CRUD
⬜ Pagination & Search

Would you like me to add **Bootstrap design** and **better UI** next?
