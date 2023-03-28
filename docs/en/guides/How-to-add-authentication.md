# How to add authentication

**We are using Nginx as a reverse proxy to secure access to the SkyWalking dashboard

1.Install Nginx: You can install Nginx on your Azure VM by running the following command:
	`sudo apt-get update`
	`sudo apt-get install nginx`

2.Create a htpasswd file to store the login credentials for users who are allowed to access the SkyWalking dashboard:
	`sudo apt install apache2-utils`
	`sudo htpasswd -c /etc/nginx/.htpasswd user1`
This will create a new htpasswd file at `/etc/nginx/.htpasswd` with a single user (`user1`) and prompt you to set a password for the user.

3.Configure Nginx as a reverse proxy for Apache SkyWalking.

 Create a new Nginx configuration file `/etc/nginx/sites-available/skywalking` by using this command `sudo nano /etc/nginx/sites-available/skywalking`

 Add this content to the configuration file
	```
	server {
    listen 80;
    server_name `Azure VM IP` ; # Domain name or Azure VM IP
    location / {
    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/.htpasswd;

    proxy_pass <http://`Azure VM IP`:8080;> # Domain name or Azure VM IP
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}
```
- Nginx listens on port 80 and forwards requests to Apache SkyWalking running on port 8080 on the same machine
- auth_basic directives enable basic authentication and provide the location of the file htpasswd that has the login information.

4. Create a symbolic link to enable the new Nginx configuration
	`sudo ln -s /etc/nginx/sites-available/skywalking /etc/nginx/sites-enabled/`

5. Test the Nginx configuration and restart Nginx:
	`sudo nginx -t`
	`sudo systemctl restart nginx`

6.Access the skywalking by visiting `http://`Azure VM IP`.com`
