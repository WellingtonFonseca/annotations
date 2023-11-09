fonte: [medium](https://medium.com/@svsh227/error-413-request-entity-too-large-in-nginx-with-client-max-body-size-changes-in-nginx-6aacd525fe11)
# Error: 413 “Request Entity Too Large” in Nginx with “client_max_body_size” / Changes in Nginx config file.

This is happening because your nginx server does not allow you to upload a file that is larger than the defined size in nginx config file. To solve it, you have to modify your nginx configuration.

## Follow the step to remove/modify your nginx config file.

## Step 1: Connect to your nginx server with your terminal.

## Step 2: Go to the config location and open it
```sudo nano /etc/nginx/nginx.conf```

![image](https://miro.medium.com/v2/resize:fit:720/format:webp/1*eP8nnK-c-BcuYAlRfs94aQ.png)

### Step 3: Search for this variable: client_max_body_size. If you find it, just increase its size to 100M,
> If it doesn’t exist, then you can add the below code inside and at the end of HTTP:

```client_max_body_size 10M;```

![image](https://miro.medium.com/v2/resize:fit:720/format:webp/1*eP8nnK-c-BcuYAlRfs94aQ.png)

### Step 4: Save this file.
( If you want to save the changes you’ve made, press Control + O . To exit nano, type Control + X . If you ask nano to exit from a modified file, it will ask you if you want to save it.)

### Step 5: Restart nginx to apply the changes.
```sudo service nginx restart```

![image](https://miro.medium.com/v2/resize:fit:640/format:webp/1*tpw8thL2qirT-aAsSzHhkg.png)
