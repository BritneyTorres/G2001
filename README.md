
# Instalación de Moodle con XAMPP y MariaDB

Este repositorio describe cómo instalar Moodle localmente utilizando XAMPP e incluye la actualización o uso de MariaDB como motor de base de datos.

## Requisitos Previos

- XAMPP (incluye Apache, PHP y MariaDB)
- Moodle (descargar desde [moodle.org](https://moodle.org))
- Acceso de administrador en el equipo

## Pasos de Instalación

### 1. Instalar XAMPP

- Descarga la última versión de XAMPP desde [apachefriends.org](https://www.apachefriends.org/es/index.html).
- Ejecuta el instalador y sigue los pasos. Asegúrate de instalar Apache, PHP y MariaDB.
- Abre el **Panel de Control de XAMPP** e inicia los servicios de Apache y MariaDB.

### 2. Descargar y Preparar Moodle

- Descarga Moodle desde el link del driveu
- Descomprime el archivo en `C:/xampp/htdocs/moodle` o en la carpeta deseada dentro de `htdocs`.
- Crea una carpeta `moodledata` fuera de `htdocs` y dale permisos de escritura.

### 3. Crear la Base de Datos con MariaDB

- Accede a `http://localhost/phpmyadmin/`.
- Crea una nueva base de datos (por ejemplo, `moodle`).
- (Opcional) Crea un usuario específico para la base de datos y otorga todos los privilegios necesarios.

### 4. Instalar Moodle

- Accede a `http://localhost/moodle` en tu navegador.
- Selecciona el idioma y continúa con la instalación.
- Cuando te pregunte por el tipo de base de datos, elige **MariaDB** o **MySQL**.
- Introduce los datos de conexión a la base de datos (nombre, usuario, contraseña, host = localhost).
- Sigue los pasos del instalador hasta completar la instalación.

### 5. Configuración y Problemas Comunes

- Si se presenta un error sobre el tipo de base de datos en el archivo `config.php`, cambia el parámetro `'dbtype' => 'mysqli'` a `'dbtype' => 'mariadb'`.
- Para problemas con extensiones de PHP (como `intl`), activa la extensión desde el archivo `php.ini` quitando el punto y coma `;` delante de `extension=intl` y reinicia Apache.

### 6. Actualización de MariaDB (si aplica)

- Haz copia de seguridad de tus bases de datos desde phpMyAdmin.
- Descarga la versión más reciente de MariaDB desde la web oficial.
- Detén el servicio de MariaDB en el panel de XAMPP.
- Sustituye la carpeta `mysql` apropiadamente según las instrucciones de la documentación de MariaDB y XAMPP.
- Restaura las bases de datos.

## Notas

- Comprueba la compatibilidad de la versión de PHP de XAMPP con la versión de Moodle.
- Realiza copias de seguridad antes de cualquier actualización mayor.
- El acceso inicial a Moodle es con el usuario administrador configurado durante la instalación.

---

Guía basada en la documentación oficial de Moodle y XAMPP

link de recursos: https://drive.google.com/drive/folders/1FBZBXdT439BxC5wO5Jz7eFYjgeNFXGF8?usp=drive_link