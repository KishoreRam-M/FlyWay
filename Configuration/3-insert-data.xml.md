### **🔥 Optimized & Advanced Changelog**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
                   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
                   http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.8.xsd">

    <!-- ========================= -->
    <!-- INSERT USERS (Bulk Insert) -->
    <!-- ========================= -->
    <changeSet id="3-insert-users" author="admin" context="dev,staging,prod">
        <loadData tableName="users" file="data/users.csv" separator="," encoding="UTF-8">
            <column name="username" type="STRING"/>
            <column name="email" type="STRING"/>
            <column name="created_at" type="TIMESTAMP"/>
        </loadData>

        <rollback>
            <delete tableName="users">
                <where>username IN ('john_doe', 'jane_smith')</where>
            </delete>
        </rollback>
    </changeSet>

    <!-- =========================== -->
    <!-- UPDATE USERS (Modify Emails) -->
    <!-- =========================== -->
    <changeSet id="4-update-user-emails" author="admin" context="all">
        <update tableName="users">
            <column name="email" value="john.doe@newdomain.com"/>
            <where>username='john_doe'</where>
        </update>

        <update tableName="users">
            <column name="email" value="jane.smith@newdomain.com"/>
            <where>username='jane_smith'</where>
        </update>

        <rollback>
            <update tableName="users">
                <column name="email" value="john.doe@example.com"/>
                <where>username='john_doe'</where>
            </update>

            <update tableName="users">
                <column name="email" value="jane.smith@example.com"/>
                <where>username='jane_smith'</where>
            </update>
        </rollback>
    </changeSet>

    <!-- ===================================== -->
    <!-- ADVANCED: ONLY UPDATE IF RECORD EXISTS -->
    <!-- ===================================== -->
    <changeSet id="5-conditional-update" author="admin" context="all" runOnChange="true">
        <preConditions onFail="MARK_RAN">
            <sqlCheck expectedResult="1">
                SELECT COUNT(*) FROM users WHERE username='john_doe'
            </sqlCheck>
        </preConditions>

        <update tableName="users">
            <column name="email" value="john.doe@updated.com"/>
            <where>username='john_doe'</where>
        </update>

        <rollback>
            <update tableName="users">
                <column name="email" value="john.doe@newdomain.com"/>
                <where>username='john_doe'</where>
            </update>
        </rollback>
    </changeSet>

</databaseChangeLog>
```

---

### **🔥 Advanced Features Explained**
1️⃣ **✅ Bulk Inserts Using `loadData`**  
   - Loads users from `data/users.csv` instead of writing multiple `<insert>` statements.
   - **Efficient for large datasets**.  

2️⃣ **✅ Update User Emails with Rollback**  
   - If the email format changes, it’s automatically updated.  
   - **Rollback restores old email values**.  

3️⃣ **✅ Conditional Update Using `preConditions`**  
   - Ensures **the update runs only if the user exists** (`SELECT COUNT(*)`).  
   - Prevents Liquibase errors if the user is missing.  

---
