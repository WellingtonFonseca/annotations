[font: https://timonweb.com](https://timonweb.com/django/https-django-development-server-ssl-certificate/)
# How to run a local Django development server over HTTPS with a trusted self-signed SSL certificate
#####  Aug 10, 2021 · Updated: Aug 13, 2021 · by Tim Kamanin 



Generating a self-signed SSL certificate for local Django development has always been a hassle for me. Until the day I discovered mkcert, a zero-config tool that creates locally trusted development certificates, your browser will not complain about.

In this tutorial, I'll share my process, and you'll learn how to create a local SSL certificate for your Django project and run it in development mode with HTTPS enabled.

I can't wait to tell you about it, follow me!

## Step 1 - Generating a local SSL certificate

1. First, let's install mkcert on your machine. If you are running on macOS, you can use Homebrew package manager to do this. Run the following command in your terminal:

```
brew install mkcert
```

> If you're running on Linux or Windows, please refer to installation instructions in the package repo (https://github.com/FiloSottile/mkcert#windows).

2. Next, let's make your Operational System trust the local certificates we're about to generate. You need to install a local certificate authority (CA) in the system trust store to do this. Run the following command:

```
mkcert -install
```

3. Next, you need to generate a certificate for the localhost domain.
In the terminal, go to the root of your Django project. Then run the following terminal command to generate a certificate for `localhost` and `127.0.0.1`:

```
mkcert -cert-file cert.pem -key-file key.pem localhost 127.0.0.1
```

> If you plan to run your local server under a domain other than localhost, replace localhost with the domain of your choice.

If everything went right, you should see the following result:

```
output:
The certificate is at "cert.pem" and the key at "key.pem"
```

## Step 2 - Configuring Django server to work with HTTPS

The default Django `manage.py runserver` command doesn't support SSL; therefore, we need to use the alternative `manage.py runserver_plus` command, which is part of the excellent Django Extensions package.

1. Run the following command to install Django extensions alongwith the Wekzeug server:

```
pip install django-extensions Werkzeug
```

> The runserver_plus command requires installation of the Werkzeug server, which is better known in the world of the Python Framework Flask.

2. Next, open the `settings.py` file in your code editor and add `django_extensions` to the `INSTALLED_APPS` list:

```
INSTALLED_APPS = [
    # other apps
    "django_extensions",
]
```

3. Finally, start the local development server in HTTPS mode by running the command:

```
python manage.py runserver_plus 0.0.0.0:8000 --cert-file /home/usuario/varejospc/yourappbr/develop/django/project/settings/environments/cert.pem --key-file /home/usuario/varejospc/yourappbr/develop/django/project/settings/environments/key.pem
```


And that's it; you should now see the local development server running at the default `https://localhost:8000` address.
