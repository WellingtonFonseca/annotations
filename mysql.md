```
# criar database:
CREATE DATABASE app_leads;

# criar usuário:
CREATE USER 'app_leads_user'@'%' IDENTIFIED BY 'app_leads_password';

# dar privilégios
REVOKE ALL PRIVILEGES ON *.* FROM 'app_leads_user'@'%';
GRANT ALL PRIVILEGES ON app_leads.* TO 'app_leads_user'@'%';
FLUSH PRIVILEGES;

# validar privilégios
SHOW GRANTS FOR 'app_leads_user'@'%';
```
```
# remover usuário:
DROP USER 'app_user'@'%';
```

```bash
# ufw allow
sudo ufw allow from 192.168.0.1 to any port 3306

# ufw status
sudo ufw status numbered

# ufw remove rule
sudo ufw delete number_rule
```
