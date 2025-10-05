# What is this?
BarAssistant/SaltRim is a Cocktail database self-hosted app. I did not like the default cocktail database the app comes pre-loaded with because it uses ingredients which were too ambiguous/vague (such as "Dark" "Gold" and "White" Rum). So I did a complete overhaul on the Ingredient Categories, Ingredients, and Cocktails. Special attention was taken to Rum as it is the most broad spirit category in both production and taste. I use high-resolution images and provide a bit of history to each cocktail along with "tags". Some example screenshots are below along with instructions on how to import my custom database into your own self-hosted instance of Bar Assistant / SaltRim.

![image](https://github.com/user-attachments/assets/8f63837d-0624-4324-a935-e9b848544def)

![palmetto-1](https://github.com/user-attachments/assets/83b45de3-b944-4502-9af3-5e4bca1866b1)



# bar_assistant_database

database backup of personal Bar Assistant
https://github.com/karlomikus/vue-salt-rim

database contained in this dropbox folder will be updated daily
https://www.dropbox.com/sh/micduo5h6fhqp61/AACJTwLBrvPo95JG5exeeNNYa?dl=0

If you do an import on an existing instance, it will overwrite data related to cocktails, ingredients, glasses, images, etc. Basically all the data that can be considered shareable, so user data for example will not be exported/imported.

To import database you must already have an instance of bar-assistant in working order. It must be on the latest version of Bar-Assistant as well. Then follow these steps:
1. Download database zip from dropbox link above. Be SURE you are downloading the zip file and NOT the entire Folder in Dropbox. Dropbox shows two download buttons - One is the folder and one is for an individual file in the folder.
2. Place database zip in your bar-assistant appdata directory. It MUST be placed only in this directory. Import will NOT work if you place the database zip in a subfolder. Be sure permissions for database zip file match your bar-assistant appdata (should be 33:33 as of V4 of BarAssistant)
3. With bar-assistant still running, shell into the bar-assistant container. This will create a shell into your Bar Assistant container. Replace "bar-assistant-container-name" with the name of your Bar Assistant container
```
docker exec -it bar-assistant-container-name /bin/sh
```

Here are a couple useful commands you can execute from inside the Bar-Assistant Container. You may need these later
```
sqlite3 storage/bar-assistant/database.ba3.sqlite 'SELECT * FROM users;'  <<----Command lists Users and their User IDs
sqlite3 storage/bar-assistant/database.ba3.sqlite 'SELECT * FROM bars;'   <<----Command lists Bars and their Bar IDs
```

This will innitiate the database import. Replace "name_of_file.zip" with the database zip filename. FULL FILENAME ONLY, WITHOUT PATHS.
```
php artisan bar:import-recipes name_of_file.zip
```

4. You will be asked to enter the Bar ID of the bar you want to import to or leave empty to create a new one. The Bar ID is found in the URL for any cocktail in the Bar. If this is the FIRST time importing my database, I highly recommend you create a new bar instead of overwriting an existing bar. This can avoid potential errors in the import process.
   For example `https://my.bar.com/cocktails/glowing-embers-1` has a bar ID of "1" (the number at the end of the URL). If you import into an existing Bar it will REPLACE (not add) all data in that Bar.
   If you let it create a new bar you will be prompted to provide a new bar name. Then you will be prompted to assign the bar to your user ID.
5. Then there will be a confirmation prompt and it will import after that.
6. Exit. Compose Down/Up to restart all containers.
7. Done

# Troubleshooting
Sometimes cocktails/ingredients do not show ingredients or search is not performing correctly. This is typically an issue with meilisearch. To fix it, you need to optimize bar and refresh search.
1. Go to bars, click edit bar, then click `Optimize bar`
2. `docker compose exec bar-assistant php artisan bar:refresh-search`

# Installation Help
Always look at the official wiki for help first.
https://bar-assistant.github.io/docs/

I included my docker compose, env file, and nginx.proxy file only for reference 2023-12-11
