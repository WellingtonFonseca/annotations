
```
sudo apt-get install gettext
```

# Cuidados importantes ao configurar i18n no Django

A Babilônia caiu não foi por acaso. Internacionalização (aka i18n) é trabalhoso por natureza, e quando você esbarra em sutilezas de configuração pode ser irritante.
Esta semana eu decidi finalmente adaptar o site do Small Acts Manifesto para outros idiomas, e no processo acabei caindo numa cilada de coincidências.
Para que você não cometa o mesmo erro, criei uma receitinha de bolo para configurar o projeto.

### Começe pelo settings.py:
```
USE_I18N = True
LANGUAGES = (
    ('en', u'English'),
    ('pt-br', u'Português'),
)
LOCALE_PATHS = (PROJECT_DIR.child('locale'),)
```

É simples, mas temos alguns pontos de atenção:

- O [LANGUAGES](https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-LANGUAGES) é uma tupla de tuplas. O primeiro elemento das tuplas é o language code. Para português brasileiro o valor é `pt-br`, __tudo minúsculo separado por hífen__.

- [LOCALE_PATHS](https://docs.djangoproject.com/en/dev/ref/settings/#locale-paths) é escrito no plural e aceita vários diretórios, portanto é uma tupla contendo strings.

- Se você não sabe o que é o __PROJECT_DIR__, [veja nesta palestra](https://www.youtube.com/watch?v=dNJXN70Nqt0&feature=youtu.be&t=41m20s).

### Crie os arquivos .po

Cada idioma tem um “código fonte” da tradução.
O Django varre todo o código fonte e reúne as strings marcadas para tradução com o comando:

```
python manage.py makemessages -l en -l pt_BR
```

Atenção, o parâmetro deve ser __pt_BR__, __com pt em minúsculo e BR em maiúsculo__, separados por underscore. Esse formato é o locale name e este valor será usado para criar o diretório que conterá o arquivo django.po.

### Usando as traduções

Depois de definir as traduções editando os arquivos .po, basta empacotar a tradução, compilando os .po em .mo para os idiomas disponíveis:
```
python manage.py compilemessages
```

Ao longo do projeto você vai usar várias vezes o makemessages e o compilemessages.
Se você for realizar traduções colaborativas, o Transifex pode ajudar bastante e é grátis para projetos open source.


### Mas e a cilada?

Quando eu implementei conteúdo em português tudo funcionava bem no ambiente de desenvolvimento, mas no ambiente de stage o site só exibia o idioma padrão que é o inglês. Clássico caso de [works on my machine](http://www.keepcalm-o-matic.co.uk/p/keep-calm-it-works-on-my-machine/). Como pode?
O problema era que eu estava desenvolvendo no Mac e o site está hospedado em Linux.
Por desatenção, eu executei o `makemessages -l pt_br`, em minúsculo.
Na minha máquina tudo funcionava, porque o filesystem do Mac não é case sensitive por padrão, logo conseguia encontrar o diretório. Já no Linux, era como se não existisse dicionário disponível em português.
