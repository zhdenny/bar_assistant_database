# bar_assistant_database

database backup of personal Bar Assistant
https://github.com/karlomikus/vue-salt-rim

database contained in this dropbox folder will be updated daily
https://www.dropbox.com/sh/micduo5h6fhqp61/AACJTwLBrvPo95JG5exeeNNYa?dl=0

If you do an import on an existing instance, it will overwrite data related to cocktails, ingredients, glasses, images, etc. Basically all the data that can be considered shareable, so user data for example will not be exported/imported.

To import database you must already have an instance of bar-assistant in working order. It must be on the latest version of Bar-Assistant as well. Then follow these steps:
1. Download database zip from dropbox link above
2. Place database zip in your bar-assistant appdata folder. Be sure permissions for database zip file match your bar-assistant appdata (should be 1000:1000)
3. With bar-assistant still running, shell into the bar-assistant container and run the import command like below:
```
#This will create a shell into your Bar Assistant container.
#Replace "bar-assistant-container-name" with the name of your Bar Assistant container
docker exec -it bar-assistant-container-name /bin/sh

Here are a couple useful commands you can execute from inside the Bar-Assistant Container. You may need these later
sqlite3 storage/bar-assistant/database.ba3.sqlite 'SELECT * FROM users;'  <<----Command lists Users and their User IDs
sqlite3 storage/bar-assistant/database.ba3.sqlite 'SELECT * FROM bars;'   <<----Command lists Bars and their Bar IDs

#This will innitiate the database import
#Replace "name_of_file.zip" with the database zip filename
php artisan bar:import-recipes name_of_file.zip
```
4. You will be asked to enter the Bar ID of the bar you want to import to or leave empty to create a new one. The Bar ID is found in the URL for any cocktail in the Bar.
   For example `https://my.bar.com/cocktails/glowing-embers-1` has a bar ID of "1" (the number at the end of the URL). If you import into an existing Bar it will REPLACE (not add) all data in that Bar.
   If you let it create a new bar you will be prompted to provide a new bar name. Then you will be prompted to assign the bar to your user ID.
5. Then there will be a confirmation prompt and it will import after that.
6. Exit. Compose Down/Up to restart all containers.
7. Done


# Installation Help
Always look at the official wiki for help first.
https://bar-assistant.github.io/docs/

I included my docker compose, env file, and nginx.proxy file only for reference 2023-12-11
