...
Ernest Ankrah
CS 623 Programming Project
In this project a delete transaction will be performed on a MySQl Database


#load drivers
import mysql.connector
from mysql.connector import mysql.connector

def delete_product_p1():
    try:
        # Connect to the MySQL database
        conn = mysql.connector.connect(
            host='localhost',
            database='mysql',
            user='root',
            password='password'
        )
        if conn.is_connected():
            cursor = conn.cursor()
            print("Connected to the database.")

            # Start a transaction (Isolation)
            conn.start_transaction()
            print("Transaction started.")

            try:
                # Delete from Stock table (Consistency)
                cursor.execute("DELETE FROM Stock WHERE prodid = %s", ('p1',))
                print("Deleted from Stock table.")

                # Delete from Product table (Consistency)
                cursor.execute("DELETE FROM Product WHERE prodid = %s", ('p1',))
                print("Deleted from Product table.")

                # Commit the transaction if all operations succeed (Atomicity and Durability)
                conn.commit()
                print("Transaction committed successfully.")
            except Exception as e:
                # Rollback the transaction if any operation fails (Atomicity)
                conn.rollback()
                print(f"Transaction failed and rolled back: {e}")
            finally:
                # Close the cursor and connection
                cursor.close()
                conn.close()
                print("Transaction ended.")

                
    
# Call the function to delete product p1 and demonstrate ACID properties
delete_product_p1()
