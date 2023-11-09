<!-- https://ilovedjango.com/django/tips/bugs/nginx-django-throwing-403-error-forbidden-on-static-files/ -->

sudo nano /etc/nginx/nginx.conf

change:
user www-data;
<!-- to -->
user youruser <!-- or --> user root

sudo systemctl restart nginx

