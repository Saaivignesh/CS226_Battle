markdown
# 🏏 ODI Cricket Stats with PostgreSQL + Neon + Python  

This project builds a **cricket ODI stats database** using **PostgreSQL (on Neon)** and queries it with **Python + psycopg2 + Pandas**.  
We create a table, insert data for legendary ODI batsmen, then run queries to analyze their performance.  

---

## 📂 Project Structure
- `notebook.ipynb` → Jupyter Notebook with step‑by‑step cells  
- `README.md` → Documentation  

---

## ⚙️ Requirements  
Install dependencies:
```bash
pip install psycopg2 pandas
Sign up for a free Neon PostgreSQL database 👉 https://console.neon.tech

🔑 Database Connection
Update the connection string with your Neon credentials:

python
import psycopg2

conn = psycopg2.connect(
    "postgresql://<username>:<password>@<host>/<database>?sslmode=require&channel_binding=require"
)
cur = conn.cursor()
📊 Step 1: Create Table
python
create_odi_sql = """
DROP TABLE IF EXISTS odi_data;

CREATE TABLE odi_data (
    player TEXT PRIMARY KEY,
    span TEXT,
    mat INT,
    inns INT,
    no INT,
    runs INT,
    hs TEXT,
    ave REAL,
    bf INT,
    sr REAL,
    hundreds INT,
    fifties INT,
    ducks INT
);
"""
cur.execute(create_odi_sql)
conn.commit()
print("odi_data table created successfully with PRIMARY KEY on player!")
📝 Step 2: Insert Data
Block 1 (Players 1–5)
python
insert_odi_sql1 = """
INSERT INTO odi_data (player, span, mat, inns, no, runs, hs, ave, bf, sr, hundreds, fifties, ducks) VALUES
('SR Tendulkar (INDIA)','1989-2012',463,452,41,18426,'200*',44.83,21367,86.23,49,96,20),
('KC Sangakkara (Asia/ICC/SL)','2000-2015',404,380,41,14234,'169',41.98,18048,78.86,25,93,16),
('RT Ponting (AUS/ICC)','1995-2012',375,365,39,13704,'164',42.04,17046,80.39,30,82,20),
('ST Jayasuriya (Asia/SL)','1989-2011',445,433,18,13430,'189',32.36,14725,91.20,28,68,34),
('DPMD Jayawardene (Asia/SL)','1998-2015',448,418,39,12650,'144',33.37,16020,78.96,19,77,34);
"""
cur.execute(insert_odi_sql1)
conn.commit()
print("Inserted Block 1 of ODI players")
Block 2 (Players 6–10)
python
insert_odi_sql2 = """
INSERT INTO odi_data (player, span, mat, inns, no, runs, hs, ave, bf, sr, hundreds, fifties, ducks) VALUES
('Inzamam-ul-Haq (Asia/PAK)','1991-2007',378,350,53,11739,'137*',39.52,15812,74.24,10,83,20),
('V Kohli (INDIA)','2008-2019',242,233,39,11609,'183',59.84,12445,93.28,43,55,13),
('JH Kallis (Afr/ICC/SA)','1996-2014',328,314,53,11579,'139',44.36,15885,72.89,17,86,16),
('SC Ganguly (Asia/INDIA)','1992-2007',311,300,23,11363,'183',41.02,15416,73.70,22,72,16),
('R Dravid (Asia/ICC/INDIA)','1996-2011',344,318,40,10889,'153',39.16,15284,71.24,12,83,13);
"""
cur.execute(insert_odi_sql2)
conn.commit()
print("Inserted Block 2 of ODI players")
Block 3 (Players 11–15)
python
insert_odi_sql3 = """
INSERT INTO odi_data (player, span, mat, inns, no, runs, hs, ave, bf, sr, hundreds, fifties, ducks) VALUES
('MS Dhoni (Asia/INDIA)','2004-2019',350,297,84,10773,'183*',50.57,12303,87.56,10,73,10),
('CH Gayle (ICC/WI)','1999-2019',301,294,17,10480,'215',37.83,12019,87.19,25,54,25),
('BC Lara (ICC/WI)','1990-2007',299,289,32,10405,'169',40.48,13808,75.91,19,63,16),
('TM Dilshan (SL)','1999-2016',330,303,41,10290,'161*',39.27,11933,86.23,22,47,11),
('Mohammad Yousuf (Asia/PAK)','1998-2010',288,273,40,9720,'141*',41.71,12944,75.10,15,64,18);
"""
cur.execute(insert_odi_sql3)
conn.commit()
print("Inserted Block 3 of ODI players")
Block 4 (Remaining players 16–33)
python
insert_odi_sql_rest = """
INSERT INTO odi_data (player, span, mat, inns, no, runs, hs, ave, bf, sr, hundreds, fifties, ducks) VALUES
('AC Gilchrist (AUS/ICC)','1996-2008',287,279,11,9619,'172',35.89,9922,96.94,16,55,19),
('AB de Villiers (Afr/SA)','2005-2018',228,218,39,9577,'176',53.50,9473,101.09,25,53,7),
('M Azharuddin (INDIA)','1985-2000',334,308,54,9378,'153*',36.92,12669,74.02,7,58,9),
('PA de Silva (SL)','1984-2003',308,296,30,9284,'145',34.90,11443,81.13,11,64,17),
('RG Sharma (INDIA)','2007-2019',221,214,32,8944,'264',49.14,10063,88.28,28,43,13),
('Saeed Anwar (PAK)','1989-2003',247,244,19,8824,'194',39.21,10938,80.67,20,43,15),
('S Chanderpaul (WI)','1994-2011',268,251,47,8778,'150',41.60,14208,70.74,11,59,15),
('Yuvraj Singh (Asia/INDIA)','2000-2017',304,278,40,8701,'150',36.55,9924,87.67,14,52,18),
('DL Haynes (WI)','1978-1994',238,237,28,8648,'152*',41.37,13707,63.09,17,57,13),
('MS Atapattu (SL)','1990-2007',268,259,32,8529,'132*',37.57,12594,67.72,11,59,14),
('ME Waugh (AUS)','1988-2002',244,236,20,8500,'173',39.35,11053,76.90,18,50,16),
('LRPL Taylor (NZ)','2006-2019',228,212,37,8376,'181*',47.86,10091,83.00,20,50,9),
('V Sehwag (Asia/ICC/INDIA)','1999-2013',251,245,9,8273,'219',35.05,7929,104.33,15,38,15),
('HM Amla (SA)','2008-2019',181,178,14,8113,'159',49.46,9718,88.39,27,39,4),
('HH Gibbs (SA)','1996-2010',248,240,13,8094,'175',36.13,12370,65.43,21,37,22),
('Shahid Afridi (Asia/ICC/PAK)','1996-2015',398,369,27,8064,'124',23.57,6892,117.00,6,39,30),
('SP Fleming (ICC/NZ)','1994-2007',280,269,21,8037,'134*',32.40,11422,71.49,8,49,17),
('MJ Clarke (AUS)','2003-2015',245,223,40,7981,'130',44.58,10104,78.98,8,58,13);
"""
cur.execute(insert_odi_sql_rest)
conn.commit()
print("Inserted remaining ODI players")
🔎 Step 3: Example Queries
1. 🏆 Players with ≥10 centuries
python
cur.execute("SELECT player, hundreds FROM odi_data WHERE hundreds >= 10 ORDER BY hundreds DESC;")
display(pd.DataFrame(cur.fetchall(), columns=["player","hundreds"]))
2. 🔍 Players starting with 'S'
python
cur.execute("SELECT player FROM odi_data WHERE player LIKE 'S%';")
display(pd.DataFrame(cur.fetchall(), columns=["player"]))
3. 📈 Players with >12,000 runs
python
cur.execute("SELECT player, runs FROM odi_data WHERE runs > 12000 ORDER BY runs DESC;")
display(pd.DataFrame(cur.fetchall(), columns=["player","runs"]))
4. 🕰️ Players who debuted after 2000
python
cur.execute("""
SELECT player, span
FROM odi_data
WHERE CAST(split_part(span, '-', 1) AS INTEGER) > 2000;
""")
display(pd.DataFrame(cur.fetchall(), columns=["player","span"]))
5. 👑 Dhoni’s “Finisher Index”
python
cur.execute("""
SELECT player, inns, no, ave,
       ROUND( (CAST(no AS REAL) / inns) * ave, 2 ) AS finisher_index
FROM odi_data
WHERE runs > 5000
ORDER BY finisher_index DESC
LIMIT 10;
""")
display(pd.DataFrame(cur.fetchall(),
                     columns=["player","innings","not_outs","average","finisher_index"]))
