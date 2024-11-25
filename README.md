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

![Screenshot 2024-11-24 165032](https://github.com/user-attachments/assets/61039e21-ad09-4157-a877-f3a9b027b071)
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

![Screenshot 2024-11-24 212917](https://github.com/user-attachments/assets/881dec7a-afc6-4271-b6bc-116ca281c385)

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


    server_name _;


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
## Task 4

We are going to install ufw, to start, type:
```
sudo pacman -S ufw
```
For the SSH and HTTP, type:
```
sudo ufw allow ssh
```
```
sudo ufw allow http
```
After that, we are going to set by limiting the SSH rate
```
sudo ufw limit ssh
```
To enable ufw, type:
```
sudo ufw enable
```
You can also see the status of your ufw by typing,
```
sudo ufw status
```
----
![Screenshot 2024-11-24 195345](https://github.com/user-attachments/assets/d626e3ec-189c-4cfa-836a-97a28940b417)

**NOTE** I ran into an issue while doing task 4, I was not sure what the issue was so I attempted to do it by,
```
sudo pacman -Syu
```
```
I then went to Digital Ocean to turn off and on
```
It did not work

