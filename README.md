# docker-woonuxt

Welcome to this humble repo to facilitate Woonuxt develop environment. Feel free to comment or correct. Due to the choice of Overlay2 filesysem for docker... should be working only on linux and maybe on mac.

# Step 1 : Clone and set up repo

> Clone repo 🥷

`git clone git@github.com:fabdao/docker-woonuxt.git`

> Navigate 🚣

`cd docker-woonuxt`

> Fetch git submodule Woonuxt and Wordpres 🥷🍃🪵

`git submodule init`

`git submodule update`

# Step 2 : Initiate Wordpress DataBase without reverse proxy service

> Rename initial docker compose file for run ⚒️🐋

`cp docker/initial.docker-compose.yml docker-compose.yml`

> Prepare .env file ☔⚠️🔫

`cp docker/.env.example .env`

> Try to run docker 🐳

`docker compose up`

## Troubleshooting mySQL

### Error :: mysql :: healthcheck
![SQL error](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-error-mySQL.png)

### Fix :: mysql :: healthcheck
> Unmount docker containers

`docker compose down`

> Update permissions for DB folder ( we are still inside Docker folder)

`sudo chown -R 999:999 ./db/data`

`sudo chmod 775 ./db/data`

## Troubleshooting Docker's volumes

### Error :: filesystem :: overlay
![Volumes error](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-error-volumes-overlay2.png)

### Check if Docker is using the correct FileSystem :

> Display docker's infos : 

`sudo docker info | grep -i overlay`

> Wrong output :

![overlay output](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-infos-overlayFS.png)

> Correct output :

![overlay2 output](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-infos-overlay2.png)

### Fix :: filesystem :: overlay

> Prérequiste : please take time to check [docker's overlay official documentation](https://docs.docker.com/engine/storage/drivers/overlayfs-driver/) ☕
>
> 

## Set up Wordpress and required plugins

Open your browser to `http://localhost`, normally at this point you should be able to see this screen :

![Wordpress first account](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-creation-compte-admin.png)

## Install "Woonuxt Settings" plugin

Go to wordpress admin panel and reach the plugin section. From there you will be able to manually install the woonuxt-setting plugin. [ ZIP to upload is located at `assets/woonuxt/woonuxt-settings.zip` ]

![Upload woonuxt settings](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-upload-woonuxt-settings.png)

## Troubleshooting manual upload

### Error : The uploaded file exceeds the upload_max_filesize

![Upload error max size](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-uploaded-file-exceeds.png)

### Fix : Modify .htaccess

Locate .htaccess at `src/wordpress/.htaccess` and add at the bottom of the file :
> `php_value upload_max_filesize 256M`

### Error : Unable the write the wordpress upload folder

![Upload folder locked](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-permissions.png)

### Fix : Set Wordpress folder permissions

Locate the wordpress folder inside the docker one and try the change permission to www-data :

`sudo chown -R www-data:www-data wordpress`

## Install others required pluging ( Woocommerce, GraphQL and Headless-Login)

### Go the plugin section, and open settings from 'woonuxt-setting'

![other-plugin-install](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-pluging-install.png)

### Once installed :

> Add `http://localhost:3000` into Front End URL parameter and don´t forget to push SAVE Button at the end of this page...

### Import dummy sample products :

Install Wordpress importer and run it !

![worpdress-importer](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-import-tools.png)

Import `assets/sample-data/sample_products.xml`

![dummy-import](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-Import-dummies-product.png)

### Set-up Headless Login pluging

Rendez-vous for woonuxt settings section and set like so... ( maybe advances options buttons should be toggle at the upper-right of the screen)

![HLS-providers](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-HLS-providers.png)
![WP-HLS-Cookies](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-HLS-Cookies.png)
![WP-HLS-AccessCTRL](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-HLS-AccessCTRL.png)

# Step 3 : Start Woonuxt with reverse proxy service

> Unmount docker container

`docker compose down`

> Erase docker volumes

`docker volume rm $(docker volume ls -q)`

> Reset docker file without reverse proxy

`mv docker-compose.yml initial.docker-compose.yml`

> Enable docker file with reverse proxy

`mv regular.docker-compose.yml docker-compose.yml`

> Copy .env from docker/woonux to src/woonuxt

`cp ./docker/woonuxt/.env.example ./src/woonuxt/.env`

> modify GQL_HOST into .env

`GQL_HOST="http://wordpress-crm/graphql"`

## Moment of truth

> `docker compose up`

![DOCKER-success-with-woonuxt](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-success-with-woonuxt.png)

## Visit `http://localhost:3000`

![woonuxt-success-browser](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/woonuxt.png)
