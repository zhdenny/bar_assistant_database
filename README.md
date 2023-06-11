# bar_assistant_database
database backup of personal Bar Assistant
https://github.com/karlomikus/vue-salt-rim

database contained in this dropbox folder will be updated weekly
https://www.dropbox.com/sh/micduo5h6fhqp61/AACJTwLBrvPo95JG5exeeNNYa?dl=0

If you do an import on an existing instance, it will overwrite data related to cocktails, ingredients, glasses, images, etc. Basically all the data that can be considered shareable, so user data for example will not be exported/imported.

To import database you must already have an instance of bar-assistant in working order. Then follow these steps:
1. Download database zip from dropbox link above
2. Place database zip in your bar-assistant appdata folder. Be sure permissions for database zip file match your bar-assistant appdata (should be 33:33)
3. With bar-assistant still running, shell into the bar-assistant container and run the import command like below:
```
#This will create a shell into your Bar Assistant container.
#Replace "bar-assistant-container-name" with the name of your Bar Assistant container
docker exec -it bar-assistant-container-name /bin/sh

#This is innitiate the database import
php artisan bar:import
```
4. You will be asked to confirm import. Confirm and wait for import to complete.
5. Done.
