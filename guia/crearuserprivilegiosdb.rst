Cómo crear un usuario en MySQL/MariaDB y otorgar permisos en una base de datos específica
========================================================================================

Creación de la Database::

  mysql> CREATE DATABASE `mydb`;
  
Creación del Usuario::

  mysql> CREATE USER 'myuser'@localhost IDENTIFIED BY 'mypassword';

Otorgar permisos para acceder y usar el servidor MySQL
-------------------------------------------------------

Solo permita el acceso desde localhost (esta es la configuración más segura y común que usará para una aplicación web)::

  mysql> GRANT USAGE ON *.* TO 'myuser'@localhost IDENTIFIED BY 'mypassword';
  
Para permitir el acceso al servidor MySQL desde cualquier otra computadora en la red::

  mysql> GRANT USAGE ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword';
  
En MySQL 8 o superior no añadiremos la parte IDENTIFIED BY 'mypassword'


Otorgar todos los privilegios a un usuario en una base de datos específica
---------------------------------------------------------------------------

MySQL 5.7 y anteriores versiones::

  mysql> GRANT ALL privileges ON `mydb`.* TO 'myuser'@localhost IDENTIFIED BY 'mypassword';

MySQL 8 y versiones superiores::

  mysql> GRANT ALL ON `mydb`.* TO 'myuser'@localhost;
  
Aplicar los cambios realizados
---------------------------------

Para que sean efectivos los nuevos permisos asignados debes terminar con el siguiente comando::

  mysql> FLUSH PRIVILEGES;
  
Verifica que tu nuevo usuario tenga los permisos correctos::

  mysql> SHOW GRANTS FOR 'myuser'@localhost;     
  +--------------------------------------------------------------+ 
  | Grants for myuser@localhost                                  | 
  +--------------------------------------------------------------+ 
  | GRANT USAGE ON *.* TO 'myuser'@'localhost'                   | 
  | GRANT ALL PRIVILEGES ON `mydb`.* TO 'myuser'@'localhost'     | 
  +--------------------------------------------------------------+ 
  2 rows in set (0,00 sec)
  
Si te has equivocado en algún momento puedes deshacer todos los pasos anteriores ejecutando los siguientes comandos, teniendo la precaución de reemplazar localhost por '%' si también lo cambiaste en los comandos anteriores::

  DROP USER myuser@localhost;
  DROP DATABASE mydb;
  
Un script de Linux en Bash muy simple y pequeño que lo ayudará a hacer todo esto de una manera mucho más rápida y directa. Simplemente cambie sus nombres de usuario y base de datos, y eso es todo::

  #! /bin/bash

  newUser='testuser'
  newDbPassword='testpwd'
  newDb='testdb'
  host=localhost
  #host='%'

  # MySQL 5.7 and earlier versions 
  #commands="CREATE DATABASE \`${newDb}\`;CREATE USER '${newUser}'@'${host}' IDENTIFIED BY '${newDbPassword}';GRANT USAGE ON *.* TO '${newUser}'@'${host}' IDENTIFIED BY '${newDbPassword}';GRANT ALL privileges ON \`${newDb}\`.*
  TO '${newUser}'@'${host}' IDENTIFIED BY '${newDbPassword}';FLUSH PRIVILEGES;"

  # MySQL 8 and higher versions
  commands="CREATE DATABASE \`${newDb}\`;CREATE USER '${newUser}'@'${host}' IDENTIFIED BY '${newDbPassword}';GRANT USAGE ON *.* TO '${newUser}'@'${host}';GRANT ALL ON \`${newDb}\`.* TO '${newUser}'@'${host}';FLUSH PRIVILEGES;"

  echo "${commands}" | /usr/bin/mysql -u root -p

