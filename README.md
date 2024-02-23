# epicbox-nodejs

Files for Installing the epicbox Server

Read the epicbox Installation pdf for specifics

Includes binaries as well as sample config and service files

## Installation:

Install and Run an Epicbox Relay Server

If you would like to run a local epicbox server for your state or country, please follow these
steps. It is assumed you are logged in as root or running sudo su.

Prepare a server class computer running linux (Ubuntu 20.04, Mint 20.3, VPS is OK)

Create user 'epicbox'
cd /home/epicbox

Download epicbox.zip and unzip into /home/epicbox
chown files to epicbox and make binaries executable (if not already) in /home/epicbox

Create a DNS entry pointing to epicbox.your-domain

Install nginx

In epicbox.nginx: modify domain to epicbox.your.domain then copy to /etc/nginx/sites-enabled
- $ systemctl restart nginx

Install certbot and run to update the nginx file:
- $ apt install certbot
- $ apt install certbot python3-certbot-nginx
- $ certbot --nginx --redirect -d your.domain -d epicbox.your.domain -m youremailaddress --agree-tos --no-eff-email

Restart nginx service
- $ systemctl restart nginx

Install mongodb: https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition

Copy mongod.conf to /etc
- $ service mongod start
- $ service mongod status

Monitor mongodb with 'journalctl -fu mongod.service'

Epicbox Server:

In config.json: change "epicbox_domain": "epicbox.epicnet.us" to your epicbox.your.domain
- $ cp epicbox.service /etc/systemd/system
- $ systemctl daemon-reload
- $ systemctl start epicbox.service
- $ systemctl enable epicbox.service

Monitor epicbox with 'journalctl -fu epicbox.service'

Creante mongodb index:
- $ mongosh
```
use epicbox
db.slates.createIndex({queue:1, made:1, createdat: 1})
db.slates.createIndex({messageid:1, made:1})
db.slates.createIndex({ "createdat": 1 }, {expireAfterSeconds: 604800 })
quit()
``` 
Note: epicbox created from app_mongo.js at https://github.com/EpicCash/epicboxnodejs-source

(run pkg -t node18 -o epicbox app_mongo.js)

