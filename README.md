# docker-woonuxt
Local DEV env : Docker + Woonuxt

git clone git@github.com:fabdao/docker-woonuxt.git
cd docker-woonuxt

git submodule init

git submodule update


mv initial.docker-compose.yml docker-compose.yml
mv .env.example .env

ERROR
^DOCKER-error-mySQL.png
FIX

docker compose down
Inside Docker folder
sudo chown -R 999:999 ./db/data
sudo chmod 775 ./db/data

REPRISE .ENV dans docker
MYSQL_USER=sqlUser
WORDPRESS_DB_USER=sqlUser
doit etre similaire

GO to
http://localhost
^WP-creation-compte-admin.png

Add pluging :
^WP-upload-woonuxt-settings.png
folder assets/woonuxt/woonuxt-settings.zip

ERROR
The uploaded file exceeds the upload_max_filesize directive in php.ini.
^WP-uploaded-file-exceeds.png

Fix Modify src/wordpress/.htaccess
add bottom
php_value upload_max_filesize 256M

^WP-permissions.png
sudo chown -R www-data:www-data wordpress

Install pluging
^WP-pluging-install.png

Once installed :
^WP-plugins-installed.png
Add http://localhost:3000 into Front End URL parameter
Don t forget to push SAVE Buttons at the end of this page

Import dummy sample products :
^WP-import-tools.png

Install Wordpress importer and run it !
import assets/sample-data/sample_products.xml
WP-Import-dummies-product.png

Set UP Pluggin in
WP-HLS-prviders.png
WP-HLS-Cookies.png
WP-HLS-AccessCTRL.png


Time of thruth
docker compose down
docker volume rm $(docker volume ls -q)

mv docker-compose.yml initial.docker-compose.yml
mv regular.docker-compose.yml docker-compose.yml

cp woonuxt/.env.example ../src/woonuxt/.env

modify .ENV
GQL_HOST="http://wordpress-crm/graphql"

docker compose up



DOCKER-success-with-woonuxt.png
http://localhost:3000/

woonuxt.png
