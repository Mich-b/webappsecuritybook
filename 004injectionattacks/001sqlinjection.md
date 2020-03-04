# SQL Injection
A SQL injection attack consists of injection of a SQL query via the input data from the client (e.g. a browser) to the application. A successful SQL injection exploit can read sensitive data from the database, modify database data (Insert/Update/Delete), execute administration operations on the database (such as shutdown the database management system), recover the content of a given file present on the database management system file system and in some cases issue commands to the operating system. SQL injection attacks are a type of injection attack.

# Insecure example
Imagine the following code is present in your .NET application:

```
using (DbDataContext context = new DbDataContext())
{
    string q = "SELECT Name from Items where ProductCode = " + model.ProductCode;
    name = context.ExecuteQuery<string>(q).SingleOrDefault().ToString();
}
```

If `model.ProductCode` can be controlled by the user (e.g. https://www.example.org/products/12, where `12` is the productcode), the above code is vulnerable to SQL injection. An attacker may input the following 'productcode': `;SELECT * FROM users;`
This would result in the following query:

```
SELECT Name from Items where ProductCode = ;SELECT * FROM users;
```

Our parameter `;SELECT * FROM users;` is interpreted as SQL code. Can you imagine what the results will be? What if the `users` table contains sensitive data such as plaintext passwords?

# Prevent SQL injection
Fool-proof mechanisms exist in almost every programming language to prevent SQL injection from occurring. Typically, the following prevention measures exist:
* Use parametrized queries
* Use stored procedures (but does not guarantee protection)
* Whitelist input validation (but does not guarantee protection)
* Escape user input (but does not guarantee protection)

The recommended approach is to use parametrized queries. 

Measures also exist to mitigate the impact of a SQL Injection flaw. When an application uses a database to store data, the application (technical) users will be given permissions on the database. It is good practice to limit the permissions for these application users. Typically an application is not going to delete entire tables (`DROP table`), and therefore does not require drop rights. 

# Secure example
Applying the practice of parametrized queries to the insecure example given above, the following code is constructed:

```
 using (DbDataContext context = new DbDataContext())
{
    string q = "SELECT Name from Items where ProductCode = {0}";
    name = context.ExecuteQuery<string>(q, model.ProductCode).SingleOrDefault().ToString();
}
```

Instead of passing the input directly to the command, a parameter (`{0}`) is provided, hence the name _parametrized query_. 
The key here is that the query has already been interpreted before the variable is added. It knows that it will query the Items table with a certain ProductCode. The inserted ProductCode will not be parsed for code. Let's see how this works by reusing our previous malicious input. This would again result in the following query:

```
SELECT Name from Items where ProductCode = ;SELECT * FROM users;
```

There is no difference in the query that is executed, the only difference is that `;SELECT * FROM users;` **will NOT be interpreted as SQL code**, but simply as plain data. 

# Source attribution
Some parts of this page are based on the [SQL Injection Prevention Cheat Sheet](https://owasp.org/www-project-cheat-sheets/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet), which is licensed under [FLOSS](https://owasp.org/about/).