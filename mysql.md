```sql
# criar database:
CREATE DATABASE app_leads;

# criar usuário:
CREATE USER 'app_leads_user'@'%' IDENTIFIED BY 'app_leads_password' REQUIRE SSL;

# dar privilégios
REVOKE ALL PRIVILEGES ON *.* FROM 'app_leads_user'@'%';
GRANT ALL PRIVILEGES ON app_leads.* TO 'app_leads_user'@'%';
FLUSH PRIVILEGES;

# validar privilégios
SHOW GRANTS FOR 'app_leads_user'@'%';

# remover usuário:
DROP USER 'app_user'@'%';

# listar usuarios
select Host, User from mysql. user;

# alterar host usuario
RENAME USER 'user'@'123.4.5' TO 'user'@'123.4.6';

# derrubar sessões presas
SHOW PROCESSLIST;

# localize os IDS
KILL id;

# Verifying that a Connection is Using TLS
# You can verify that a connection is using TLS by checking the connection's Ssl_cipher status variable. If it is non-empty, then the connection is using TLS. For example:

SHOW SESSION STATUS LIKE 'Ssl_cipher';
+---------------+---------------------------+
| Variable_name | Value                     |
+---------------+---------------------------+
| Ssl_cipher    | DHE-RSA-AES256-GCM-SHA384 |
+---------------+---------------------------+
1 row in set (0.00 sec)
```
