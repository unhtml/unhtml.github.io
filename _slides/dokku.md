---
marp: true
class:
  - lead
  - invert
---



# Dokku 
## open source PAAS laternative to  heroku
### 黄滚@2023.2

---

An open source PAAS alternative to Heroku.
Dokku helps you build and manage the lifecycle of applications from building to scaling.
based on Git and Docker

[dokku](https://dokku.com/)


--- 
# Installation

## step 1. download the installation script

> wget https://dokku.com/install/v0.29.4/bootstrap.sh

## step 2. run the installer
>sudo apt install docker.io # optional
>sudo DOKKU_TAG=v0.29.4 bash bootstrap.sh
or 
>sudo http_proxy=.... DOKKU_TAG=v0.29.4 bash bootstrap.sh

---
# Installation

## step 3. Configure your server domain

>dokku domains:set-global dokku.me

## step 4. and your ssh key to the dokku user

>echo "your-public-key-contents-here" | dokku ssh-keys:add admin

---
# first deployment

## step 1. clone the sample code
>git clone git@github.com:heroku/go-getting-started.git

## step 2. edit the code
enjoy the coding experirence

---

# first deployment
## step 3. choose a nice name and add remote to git
> git remote add dok dokku@17.biying.cn:go-app-1

## step 4. push to remote and all is done
> git push dok master

---

# installation under the fucking GFW
>cat /home/dokku/ENV

    CURL_CONNECT_TIMEOUT="90"
    CURL_TIMEOUT="600"
    DOKKU_PROXY_PORT="3000"
    HTTP_PROXY="{your http proxy address}"

---

# How to change domain name

    Usage: dokku domains[:COMMAND]
    Manage domains used by the proxy
    
    Additional commands:
    
    domains:add <app> <domain> [<domain> ...]       Add domains to app
    domains:add-global <domain> [<domain> ...]      Add global domain names
    domains:clear <app>                             Clear all domains for app
    domains:clear-global                            Clear global domain names
    domains:disable <app>                           Disable VHOST support
    domains:enable <app>                            Enable VHOST support
    domains:remove <app> <domain> [<domain> ...]    Remove domains from app
    domains:remove-global <domain> [<domain> ...]   Remove global domain names
    domains:report [<app>|--global] [<flag>]        Displays a domains report for one or more apps
    domains:set <app> <domain> [<domain> ...]       Set domains for app
    domains:set-global <domain> [<domain> ...]      Set global domain names

---

# enter docker container

> dokku enter <app>

![image](https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/ed/ede6311771981710e5ba2f965d968676.png)

---

# ssl support
## install letsencrypt plug
>sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
## enable ssl
>letsencrypt:enable <app>    

---
# ssl support

> dokku letsencrypt:set myapp email your@email.tld  

    -----> Setting email to your@email.tld

>dokku letsencrypt:enable myapp

    =====> Let's Encrypt myapp...
    -----> Updating letsencrypt docker image...
    latest: Pulling from dokku/letsencrypt
    ...
    -----> Certificate retrieved successfully.
    -----> Symlinking let's encrypt certificates
    -----> Configuring SSL for myapp.mydomain.com...(using /var/lib/dokku/plugins/available/nginx-vhosts/templates/nginx.ssl.conf.template)
    -----> Creating https nginx.conf
    -----> Running nginx-pre-reload
       Reloading nginx
    -----> Disabling letsencrypt proxy for myapp...
       done

---
# ssl support
## other commands
    
    dokku letsencrypt
    Usage: dokku letsencrypt[:COMMAND]
    Manage the letsencrypt integration

    Additional commands:

    letsencrypt:active <app>                Verify if letsencrypt is active for an app
    letsencrypt:auto-renew <app>            Auto-renew app if renewal is necessary
    letsencrypt:auto-renew                  Auto-renew all apps secured by letsencrypt if renewal is necessary
    letsencrypt:cleanup <app>               Remove stale certificate directories for app
    letsencrypt:cron-job [--add --remove]   Add or remove a cron job that periodically calls auto-renew.
    letsencrypt:disable <app>               Disable letsencrypt for an app
    letsencrypt:enable <app>                Enable or renew letsencrypt for an app
    letsencrypt:help                        Display letsencrypt help
    letsencrypt:list                        List letsencrypt-secured apps with certificate expiry times
    letsencrypt:revoke <app>                Revoke letsencrypt certificate for app
---

# MySQL support 
## install mysql plugin

>sudo dokku plugin:install https://github.com/dokku/dokku-mysql.git mysql

---

# MySQL support
## create MySQL instance
># usage
>dokku mysql:create <service> [--create-flags...]

## print the information
># usage
>dokku mysql:info <service> [--single-info-flag]

---

# MySQL support
## link mysql service to app

># usage
>dokku mysql:link <service> <app> [--link-flags...]

docker environments will be set:(suppose the mysql service is lollipop)

    DOKKU_MYSQL_LOLLIPOP_NAME=/lollipop/DATABASE
    DOKKU_MYSQL_LOLLIPOP_PORT=tcp://172.17.0.1:3306
    DOKKU_MYSQL_LOLLIPOP_PORT_3306_TCP=tcp://172.17.0.1:3306
    DOKKU_MYSQL_LOLLIPOP_PORT_3306_TCP_PROTO=tcp
    DOKKU_MYSQL_LOLLIPOP_PORT_3306_TCP_PORT=3306
    DOKKU_MYSQL_LOLLIPOP_PORT_3306_TCP_ADDR=172.17.0.1

---

# MySQL support

environment variable will be set under app

>DATABASE_URL=mysql://mysql:SOME_PASSWORD@dokku-mysql-lollipop:3306/lollipop

connect to mysql:

>dokku mysql:connect lollipop

enter the docker container:
>dokku mysql:enter lollipop

run a command directly against the service
>dokku mysql:enter lollipop date

---

# customize the PORT
## customize 

<img src="https://cdn.jsdelivr.net/gh/xurenlu/bed/resource/87/87f296abda05173d232077edaf712ea4.png" width="800"/>

