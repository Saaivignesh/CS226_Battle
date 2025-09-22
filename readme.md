 🚢 Battleship Database with PostgreSQL + Neon

This project demonstrates creating and populating a small **relational database** of naval ships and their classes using **PostgreSQL** hosted on **Neon**.  
It includes two tables (`Classes` and `Ships`), data insertion, and queries run via Python (`psycopg2`) + Pandas.

---

## 📂 Tables Created

### Classes
- `class` (TEXT, Primary Key)
- `type` (TEXT)
- `country` (TEXT)
- `numGuns` (INT)
- `bore` (INT)
- `displacement` (INT)

### Ships
- `name` (TEXT, Primary Key)
- `class` (TEXT, Foreign Key → Classes)
- `launched` (INT)

---

## ⚙️ Code: Creating Tables
```python
ddl = """
DROP TABLE IF EXISTS Ships CASCADE;
DROP TABLE IF EXISTS Classes CASCADE;

CREATE TABLE Classes (
  class        TEXT PRIMARY KEY,
  type         TEXT,
  country      TEXT,
  numGuns      INT,
  bore         INT,
  displacement INT
);

CREATE TABLE Ships (
  name     TEXT PRIMARY KEY,
  class    TEXT REFERENCES Classes(class),
  launched INT
);
"""
cur.execute(ddl)
conn.commit()
print("Tables created.")
📝 Code: Inserting Data
python
cur.execute("""
INSERT INTO Classes (class, type, country, numGuns, bore, displacement) VALUES
('Bismarck','bb','Germany',8,15,42000),
('Iowa','bb','USA',9,16,46000),
('Kongo','bc','Japan',8,14,32000),
('North Carolina','bb','USA',9,16,37000),
('Renown','bc','Gt. Britain',6,15,32000),
('Revenge','bb','Gt. Britain',8,15,29000),
('Tennessee','bb','USA',12,14,32000),
('Yamato','bb','Japan',9,18,65000);
""")

cur.execute("""
INSERT INTO Ships (name, class, launched) VALUES
('California','Tennessee',1921),
('Haruna','Kongo',1915),
('Hiei','Kongo',1914),
('Iowa','Iowa',1943),
('Kirishima','Kongo',1915),
('Kongo','Kongo',1913),
('Missouri','Iowa',1944),
('Musashi','Yamato',1942),
('New Jersey','Iowa',1943),
('North Carolina','North Carolina',1941),
('Ramillies','Revenge',1917),
('Renown','Renown',1916),
('Repulse','Renown',1916),
('Resolution','Revenge',1916),
('Revenge','Revenge',1916),
('Royal Oak','Revenge',1916),
('Royal Sovereign','Revenge',1916),
('
