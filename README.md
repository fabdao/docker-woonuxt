# docker-woonuxt

Welcome to this humble repo to facilitate Woonuxt development environment. Feel free to comment or correct. 
Due to the choice of Overlay2 filesysem for docker... should be working only on linux and maybe on mac.

# Step 0 : Visit (WooNuxt official website)[https://woonuxt.com]

# Step 1 : Clone and set up repo

> Clone repo 🥷

`git clone git@github.com:fabdao/docker-woonuxt.git`

> Navigate 🚣

`cd docker-woonuxt`

> Fetch git submodule Woonuxt and Wordpres 🥷🍃🪵

`git submodule init`

`git submodule update`

# Step 2 : Initiate Wordpress DataBase without reverse proxy service

> Copy initial docker compose file for run ⚒️🐋

`cp docker/initial.docker-compose.yml docker-compose.yml`

> Copy .env file ☔⚠️🔫

`cp docker/.env.example .env`

> Try to run docker 🐳

`docker compose up`

## Command line helper

### Display :: docker :: container 🖵🐋🪺
> List ALL Running Containers `docker ps`

### Display :: docker :: container -all 🖵🐋🪹
> List ALL Running Containers (including STOPPED) `docker ps -a`

### Stop :: docker :: container 🪺🐋🪹
> Unmount Docker containers `docker compose down`

### Display :: docker :: logs 🖵🐋🥚
> Display Docker log´s `docker compose logs mysql-db`

### Display :: docker :: inspect 🖵🐋🍳
> Display Docker log´s `docker inspect mysql-db`

### System :: docker :: stop 🧑‍💻🐋❌
> Stop Docker `sudo systemctl stop docker`

### System :: docker :: start 🧑‍💻🐋✔️
> Start Docker `sudo systemctl stop start`

## Troubleshooting mySQL

### Error :: mysql :: healthcheck ❌💾🧑‍⚕️
![SQL error](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-error-mySQL.png)

### Fix :: mysql :: healthcheck ✔️💾🧑‍⚕️
> Update permissions for DB folder

`sudo chown -R 999:999 ./docker/db/data`

`sudo chmod -R 770 ./docker/db/data`

## Troubleshooting Docker's volumes

### Error :: filesystem :: overlay ❌📁🔁
![Volumes error](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-error-volumes-overlay2.png)

### Check if Docker is using the correct FileSystem

> Display docker's infos :

`sudo docker info | grep -i overlay`

> Wrong output :

![overlay output](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-infos-overlayFS.png)

> Correct output :

![overlay2 output](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-infos-overlay2.png)

### Fix :: filesystem :: overlay ✔️📁🔁

> Prérequiste : please take time to follow this guide (5min) : [docker's overlay official documentation](https://docs.docker.com/engine/storage/drivers/overlayfs-driver/) ☕

### Sum up for lazy devs from documentation above ☝️

> Stop Docker `sudo systemctl stop docker`

> Edit `/etc/docker/daemon.json`. If it doesn't yet exist, create it.

> Assuming that the file was empty, add the following contents : `{ "storage-driver": "overlay2" }`

> Start Docker `sudo systemctl stop start`

⚠️ Docker doesn't start if the `daemon.json` file contains invalid JSON. ⚠️

> Verify that the daemon is using the overlay2 storage driver `docker info`

> Use the docker info command and look for Storage Driver `Storage Driver: overlay2`

### 🙀🤙 Remember the use the right docker engine 🤙🙀​
### Change :: docker :: engine 🔁🐋🚂
![Chamge Docker engine](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-change-builder.png)
> In my case only `default` works...

## Set up Wordpress and required plugins

Open your browser to `http://localhost`, normally at this point you should be able to see this screen :

![Wordpress first account](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-creation-compte-admin.png)

## Install "Woonuxt Settings" plugin

### Download [Woonuxt Settings](https://woonuxt.com/) plugin 📦
> An outdated ZIP to upload is also located at `assets/woonuxt/woonuxt-settings.zip`

### Go to wordpress admin panel and install 👷
![Upload woonuxt settings](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-upload-woonuxt-settings.png)

## Troubleshooting manual upload

### Error : The uploaded file exceeds the upload_max_filesize ❌

![Upload error max size](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-uploaded-file-exceeds.png)

### Fix : Modify .htaccess ✔️

> Copy .htaccess from `src/wordpress/.htaccess` to `docker/wordpress/.htaccess` :

> Edit `docker/wordpress/.htaccess` uncommenting the bottom line `php_value upload_max_filesize 256M`

> Reset wordpress container folder permission `sudo chown -R www-data:www-data docker/wordpress`

> Verify permission inside wordpress container `docker exec -i wordpress-crm ls -l /var/www/html/.htaccess`

> Verify .htaccess inside wordpress container `docker exec -i wordpress-crm cat /var/www/html/.htaccess`

### Error : Unable the write the wordpress upload folder ❌

![Upload folder locked](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-permissions.png)

### Fix : Set Wordpress folder permissions ✔️

> Reset wordpress container folder permission `sudo chown -R www-data:www-data docker/wordpress`

## Install others required pluging ( Woocommerce, GraphQL and Headless-Login)

### Go the plugin section, and open settings from 'woonuxt-setting'

![other-plugin-install](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-pluging-install.png)

### Once installed :

> Add `http://localhost:3000` into Front End URL parameter and don´t forget to push SAVE Button at the end of this page...

### Import dummy sample products :

> Go to Tools > Import and install and run Wordpress importer (last one)!

![worpdress-importer](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-import-tools.png)

> Import `assets/sample-data/sample_products.xml`

![dummy-import](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-Import-dummies-product.png)

### Enable GraphQL public introspection

> Rendez-vous at GraphQL > Settings > WPGraphQL General Settings section and set like so...
![WP-GQL-intro](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/GraphQL-public-introspection.png)

### Set-up Headless Login pluging

> Rendez-vous at GraphQL > Settings > Headless section and set like so... ( maybe advanced options buttons should be toggle at the upper-right of the screen)

![HLS-providers](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-HLS-providers.png)
![WP-HLS-AccessCTRL](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-HLS-AccessCTRL.png)
![WP-HLS-Cookies](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/WP-HLS-Cookies.png)

# Step 3 : Start Woonuxt

> Edit `docker-compose.yml` : uncomment the woonuxt service to enable it

![DOCKER-woonuxt-disable](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-woonuxt-service-off.png)
![DOCKER-woonuxt-enable](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-woonuxt-service-on.png)

## Moment of truth

> Rebuild your stack : `docker compose up` 🤞

![DOCKER-success-with-woonuxt](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/DOCKER-success-with-woonuxt.png)

## Visit `http://localhost:3000`

![woonuxt-success-browser](https://github.com/fabdao/docker-woonuxt/blob/main/assets/img/woonuxt.png)
