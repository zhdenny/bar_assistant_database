

# ¿Qué es esto?
BarAssistant/SaltRim es una aplicación self-hosted para una base de datos de cócteles. No me gustaba la base de datos de cócteles predeterminada con la que viene la aplicación, ya que utiliza ingredientes demasiado ambiguos o vagos (como Ron "Oscuro", "Dorado" y "Blanco"). Por lo tanto, realicé una revisión completa de las Categorías de Ingredientes, los Ingredientes y los Cócteles. Se prestó especial atención al Ron, ya que es la categoría de destilado más amplia tanto en producción como en sabor. Utilizo imágenes de alta resolución y proporciono un poco de historia para cada cóctel junto con "etiquetas". A continuación se muestran algunas capturas de pantalla de ejemplo, junto con las instrucciones sobre cómo importar mi base de datos personalizada a tu propia instancia self-hosted de Bar Assistant / SaltRim.

![image](https://github.com/user-attachments/assets/8f63837d-0624-4324-a935-e9b848544def)

![palmetto-1](https://github.com/user-attachments/assets/83b45de3-b944-4502-9af3-5e4bca1866b1)



# bar_assistant_database

Respaldo de base de datos de Bar Assistant personal
https://github.com/karlomikus/vue-salt-rim

La base de datos contenida en esta carpeta de Dropbox se actualizará diariamente
https://www.dropbox.com/sh/micduo5h6fhqp61/AACJTwLBrvPo95JG5exeeNNYa?dl=0

Si realizas una importación en una instancia existente, se sobrescribirán los datos relacionados con cócteles, ingredientes, vasos, imágenes, etc. Básicamente, todos los datos que se puedan considerar compartibles, por lo que, por ejemplo, los datos del usuario no se exportarán/importarán.

Para importar la base de datos, ya debes tener una instancia de bar-assistant funcionando. También debe estar en la versión más reciente de Bar-Assistant. Luego, sigue estos pasos:
1. Descarga el archivo zip de la base de datos desde el enlace de Dropbox anterior. ASEGÚRATE de que estás descargando el archivo zip y NO la carpeta completa en Dropbox. Dropbox muestra dos botones de descarga: uno es para la carpeta y otro para un archivo individual dentro de la carpeta.
2. Coloca el zip de la base de datos en el directorio `appdata` de bar-assistant. DEBE colocarse únicamente en este directorio. La importación NO funcionará si colocas el zip de la base de datos en una subcarpeta. Asegúrate de que los permisos del archivo zip de la base de datos coincidan con los de `appdata` de bar-assistant (debería ser 33:33 a partir de la V4 de BarAssistant).
3. Con bar-assistant aún ejecutándose, abre una terminal (shell) en el contenedor de bar-assistant. Esto creará una shell en tu contenedor de Bar Assistant. Reemplaza "bar-assistant-container-name" con el nombre de tu contenedor de Bar Assistant
```
docker exec -it bar-assistant-container-name /bin/sh
```

Aquí hay algunos comandos útiles que puedes ejecutar desde dentro del contenedor de Bar-Assistant. Podrías necesitarlos más adelante
```
sqlite3 storage/bar-assistant/database.ba3.sqlite 'SELECT * FROM users;'  <<----Command lists Users and their User IDs
sqlite3 storage/bar-assistant/database.ba3.sqlite 'SELECT * FROM bars;'   <<----Command lists Bars and their Bar IDs
```

Esto iniciará la importación de la base de datos. Reemplaza "name_of_file.zip" con el nombre del archivo zip de la base de datos. NOMBRE COMPLETO DEL ARCHIVO ÚNICAMENTE, SIN RUTAS.
```
php artisan bar:import-recipes name_of_file.zip
```

4. Se te pedirá que ingreses el ID del Bar al que deseas importar o déjalo vacío para crear uno nuevo. El ID del Bar se encuentra en la URL de cualquier cóctel en el Bar. Si esta es la PRIMERA vez que importas mi base de datos, te recomiendo encarecidamente crear un bar nuevo en lugar de sobrescribir uno existente. Esto puede evitar errores potenciales en el proceso de importación.
   Por ejemplo `https://my.bar.com/cocktails/glowing-embers-1` tiene un ID de bar de "1" (el número al final de la URL). Si importas en un Bar existente, REEMPLAZARÁ (no agregará) todos los datos en ese Bar.
   Si dejas que cree un bar nuevo, se te pedirá que proporciones un nombre nuevo para el bar. Luego se te pedirá que asignes el bar a tu ID de usuario.
5. Luego habrá una ventana de confirmación y procederá con la importación.
6. Sal. Ejecuta `docker compose down/up` para reiniciar todos los contenedores.
7. Listo.

# Solución de problemas
A veces los cócteles/ingredientes no muestran los ingredientes o la búsqueda no funciona correctamente. Esto suele ser un problema con meilisearch. Para solucionarlo, debes optimizar el bar y actualizar la búsqueda.
1. Ve a bars, haz clic en editar bar y luego haz clic en `Optimize bar`
2. `docker compose exec bar-assistant php artisan bar:refresh-search`

# Ayuda de instalación
Consulta siempre primero el wiki oficial para obtener ayuda.
https://bar-assistant.github.io/docs/

Incluí mi archivo `docker compose`, mi archivo `.env` y mi archivo `nginx.proxy` solo como referencia al 2023-12-11.
