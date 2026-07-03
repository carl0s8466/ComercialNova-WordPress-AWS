# Proceso de Instalación y Stack Tecnológico

* **Sistema Operativo:** Amazon Linux 2023.
* **Stack Web (LAMP):** Servidor Web Apache (httpd), PHP 8.2, PHP-FPM y el controlador de base de datos php-mysqli.
* **Automatización:** El aprovisionamiento se realizó mediante scripts de inicialización automatizada (User Data) que configuraron los servicios y los permisos del directorio `/var/www/html` a nivel de propietario `apache:apache` para asegurar la correcta ejecución del Core de WordPress.
