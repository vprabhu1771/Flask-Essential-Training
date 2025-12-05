`app.py`

```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from flask_admin import Admin
from flask_admin.contrib.sqla import ModelView
import secrets

app = Flask(__name__)

# Secret key
app.config['SECRET_KEY'] = secrets.token_hex(32)

# Database
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'
db = SQLAlchemy(app)


# Model
class Category(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(255), unique=True)

    def _init_(self, name):
        self.name = name


# Flask Admin
admin = Admin(app, name='MyAdmin')
admin.add_view(ModelView(Category, db.session))


# --------------------------
# 🚀 CATEGORY API ENDPOINTS
# --------------------------

# Get all categories
@app.get('/api/categories')
def get_categories():
    categories = Category.query.all()
    data = [{"id": c.id, "name": c.name} for c in categories]
    return jsonify({"status": True, "data": data})


# Get single category
@app.get('/api/categories/<int:id>')
def get_category(id):
    category = Category.query.get(id)
    if not category:
        return jsonify({"status": False, "message": "Category not found"}), 404
    return jsonify({"id": category.id, "name": category.name})


# Create category
@app.post('/api/categories')
def create_category():
    data = request.get_json()
    name = data.get("name")

    if not name:
        return jsonify({"status": False, "message": "Name is required"}), 400

    # Prevent duplicates
    if Category.query.filter_by(name=name).first():
        return jsonify({"status": False, "message": "Category already exists"}), 400

    new_cat = Category(name=name)
    db.session.add(new_cat)
    db.session.commit()
    return jsonify({"status": True, "message": "Category created"}), 201


# Update category
@app.put('/api/categories/<int:id>')
def update_category(id):
    category = Category.query.get(id)
    if not category:
        return jsonify({"status": False, "message": "Category not found"}), 404

    data = request.get_json()
    name = data.get("name")

    if name:
        category.name = name
    db.session.commit()

    return jsonify({"status": True, "message": "Category updated"})


# Delete category
@app.delete('/api/categories/<int:id>')
def delete_category(id):
    category = Category.query.get(id)
    if not category:
        return jsonify({"status": False, "message": "Category not found"}), 404

    db.session.delete(category)
    db.session.commit()

    return jsonify({"status": True, "message": "Category deleted"})


# Run App
if __name__ == '__main__':
    with app.app_context():
        db.create_all()
    app.run(debug=True)
```
