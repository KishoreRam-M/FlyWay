## 📌 **Liquibase Changelog for MySQL (Simple Explanation)**  

Liquibase helps manage **database changes** in a structured way. Instead of writing raw SQL, we use **XML files** to create tables, insert data, and modify the database.  

### **🔹 Overview:**
1. **`changelog-master.xml`** → The **main file** that includes all other files.  
2. **`1-create-tables.xml`** → Creates tables in the database.  
3. **`2-insert-data.xml`** → Adds sample data to the tables.  
4. **`3-add-index.xml`** → Improves database speed by adding an **index**.  

Each of these files contains **small pieces of database changes** called **change sets**.

---

## **1️⃣ `changelog-master.xml` (Main File)**
📌 **This file acts like a table of contents.** It includes other files so they run in order.  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.8.xsd">

    <include file="changelog/1-create-tables.xml"/>
    <include file="changelog/2-insert-data.xml"/>
    <include file="changelog/3-add-index.xml"/>
    
</databaseChangeLog>
```
✅ **What this does:**  
- Reads and runs `1-create-tables.xml` first (to create tables).  
- Then, runs `2-insert-data.xml` (to add sample data).  
- Finally, runs `3-add-index.xml` (to improve search speed).  

---

## **2️⃣ `1-create-tables.xml` (Creating Tables)**
📌 **This file creates a `users` table.**  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
                   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
                   http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.8.xsd">

    <changeSet id="1" author="admin">
        <createTable tableName="users">
            <column name="id" type="BIGINT" autoIncrement="true">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="username" type="VARCHAR(50)">
                <constraints unique="true" nullable="false"/>
            </column>
            <column name="email" type="VARCHAR(100)">
                <constraints nullable="false"/>
            </column>
            <column name="created_at" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP"/>
        </createTable>
    </changeSet>

</databaseChangeLog>
```
✅ **What this does:**  
- **Creates a table** called `users`.  
- **Adds columns**:  
  - `id` → A unique ID for each user.  
  - `username` → Must be **unique** (no duplicates allowed).  
  - `email` → Required, but not unique.  
  - `created_at` → Auto-fills with the current time when a user is added.  

---

## **3️⃣ `2-insert-data.xml` (Adding Sample Data)**
📌 **This file adds example users to the `users` table.**  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
                   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
                   http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.8.xsd">

    <changeSet id="2" author="admin">
        <insert tableName="users">
            <column name="username" value="john_doe"/>
            <column name="email" value="john.doe@example.com"/>
        </insert>

        <insert tableName="users">
            <column name="username" value="jane_smith"/>
            <column name="email" value="jane.smith@example.com"/>
        </insert>
    </changeSet>

</databaseChangeLog>
```
✅ **What this does:**  
- Adds **two users** (`john_doe` and `jane_smith`) to the database.  
- These users will automatically appear in the `users` table when Liquibase runs.  

---

## **4️⃣ `3-add-index.xml` (Adding an Index for Faster Searches)**
📌 **This file improves database performance.**  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
                   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
                   http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.8.xsd">

    <changeSet id="3" author="admin">
        <createIndex indexName="idx_users_email" tableName="users">
            <column name="email"/>
        </createIndex>
    </changeSet>

</databaseChangeLog>
```
✅ **What this does:**  
- Creates an **index** (a shortcut for the database) on the `email` column.  
- **Speeds up searches** when looking for users by email.  

---

## **💡 How to Use This Changelog**
Once these XML files are created, run the command:  
```sh
liquibase update --changelog-file=changelog-master.xml
```
✅ **This will:**  
1. Create the `users` table.  
2. Insert two users.  
3. Add an index for fast email searches.  

---

## **🌎 Real-Life Analogy**
Imagine you’re **organizing a bookshelf**:  
📌 **Step 1** → You **build the bookshelf** (**create table**).  
📌 **Step 2** → You **add books to the shelf** (**insert data**).  
📌 **Step 3** → You **put labels on the books** so you can find them faster (**add index**).  

---

## **🔹 Summary**
| XML File | Purpose |
|----------|---------|
| **`changelog-master.xml`** | Includes all other changelog files. |
| **`1-create-tables.xml`** | Creates the `users` table. |
| **`2-insert-data.xml`** | Adds example users. |
| **`3-add-index.xml`** | Improves search speed. |

\
