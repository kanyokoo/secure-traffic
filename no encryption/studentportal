from flask import Flask, render_template, request, redirect, session
import sqlite3

app = Flask(__name__)
app.secret_key = 'curtain' 

def init_db():
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS students (
                        id INTEGER PRIMARY KEY AUTOINCREMENT,
                        username TEXT NOT NULL,
                        password TEXT NOT NULL,
                        email TEXT NOT NULL)''')
    conn.commit()
    conn.close()

@app.route('/')
def home():
    return render_template('login.html')

@app.route('/login', methods=['POST'])
def login():
    username = request.form['username']
    password = request.form['password']
    
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM students WHERE username=? AND password=?", (username, password))
    user = cursor.fetchone()
    conn.close()
    
    if user:
        session['user'] = username
        return redirect('/dashboard')
    else:
        return "Invalid Credentials! Try Again."

@app.route('/dashboard')
def dashboard():
    if 'user' in session:
        return render_template('dashboard.html', user=session['user'])
    else:
        return redirect('/')

@app.route('/logout')
def logout():
    session.pop('user', None)
    return redirect('/')

@app.route('/register', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        email = request.form['email']
        
        conn = sqlite3.connect('database.db')
        cursor = conn.cursor()
        cursor.execute("INSERT INTO students (username, password, email) VALUES (?, ?, ?)", (username, password, email))
        conn.commit()
        conn.close()
        
        return redirect('/')
    return render_template('register.html')

if __name__ == '__main__':
    init_db()
    app.run(debug=True, port=5500)
