# funkwhale_unraid
A modified docker compose and env file to simplify funkwhale depolyment on unraid with a reverse proxy using SWAG  
 - current ver: Funkwhale 1.4.0

# Installation

1. Open unraid terminal and run the following command. you will use the output as your Django secret key
   - openssl rand -base64 45
3. open the .env file in a text editor and configure the following variables
   - FUNKWHALE_API_IP
   - FUNKWHALE_API_PORT
   - FUNKWHALE_HOSTNAME
   - EMAIL_CONFIG (if desired)
   - ACCOUNT_EMAIL_VERIFICATION (if desired)
   - MEDIA_ROOT (music uploads from external users will go here)
   - DJANGO_SECRET_KEY (use the rand from step 1)
   - MUSIC_DIRECTORY_SERVE_PATH
4. open funkwhale.subdomain.conf and edit $upstream_app to point to your unraid server IP (NOT the name of the container)
5. create a funkwhale appdata folder and move docker-compose.yml and .env to it (/mnt/user/appdata/funkwhale)
6. In unraid terminal, navigate to the appdata folder (cd /mnt/user/appdata/funkwhale)
7. run "docker compose up -d postgres"
8. run "docker compose run --rm api funkwhale-manage migrate"
9. run "docker compose run --rm api funkwhale-manage fw users create --superuser" to create an admin account
10. run "docker compose up -d"
11. move funkwhale.subdomain.conf to your SWAG proxyconfs folder (/mnt/user/appdata/swag/nginx/proxyconfs)
12. restart the SWAG container

set up your cname on cloudflare/duckdns/etc and you should be able to navigate to your funkwhale instance and login with your superuser account (you will NOT be able to login if you navigate to [IP]:[PORT] )

note: the volume mappings will look incorrect but leave them as is. the docker-compose file does not need to be changed
