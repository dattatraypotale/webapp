# webapp
🛠️ Features
	• Responsive Design: Adapts seamlessly to various screen sizes, ensuring a consistent user experience across devices.
	• User-Friendly Interface: Intuitive layout with clear input fields and action buttons.
	• Toggle Between Forms: Users can easily switch between the login and registration forms without page reloads.
	• Modern Aesthetics: Clean design with a blue color scheme and subtle hover effects.

🚀 How to Implement
	1. Copy the Code: Select and copy the entire HTML and CSS code provided above.
	2. Create a New HTML File: Open your code editor and create a new file named index.html.
	3. Paste the Code: Paste the copied code into the index.html file.
	4. Save the File: Save the file and open it in your web browser to see the form in action.CodePen+2Adobe Stock+2CodePen+2

Sources
You said:
use SQLite database for the website also mention prerequisite while running the website

ChatGPT said:
Here's how you can integrate an SQLite database into your website for handling user registration and login. I'll include:
	1. 🔧 Prerequisites
	2. 🌐 HTML/CSS (already provided earlier)
	3. 🐍 Python Flask Backend with SQLite
	4. 🗃️ SQLite Schema (Users Table)

🔧 Prerequisites
To run the website with SQLite and Flask, you’ll need:
✅ Software Requirements:
	• Python 3.x
	• Pip (Python package manager)
✅ Install Dependencies:

bash
CopyEdit
pip install flask flask_sqlalchemy

🌐 HTML (Frontend)
Use the same HTML/CSS provided earlier — no changes needed for the frontend.

🐍 Flask Backend Code (Python + SQLite)
Create a file named app.py:

python
CopyEdit
from flask import Flask, render_template_string, request, redirect, url_for, flash
from flask_sqlalchemy import SQLAlchemy
from werkzeug.security import generate_password_hash, check_password_hash
app = Flask(__name__)
app.secret_key = 'your_secret_key'  # Replace with a secure key
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///users.db'
db = SQLAlchemy(app)
# SQLite User model
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(150))
    email = db.Column(db.String(150), unique=True, nullable=False)
    password = db.Column(db.String(256), nullable=False)
@app.route('/')
def index():
    return redirect(url_for('login'))
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        email = request.form['email']
        password = request.form['password']
        user = User.query.filter_by(email=email).first()
        if user and check_password_hash(user.password, password):
            flash('Login successful!', 'success')
        else:
            flash('Invalid email or password.', 'danger')
    return render_template_string(open("index.html").read())
@app.route('/register', methods=['POST'])
def register():
    name = request.form['name']
    email = request.form['email']
    password = request.form['password']
    confirm_password = request.form['confirm-password']
if password != confirm_password:
        flash('Passwords do not match!', 'danger')
        return redirect(url_for('login'))
existing_user = User.query.filter_by(email=email).first()
    if existing_user:
        flash('Email already registered!', 'danger')
        return redirect(url_for('login'))
hashed_password = generate_password_hash(password)
    new_user = User(name=name, email=email, password=hashed_password)
    db.session.add(new_user)
    db.session.commit()
    flash('Registration successful!', 'success')
    return redirect(url_for('login'))
if __name__ == '__main__':
    with app.app_context():
        db.create_all()  # Create tables if they don't exist
    app.run(debug=True)

🗃️ SQLite Database Setup
No separate SQL script is needed — the table is created automatically with:

python
CopyEdit
db.create_all()
This creates a users.db file locally with a User table when the Flask app starts.

🏁 Running the Website
	1. Ensure index.html (from earlier) is in the same directory as app.py.
	2. Run the app:

bash
CopyEdit
python app.py
	3. Open your browser and go to http://127.0.0.1:5000.

✅ Summary
Component	Description
HTML/CSS	Frontend UI with blue sections
Flask	Handles routing and logic
SQLite	Local DB for user credentials
SQLAlchemy	ORM for DB access
Hashing	Secure password storage



