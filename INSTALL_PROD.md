# Semillas del conocimiento - Instalación (PRODUCCIÓN)

## Requisitos mínimos
- PHP 8.0+ con PDO SQLite, fileinfo, mbstring, openssl.
- Servidor web (Apache/Nginx) con HTTPS configurado.
- Acceso al sistema de archivos para crear data/ y backups.

## Pasos
1) Subir carpeta 'semillas' al webroot o a la ruta deseada.
2) Asegurar que data/ y data/uploads/ existen y son escribibles por el usuario del servidor web:
   chown -R www-data:www-data semillas/data
   chmod -R 750 semillas/data
3) Proteger la carpeta data (ya incluye data/.htaccess). Si usas Nginx, asegúrate de denegar acceso a /data/*.
4) Edita config.php para ajustar BASE_URL y otras variables según tu entorno.
5) Ejecuta en el navegador: https://TU_DOMINIO/semillas/db_init.php — esto crea la BD y un usuario admin inicial.
6) Accede a https://TU_DOMINIO/semillas/admin.php y cambia la contraseña admin inmediatamente.
7) Forzar HTTPS y HSTS en el servidor (nginx/apache). Recomendado Let’s Encrypt para certificados.
8) Ajusta php.ini en producción:
   - display_errors = Off
   - log_errors = On
   - upload_max_filesize = 8M (o 6M)
   - post_max_size = 8M
9) Backups: crea un cron que copie data/database.sqlite a un backup fuera del servidor o a almacenamiento remoto cada hora/día.
10) Revisión de seguridad:
    - Revisar logs de acceso y errores.
    - Mantener sistema y PHP actualizados.
    - Considerar migrar a MySQL/Postgres si esperas alta concurrencia.

## Notas sobre límites y configuraciones por defecto
- DB: SQLite con WAL y busy timeout. Adecuado para tráfico bajo/medio.
- Uploads por defecto permitidos: image/png, image/jpeg, application/pdf, tamaño máximo 5 MB.
- CSS personalizado: se permite, pero se aplica un filtro básico para eliminar @import, url(), expression(), javascript: y reglas peligrosas. No es infalible: admin con privilegios puede inyectar UI dañina. Considera limitar CSS o revisar manualmente.