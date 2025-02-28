To **create users, databases, assign privileges, and manage passwords** in an **Amazon RDS MySQL** instance, follow these steps:  

---

## **Step 1: Connect to Your RDS MySQL**
Run the following command on your Ubuntu server:  
```bash
mysql -h your-rds-endpoint.rds.amazonaws.com -u your-root-user -p
```
Enter your **RDS root password** when prompted.

---

## **Step 2: Create a New Database**
To create a new database:  
```sql
CREATE DATABASE my_database;
```

---

## **Step 3: Create a New User**
Run this command to **create a user**:  
```sql
CREATE USER 'my_user'@'%' IDENTIFIED BY 'MySecurePassword123!';
```
🔹 `%` means the user can connect from **any IP**.  
🔹 For security, replace `%` with your server's IP:  
```sql
CREATE USER 'my_user'@'your-server-ip' IDENTIFIED BY 'MySecurePassword123!';
```

---

## **Step 4: Grant Privileges to the User**
To give **full access** to the database:  
```sql
GRANT ALL PRIVILEGES ON my_database.* TO 'my_user'@'%';
```
To give **only SELECT, INSERT, and UPDATE** permissions:  
```sql
GRANT SELECT, INSERT, UPDATE ON my_database.* TO 'my_user'@'%';
```
After granting privileges, apply the changes:  
```sql
FLUSH PRIVILEGES;
```

---

## **Step 5: Change a User’s Password**
To **change an existing user's password**, run:  
```sql
ALTER USER 'my_user'@'%' IDENTIFIED BY 'NewSecurePassword456!';
FLUSH PRIVILEGES;
```

---

## **Step 6: View Users and Privileges**
To see all users:  
```sql
SELECT User, Host FROM mysql.user;
```
To check a user’s privileges:  
```sql
SHOW GRANTS FOR 'my_user'@'%';
```

---

## **Step 7: Revoke Privileges (If Needed)**
To **remove all privileges** from a user:  
```sql
REVOKE ALL PRIVILEGES ON my_database.* FROM 'my_user'@'%';
FLUSH PRIVILEGES;
```
To **drop the user** completely:  
```sql
DROP USER 'my_user'@'%';
FLUSH PRIVILEGES;
```

---

## **Step 8: Test Connection to RDS MySQL**
From your Ubuntu server, try logging in as the new user:  
```bash
mysql -h your-rds-endpoint.rds.amazonaws.com -u my_user -p
```
If successful, the new user setup is working! 🚀  

---

### ✅ **Summary of Commands**
| Action | Command |
|---------|---------|
| **Create Database** | `CREATE DATABASE my_database;` |
| **Create User** | `CREATE USER 'my_user'@'%' IDENTIFIED BY 'MySecurePassword123!';` |
| **Grant All Privileges** | `GRANT ALL PRIVILEGES ON my_database.* TO 'my_user'@'%';` |
| **Change Password** | `ALTER USER 'my_user'@'%' IDENTIFIED BY 'NewSecurePassword456!';` |
| **Check Users** | `SELECT User, Host FROM mysql.user;` |
| **Show User Privileges** | `SHOW GRANTS FOR 'my_user'@'%';` |
| **Revoke Privileges** | `REVOKE ALL PRIVILEGES ON my_database.* FROM 'my_user'@'%';` |
| **Delete User** | `DROP USER 'my_user'@'%';` |

Let me know if you need more help! 🚀🔥
