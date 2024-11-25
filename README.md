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

