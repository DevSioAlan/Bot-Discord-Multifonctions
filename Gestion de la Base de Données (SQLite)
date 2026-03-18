# --- EXTRAIT : Intégration Base de Données SQLite ---
import sqlite3

# Connexion et création des tables relationnelles
db_conn = sqlite3.connect("database.db")
cursor = db_conn.cursor()
cursor.execute("""CREATE TABLE IF NOT EXISTS economy (user_id INTEGER PRIMARY KEY, money INTEGER DEFAULT 0)""")
db_conn.commit()

def update_balance(user_id, amount):
    # Requête optimisée : Insère l'utilisateur s'il n'existe pas, sinon met à jour son solde
    cursor.execute("""
        INSERT INTO economy (user_id, money) VALUES (?, ?) 
        ON CONFLICT(user_id) DO UPDATE SET money = money + ?
    """, (user_id, amount, amount))
    db_conn.commit()
