> Para seguir este tutorial, você precisará de um servidor executando o Ubuntu 20.04. Cada servidor deverá ter um non-root user administrativo e um firewall configurado com o UFW. Configure isso seguindo o nosso Guia de configuração inicial de servidor para o Ubuntu 20.04.

# Passo 1 — Instalando o MariaDB

```bash
$ sudo apt update
$ sudo apt-get install mariadb-server
```

# Passo 2 — Configurando o MariaDB
```bash
$ sudo mysql_secure_installation
```
Isso levará você a uma série de prompts onde é possível fazer algumas alterações nas opções de segurança de sua instalação do MariaDB. O primeiro prompt pedirá que digite a senha atual do root do banco de dados. Como ainda não configuramos uma senha, pressione `ENTER` para indicar “nenhuma”.
```bash
Output
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
```

O próximo prompt pergunta a você se deseja configurar uma senha root do banco de dados. No Ubuntu, a conta root para o MariaDB está intimamente ligada à manutenção automatizada do sistema. Desse modo, não se deve alterar os métodos de autenticação configurados para esta conta. Se isso fosse feito, uma atualização de pacotes poderia quebrar o sistema de banco de dados devido a remoção do acesso à conta administrativa. Digite `N` e, em seguida, pressione `ENTER`.

```bash
Output
. . .
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] N
```

Mais tarde, vamos tratar de como configurar uma conta administrativa adicional para o acesso por senha caso a autenticação por soquete não seja apropriada para o seu caso de uso.

A partir daí, pressione `Y` e, depois, `ENTER` para aceitar as configurações padrão para todas as perguntas subsequentes. Isso removerá alguns usuários anônimos e o banco de dados teste, desativará os logins remotos ao root e carregará essas novas regras para que o MariaDB respeite imediatamente as alterações que você fez.

Com isso, você terminou a configuração inicial de segurança do MariaDB. O próximo passo é opcional, embora você deva segui-lo se preferir autenticar-se ao seu servidor MariaDB com uma senha.

# Passo 3 — (Opcional) Criando um usuário administrativo que implante a autenticação por senha
Em sistemas Ubuntu executando o MariaDB 10.3, o root user do MariaDB é configurado para autenticar-se usando o plug-in `unix_socket` por padrão, em vez de fazê-lo com uma senha. Isso permite maior segurança e usabilidade na maioria dos casos, mas também pode complicar as coisas quando for necessário permitir direitos administrativos a um programa externo (por exemplo, o phpMyAdmin).

Como o servidor usa a conta root para tarefas como a rotação de registro e a inicialização e parada do servidor, é melhor não alterar os detalhes de autenticação da conta root. A alteração das credenciais no arquivo de configuração `/etc/mysql/debian.cnf` pode funcionar inicialmente, porém, atualizações de pacotes podem resultar na substituição destas alterações. Em vez de modificar a conta root, os mantenedores de pacotes recomendam criar uma conta administrativa separada, para o acesso baseado em senha.

Para fazer isso, criaremos uma nova conta chamada admin com as mesmas capacidades que a conta root, mas configurada para a autenticação por senha. Abra o prompt do MariaDB do seu terminal:

```bash
$ sudo mariadb
```

Em seguida, crie um novo usuário com privilégios root e acesso baseado em senha. Certifique-se de alterar o nome de usuário e senha para que correspondam às suas preferências:

```
GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

Recarregue os privilégios para garantir que eles estão salvos e disponíveis na sessão atual:

```
FLUSH PRIVILEGES;
```

Em seguida, saia do shell do MariaDB:

```
exit;
```

# Passo 4 — Testando o MariaDB

Quando o MariaDB é instalado dos repositórios padrão, ele normalmente é iniciado de maneira automática. Para testar isso, verifique o status dele.

```bash
$ sudo systemctl status mariadb
```

Você receberá um resultado que é parecido com este:

```bash
Output
● mariadb.service - MariaDB 10.3.22 database server
     Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2020-05-12 13:38:18 UTC; 3min 55s ago
       Docs: man:mysqld(8)
             https://mariadb.com/kb/en/library/systemd/
   Main PID: 25914 (mysqld)
     Status: "Taking your SQL requests now..."
      Tasks: 31 (limit: 2345)
     Memory: 65.6M
     CGroup: /system.slice/mariadb.service
             └─25914 /usr/sbin/mysqld
. . .
```

Se o MariaDB não estiver em execução, inicie-o com o comando `sudo systemctl start mariadb`.

Como verificação adicional, tente se conectar ao banco de dados usando a ferramenta `mysqladmin`. Esta ferramenta é um cliente que permite que você execute comandos administrativos. Por exemplo, este comando diz para se conectar ao MariaDB como root usando o soquete Unix e retornar a versão:

```bash
$ sudo mysqladmin version
```

Você receberá um resultado semelhante a este:

```bash
Output
mysqladmin  Ver 9.1 Distrib 10.3.22-MariaDB, for debian-linux-gnu on x86_64
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Server version		10.3.22-MariaDB-1ubuntu1
Protocol version	10
Connection		Localhost via UNIX socket
UNIX socket		/var/run/mysqld/mysqld.sock
Uptime:			4 min 49 sec

Threads: 7  Questions: 467  Slow queries: 0  Opens: 177  Flush tables: 1  Open tables: 31  Queries per second avg: 1.615
```

Se você configurou um usuário administrativo separado com autenticação por senha, execute a mesma operação digitando:

```bash
$ mysqladmin -u admin -p version
```

# Conclusão

Neste guia, você instalou o sistema de gerenciamento de banco de dados relacional MariaDB e o protegeu usando o script `mysql_secure_installation` que veio instalado com ele. Você também teve a opção de criar um novo usuário administrativo que utiliza a autenticação por senha antes de testar a funcionalidade do servidor MariaDB.

[fonte: digital ocean](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-20-04-pt#passo-2-configurando-o-mariadb)

# mude a porta de acesso

```bash
$ nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

na seção `[mysqld]` remova o `#` de `port` e altere para o `NUMERO_DA_PORTA` que desejar. Salve e feche.

reinicie `$ sudo systemctl restart mysql`

valide se o número da porta está respondendo com `/mysqld`

```bash
$ sudo netstat -anp | grep NUMERO_DA_PORTA
```

caso sim, estamos OK

ufw allow:
```bash
$ sudo ufw allow from IP_ADDRESS to any port NUMERO_DA_PORTA
```

ufw status:
```bash
$ sudo ufw status numbered
```

ufw remove rule:
```bash
$ sudo ufw delete NUMBERED_RULE
```


# Step 3 – Create the CA certificate (TLS/SSL)

Make a directory named ssl in /etc/mysql/ directory using the mkdir command:

```bash
$ cd /etc/mysql
$ sudo mkdir ssl
$ cd ssl
```

> Note: Common Name value used for the server and client certificates/keys must each differ from the Common Name value used for the CA certificate. To avoid any issues, I am setting them as follows. Otherwise, you will get certification verification failed error. Hence set it as follows:
> 
> CA common Name : __MariaDB admin__
> 
> Server common Name: __MariaDB server__
> 
> Client common Name: __MariaDB client__

Type the following command to create a new CA key:
```bash
$ sudo openssl genrsa 2048 > ca-key.pem
```
or
```bash
$ sudo openssl genrsa 4096 > ca-key.pem
```

The output is as follows:
```bash
Generating RSA private key, 2048 bit long modulus
....................+++
....................................................+++
e is 65537 (0x10001)
```

Type the following command to generate the certificate using that key:
```bash
sudo openssl req -new -x509 -nodes -days 365000 -key ca-key.pem -out ca-cert.pem
```

 Sample outputs:
 ```bash
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:MariaDB admin
Email Address []:
```

Now you must have two files as follows: `ca-cert.pem`  `ca-key.pem`

# Step 4 – Create the server SSL certificate

To create the server key, run:

```bash
$ sudo openssl req -newkey rsa:2048 -days 365000 -nodes -keyout server-key.pem -out server-req.pem
```

Sample outputs:
```
Generating a 2048 bit RSA private key
..........................................+++
.......................................................................................+++
writing new private key to 'server-key.pem'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:MariaDB server
Email Address []:
to be sent with your certificate request
A challenge password []:admin@123
An optional company name []:
```

Next process the server RSA key, enter:

```bash
$ sudo openssl rsa -in server-key.pem -out server-key.pem
```

Sample outputs:

```bash
writing RSA key
```

Finally sign the server certificate, run:
```bash
sudo openssl x509 -req -in server-req.pem -days 365000 -CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 -out server-cert.pem
```

Sample outputs:
```bash
Signature ok
subject=/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd/CN=MariaDB server
Getting CA Private Key
```

Now you must have additional files: `server-cert.pem`  `server-key.pem`

You must use above two files on MariaDB server itself and any other nodes that you are going to use for cluster/replication traffic. These two files will secure server side communication.

# Step 5 – Create the client TLS/SSL certificate

The mysql client, PHP/Python/Perl/Ruby app is going to use the client certificate to secure client-side connectivity. You must install the following files on all of your clients including the web server. To create the client key, run:

```bash
sudo openssl req -newkey rsa:2048 -days 365000 -nodes -keyout client-key.pem -out client-req.pem
```

 Sample outputs:
 ```bash
Generating a 2048 bit RSA private key
......................................................................................+++
..............+++
writing new private key to 'client-key.pem'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:MariaDB client
Email Address []:
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Next, process client RSA key, enter:
```bash
$ sudo openssl rsa -in client-key.pem -out client-key.pem
```

Sample outputs:

```bash
writing RSA key
```

Finally, sign the client certificate, run:
```bash
$ sudo openssl x509 -req -in client-req.pem -days 365000 -CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 -out client-cert.pem
```

Sample outputs:
```bash
Signature ok
subject=/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd/CN=MariaDB client
Getting CA Private Key
```

# Step 6 – How do I verify the certificates?

Type the following command to verify the certificates to make sure everything was created correctly:

```bash
$ openssl verify -CAfile ca-cert.pem server-cert.pem client-cert.pem
```

 Sample outputs:

 ```bash
server-cert.pem: OK
client-cert.pem: OK
```

There should not be any error and you must get OK answer for both server and client certificates

# Step 7 – Configure the MariaDB server to use SSL

Edit the `/etc/mysql/mariadb.conf.d/50-server.cnf` (or `/etc/mysql/mariadb.cnf`) as follows:

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Append/edit in `[mysqld]` as follows:
```nano
### MySQL Server ###
## Securing the Database with ssl option and certificates ##
## There is no control over the protocol level used. ##
##  mariadb will use TLSv1.0 or better.  ##
#ssl
ssl-ca=/etc/mysql/ssl/ca-cert.pem
ssl-cert=/etc/mysql/ssl/server-cert.pem
ssl-key=/etc/mysql/ssl/server-key.pem
```

Save and close the file. Secure keys using the chmod command/chown command:

```bash
$ sudo chown -Rv mysql:root /etc/mysql/ssl/

changed ownership of '/etc/mysql/ssl/server-key.pem' from root:root to mysql:root
changed ownership of '/etc/mysql/ssl/client-req.pem' from root:root to mysql:root
changed ownership of '/etc/mysql/ssl/server-req.pem' from root:root to mysql:root
changed ownership of '/etc/mysql/ssl/ca-key.pem' from root:root to mysql:root
changed ownership of '/etc/mysql/ssl/server-cert.pem' from root:root to mysql:root
changed ownership of '/etc/mysql/ssl/client-cert.pem' from root:root to mysql:root
changed ownership of '/etc/mysql/ssl/ca-cert.pem' from root:root to mysql:root
changed ownership of '/etc/mysql/ssl/client-key.pem' from root:root to mysql:root
changed ownership of '/etc/mysql/ssl/' from root:root to mysql:root
```

You can restart mariadb as follows:
```bash
$ sudo systemctl restart mysql
```

Make sure no ssl error reported. Here is simple way to verify that using the grep command
```bash
$ sudo grep ssl /var/log/syslog
$ sudo grep ssl /var/log/syslog | grep key
$ sudo grep mysqld /var/log/syslog | grep -i ssl
```

For check error log:
```bash
$ cat /var/log/mysql/error.log
```

# Step 8 – Configure the MariaDB client to use SSL

Configure the MariaDB client such as 192.168.1.200 to use SSL (add in the /etc/mysql/mariadb.conf.d/50-mysql-clients.cnf ):
```bash
sudo nano /etc/mysql/mariadb.conf.d/50-mysql-clients.cnf
```

Append/edit in `[client]`  section:
```nano
## MySQL Client Configuration ##
ssl-ca=/etc/mysql/ssl/ca-cert.pem
ssl-cert=/etc/mysql/ssl/client-cert.pem
ssl-key=/etc/mysql/ssl/client-key.pem
```

Save and close the file.
[fonte: cyberciti](https://www.cyberciti.biz/faq/how-to-setup-mariadb-ssl-and-secure-connections-from-clients/)

agora no seu cliente replique os três arquivos abaixo para conexão:
``` bash
$ cat /etc/mysql/ssl/ca-cert.pem
$ cat /etc/mysql/ssl/client-cert.pem
$ cat /etc/mysql/ssl/client-key.pem
```
