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


# recuperar dados via .ibd (mysql 8.0.36)

antes de iniciar, procure realizar o passo a passo no mesmo versionamento do mysql

1. no novo banco de dados, crie o mesmo `database` com as mesmas `tabelas` (IMPORTANTE! Se conseguir criar com o mesmo layout de colunas será ótimo, porque o `.ibd` vai recuperar a mesma ordem dos dados. Mais abaixo e explico melhor)
2. acesse a pasta `data`
3. execute `ls -la` e obtenha o `owner` do diretório, creio que em servers seja `mysql:mysql`, aqui no meu caso aparece `-rw-r-----  1 systemd-coredump systemd-coredump 486539264 abr 13 14:32 tbl_varejo_harmonizacao_vendas.ibd`, logo `systemd-coredump systemd-coredump` (ou traduzindo `systemd-coredump:systemd-coredump`) é o `owner` dos meu diretórios
4. agora que identificamos, vamos aos `.ibd`s, então no diretório `data` você vai encontrar o diretório com o mesmo nome do `database` que criamos anteriormente.
5. execute `sudo rm -rf <nome_do_database>`
6. copie o mesmo diretório do diretório que queremos recuperar `sudo cp -r diretorio_recuperar/data/<nome_do_database> .`
7. agora aplique o `owner` que identificamos no passo 3, `sudo chown systemd-coredump:systemd-coredump <nome_do_database>`
8. acesse o diretório `cd <nome_do_database>`
9. aplique também o `owner` agora a todos os arquivos aqui dentro `sudo chown systemd-coredump:systemd-coredump *.*`
10. feito! reinicie o servidor
11. depois de reiniciado acesse o seu front end (estou usando o dbeaver) e execute `use mysql;`
12. agora você irá importar o `tablespace` para todas as tabelas do seu database `alter table <nome_da_tabela> import tablespace;`

observações:
- eu tive alguns imports que alegaram 
```
InnoDB: Tablespace is missing for table <nome_do_database>/<nome_da_tabela>.
InnoDB: IO Read error: (2, No such file or directory) Error opening './<nome_do_database>/<nome_da_tabela>.cfg', will attempt to import without schema verification
```
mas que no final, os dados foram importados com sucesso. Podendo, claro, estarem fora de layout que para corrigí-los explico logo abaixo.

- e tive outros casos que
```
[1808] Schema mismatch (Clustered index validation failed. Because the .cfg file is missing, table definition of the IBD file could be different. Or the data file itself is already corrupted.)
```
mas que no final ele importou, porém com os dados totalmentes deslocados.

O que identifiquei nestes, é que por exemplo meu layout da tabela está (`nome`, `idade`, `nascimento`) porém no `.ibd` está (`nome`, `nascimento`, `idade`). O que fiz foi, corrigir o layout da tabela para que ficasse idem ao do `.ibd` e repetir o processo de recuperação anteriormente explicado, somente para estas tabelas em específico.

Logo em seguida ao executar o comando de `import tablespace` alegou a divergência acima da falta do `.cfg` mas os dados foram apresentados de forma correta.




