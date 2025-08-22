[app.py](https://github.com/user-attachments/files/21941517/app.py)
[db.py](https://github.com/user-attachments/files/21941521/db.py)
/* Global styles */
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    margin: 0;
    padding: 0;
    background: #f8f9fa;
    color: #333;
}

/* Navbar */
.navbar {
    background: #dc3545;
    padding: 15px 25px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: white;
}

.navbar h1 {
    margin: 0;
    font-size: 22px;
    font-weight: bold;
}

.navbar a {
    color: white;
    margin-left: 20px;
    text-decoration: none;
    font-weight: 500;
    transition: 0.3s;
}

.navbar a:hover {
    text-decoration: underline;
}

/* Dashboard container */
.container {
    width: 90%;
    margin: 25px auto;
    padding: 20px;
    background: white;
    border-radius: 12px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

/* Dashboard header */
.dashboard-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 25px;
}

.dashboard-header h2 {
    margin: 0;
    font-size: 24px;
    color: #dc3545;
}

.btn {
    padding: 8px 14px;
    background: #dc3545;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 500;
    transition: 0.3s;
}

.btn:hover {
    background: #b02a37;
}

/* Cards for stats */
.stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 20px;
    margin-bottom: 30px;
}

.card {
    padding: 20px;
    border-radius: 12px;
    text-align: center;
    background: #f1f1f1;
    transition: transform 0.2s;
}

.card h3 {
    margin: 0;
    font-size: 18px;
    color: #444;
}

.card p {
    font-size: 28px;
    font-weight: bold;
    margin-top: 10px;
    color: #dc3545;
}

.card:hover {
    transform: translateY(-5px);
    background: #ffeaea;
}

/* Chart section */
.chart-container {
    margin: 20px auto;
    padding: 20px;
    border-radius: 12px;
    background: #fff5f5;
    box-shadow: 0 4px 8px rgba(220,53,69,0.2);
}

/* Donor table */
table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

table thead {
    background: #dc3545;
    color: white;
}

table th, table td {
    padding: 12px;
    text-align: center;
    border: 1px solid #ddd;
}

table tr:nth-child(even) {
    background: #f9f9f9;
}

table tr:hover {
    background: #ffeaea;
}
[styles.css](https://github.com/user-attachments/files/21941539/styles.css)
import sqlite3

[add_donor.html](https://github.com/user-attachments/files/21941530/add_donor.html)
[base.html](https://github.com/user-attachments/files/21941533/base.html)[index.html](https://github.com/user-attachments/files/21941536/index.html)
[donors.html](https://github.com/user-attachments/files/21941535/donors.html)
[dashboard.html](https://github.com/user-attachments/files/21941534/dashboard.html)
[thresholds.html](https://github.com/user-attachments/files/21941532/thresholds.html)
DB_NAME = "blood_donation.db"

# ------------------ DB Connection Helper ------------------
def get_db():
    conn = sqlite3.connect(DB_NAME)
 [requirements.txt](https://github.com/user-attachments/files/21941526/requirements.txt)
   cDROP TABLE IF EXISTS donors;
D# seed.py
import sqlite3

def seed_data():
    conn = sqlite3.connect("blood_donation.db")
    cursor = conn.cursor()

    # Sample donor data
    donors = [
        ("Amit Kumar", "A+", "Delhi", "9876543210"),
        ("Priya Sharma", "B+", "Patna", "9123456780"),
        ("Rahul Singh", "O+", "Mumbai", "9988776655"),
        ("Sneha Gupta", "AB-", "Kolkata", "9765432109"),
        ("Aditya Verma", "O-", "Ranchi", "9012345678"),
        ("Pooja Yadav", "A-", "Lucknow", "8899776655"),
        ("Rohit Mehta", "B-", "Chennai", "9321654780"),
        ("Neha Rani", "AB+", "Hyderabad", "9876123450")
    ]

    cursor.executemany(
        "INSERT INTO donors (name, blood_group, location, contact) VALUES (?, ?, ?, ?)", 
        donors
    )

    conn.commit()
    conn.close()
    print("✅ Sample donor data inserted successfully!")

if __name__ == "__main__":
    seed_data()
[seed.py](https://github.com/user-attachments/files/21941529/seed.py)
ROP TABLE IF EXISTS blood_stock;
DROP TABLE IF EXISTS thresholds;

-- Donors table
CREATE TABLE donors (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    blood_group TEXT NOT NULL,
    age INTEGER,
    contact TEXT
);

-- Blood stock table
CREATE TABLE blood_stock (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    blood_group TEXT NOT NULL,
    units INTEGER NOT NULL
);

-- Thresholds table
CREATE TABLE thresholds (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    blood_group TEXT NOT NULL,
    min_units INTEGER NOT NULL
);

-- Insert initial stock data (all blood groups, 0 units by default)
INSERT INTO stock (blood_group, units) VALUES
('A+', 0),
('A-', 0),
('B+', 0),
('B-', 0),
('AB+', 0),
('AB-', 0),
('O+', 0),
('O-', 0);

-- Insert default thresholds (can be updated from dashboard)
INSERT INTO thresholds (blood_group, min_units) VALUES
('A+', 5),
('A-', 5),
('B+', 5),
('B-', 5),
('AB+', 5),
('AB-', 5),
('O+', 5),
('O-', 5);
[schema.sql](https://github.com/user-attachments/files/21941527/schema.sql)
import sqlite3

def init_db():
    with sqlite3.connect("blood.db") as conn:
        with open("schema.sql", "r") as f:
            conn.executescript(f.read())
    print("✅ Database initialized with schema.sql")

if __name__ == "__main__":
    init_db()
[init_db.py](https://github.com/user-attachments/files/21941524/init_db.py)
onn.row_factory = sqlite3.Row  # rows as dict-like objects
    return conn

# ------------------ Initialize DB ------------------
def init_db():
    conn = get_db()
    cur = conn.cursor()

    # Donors table
    cur.execute("""
        CREATE TABLE IF NOT EXISTS donors (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            blood_group TEXT NOT NULL,
            last_donation_date TEXT,
            total_units INTEGER DEFAULT 0,
            location TEXT
        )
    """)

    # Donations table
    cur.execute("""
        CREATE TABLE IF NOT EXISTS donations (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            donor_id INTEGER,
            donation_date TEXT,
            units INTEGER,
            FOREIGN KEY (donor_id) REFERENCES donors (id)
        )
    """)

    # Thresholds table
    cur.execute("""
        CREATE TABLE IF NOT EXISTS thresholds (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            blood_group TEXT UNIQUE,
            min_units INTEGER
        )
    """)

    conn.commit()
    conn.close()

# ------------------ Donors ------------------
def get_donors():
    conn = get_db()
    cur = conn.cursor()
    cur.execute("SELECT * FROM donors ORDER BY name")
    rows = cur.fetchall()
    conn.close()
    return rows

def add_or_update_donor_with_donation(name, group, units, date, location):
    conn = get_db()
    cur = conn.cursor()

    # check if donor already exists
    cur.execute("SELECT id, total_units FROM donors WHERE name = ? AND blood_group = ?", (name, group))
    donor = cur.fetchone()

    if donor:
        # update existing donor
        donor_id = donor["id"]
        total = (donor["total_units"] or 0) + units
        cur.execute(
            "UPDATE donors SET total_units=?, last_donation_date=?, location=? WHERE id=?",
            (total, date, location, donor_id)
        )
    else:
        # create new donor
        cur.execute(
            "INSERT INTO donors (name, blood_group, last_donation_date, total_units, location) VALUES (?,?,?,?,?)",
            (name, group, date, units, location)
        )
        donor_id = cur.lastrowid

    # add donation record
    cur.execute(
        "INSERT INTO donations (donor_id, donation_date, units) VALUES (?,?,?)",
        (donor_id, date, units)
    )

    conn.commit()
    conn.close()

# ------------------ Stock Summary ------------------
def get_stock_summary():
    conn = get_db()
    cur = conn.cursor()
    cur.execute("""
        SELECT d.blood_group, 
               SUM(d.total_units) as units,
               COALESCE(t.min_units, 0) as min_units
        FROM donors d
        LEFT JOIN thresholds t ON d.blood_group = t.blood_group
        GROUP BY d.blood_group
    """)
    rows = cur.fetchall()
    conn.close()
    return rows

# ------------------ Thresholds ------------------
def get_thresholds():
    conn = get_db()
    cur = conn.cursor()
    cur.execute("SELECT * FROM thresholds ORDER BY blood_group")
    rows = cur.fetchall()
    conn.close()
    return rows

def set_threshold(group, min_units):
    conn = get_db()
    cur = conn.cursor()
    cur.execute("INSERT OR REPLACE INTO thresholds (blood_group, min_units) VALUES (?,?)", (group, min_units))
    conn.commit()
    conn.close()
