# Assignment03P1_2420

# Assignment3 Part 1


## Task 1

- First set up a system user webgen with a home directory by using the code below
```
sudo useradd -r -m -d /var/lib/webgen -s /usr/sbin/nologin
```

- We will then set up the files to the directory
```
sudo mkdir -p /var/lib/webgen/bin
```
```
sudo mkdir -p /var/lib/webgen/HTML
```
- Now create the files 
```
sudo touch /var/lib/webgen/bin/generate_index
```
```
sudo touch /var/lib/webgen/HTML/index.html
```
- Then set the ownership
```
sudo chown -R webgen:webgen /var/lib/webgen
```
- Now you're done, to see your structure, just type:
```
sudo tree /var/lib/webgen
```
- This is the picture (below) of what it suppose to look like.
----
## Task 2

We will now create a service file for both generate-index.service and generate-index.timer
```
nvim generate-index.service
```
```
nvim generate-index.timer
```
This is what the generate-index.service file should look like
```
[Unit]
Description=The generate_index script 
Wants=network-online.target
After=network-online.target

[Service]
Type=oneshot
User=webgen
Group=webgen
ExecStart=/var/lib/webgen/bin/generate_index

[Install]
WantedBy=multi-user.target
```
This is what the generate-index.service should look like
```
[Unit]
Description=Run the generate_index at 5am on a daily basis

[Timer]
OnCalendar=*-*-* 05:00:00
Persistent=true

[Install]
WantedBy=timer.target
```
Since we finished creating the service file, we need to start and enable the services
```
sudo systemctl start generate-index.service
sudo systemctl enable generate-index.service

sudo systemctl start generate-index.timer
sudo systemctl enable generate-index.timer
```
You can check your services status by typing:
```
sudo systemctl status generate-index.service
```
```
sudo systemctl status generate-index.service
```
**Note** There were some issues with this part, as seen on the picture, they are some parts that are working and some not. I attempted to try again, but i did not manage to fix it.

----
## Task 3

For this task, we will have to do install and configure nginx
- To install nginx type, 
```
sudo pacman -S ngnix
```
- after that, create and edit the ngnix.config file

Before you go ahead, make sure to create directories so you won't face any issues later on.
```
sudo mkdir /etc/nginx/sites-available
```
```
sudo mkdir /etc/nginx/sites-enabled
```
----
Create this,
```
sudo nvim /etc/nginx/nginx.conf
```
**Important** The only thing you need to edit in the ngnix.conf file is replace the #user http; (top left) with user webgen; pictures below for example,

Before

After
----

Now create a file for the server block file
```
sudo nvim /etc/nginx/sites-available/(However you name it, ex: task3_webgen)
```
Then add the code to the file 
```
server {
    listen 80;
    listen [::]:80;


    server_name task3_webgen


    root /var/lib/webgen/HTML;
    index index.html;


    	location / {
	try_files $uri $uri/ =404
    }
}
```
Now that you're done, you can test it by typing,
```
sudo nginx -t
```
You can also type the command (below)
```
sudo systemctl daemon-reload
```
- This (code) so it can restart
```
sudo systemctl start nginx
```
```
sudo systemctl enable nginx
```
- These two codes (above) can start and enable the ngnix service

to check if ngnix works, type:
```
sudo systemctl status nginx
```
----


