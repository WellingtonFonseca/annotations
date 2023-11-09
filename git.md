## Configurando o git

```
git config --global user.name 'Server Production'
git config --global user.email 'wellington.cesar.fonseca@gmail.com'
git config --global init.defaultBranch main
```

## Criando um repositório no servidor

Um repositório bare é um repositório transitório (como se fosse um github).

```
mkdir -p /home/yourappbr/bare.git
cd /home/yourappbr/bare.git
git init --bare
cd ~
```

No seu computador local, adicione o bare como remoto:

checar app_bare
```
git remote -v
```

remove app_bare
```
git remote rm app_bare
```


```
git remote add bare digitalocean-varejospc:/home/yourappbr/bare.git
git push app_bare <branch>
```

Criando o repositório da aplicação

```
mkdir -p /home/yourappbr/main
cd /home/yourappbr/main
git init
git remote add origin /home/yourappbr/bare.git
git add . && git commit -m 'Initial'
cd ~
```



## para monorepo

No servidor, em app_repo, faça pull:

```
cd /home/yourappbr/main
git pull origin <branch>
```

## para múltiplos repo
1. crie seus respectivos diretorios ex: `app_main` `app_develop`
1. dentro de cada um deles execute o comando de iniciar, apontar a oringem, commit e por fim o pull da branch, exemplo:

```
cd /home/<DIRETORIO_QUE_CRIEI>
git init
git remote add origin /home/yourappbr/bare.git
git add . && git commit -m 'Initial'
git pull origin <NOME_DA_BRANCH>
git checkout <NOME_DA_BRANCH>
```

dir: `app_main`
```
cd /home/yourappbr/app_main
git init
git remote add origin /home/yourappbr/bare.git
git add . && git commit -m 'Initial'
git pull origin main
git checkout main
```

dir: `app_develop`
```
cd /home/yourappbr/app_main
git init
git remote add origin /home/yourappbr/bare.git
git add . && git commit -m 'Initial'
git pull origin develop
git checkout develop
```

# automatizando pull no server
```
cd /home/yourappbr/bare.git/hooks
nano post-receive
```

## para mono repo
no nano:
```
#!/bin/sh
git --work-tree=/CAMINHO/DIRETORIO/PROJETO --git-dir=/CAMINHO/DIRETORIO/BARE.git checkout -f
```

## para múltiplos repo
no nano:
```
#!/bin/sh
git --work-tree=/CAMINHO/DIRETORIO/DIRETORIO_QUE_CRIEI --git-dir=/CAMINHO/DIRETORIO/BARE.git checkout <NOME_DA_BRANCH> -f
git --work-tree=/CAMINHO/DIRETORIO/DIRETORIO_QUE_CRIEI --git-dir=/CAMINHO/DIRETORIO/BARE.git checkout <OUTRO_NOME_DA_BRANCH> -f
```

por fim torne-o executável:

```chmod +x post-receive```
