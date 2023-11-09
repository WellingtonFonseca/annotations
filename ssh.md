## ssh key
no seu computador
```
ssh-keygen -C "nomedousuario" -f nomedasshkey
```

## ssh sshhostnamepersonalizado

no seu computador
```
sudo nano /etc/ssh/ssh_config


[...]
Host sshhostnamepersonalizado
        HostName 0.0.0.0
        Port 22
        ServerAliveInterval 100
        User root
        IdentityFile /meu/usuario/.ssh/nomedasshkey
[...]

```

## ssh não congelar

no servidor
```
nano /etc/ssh/sshd_config


[...]
ClientAliveInterval 60
TCPKeepAlive yes
ClientAliveCountMax 10000
[...]
```

__ClientAliveInterval:__ The server will wait 60 seconds before sending a null packet to the client to keep the connection alive

__TCPKeepAlive:__ Is there to ensure that certain firewalls don't drop idle connections.

__ClientAliveCountMax:__ Server will send alive messages to the client even though it has not received any message back from the client.

reinicie o __ssh server__: `service ssh restart` ou `service sshd restart`
