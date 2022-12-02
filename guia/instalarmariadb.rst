Instalar MariaDB
=====================

Configurar los repositorios EPEL
+++++++++++++++++++++++++++++++++++

Abrimos un terminal y añadimos las herramientas necesarias al sistema::

	$ dnf -y install epel-release yum-utils
  Configurar los repositorios para MariaDB
+++++++++++++++++++++++++++++++++++++++

Si te interesa, puedes añadir el repositorio para la última versión estable, MariaDB 10.6, o tal vez MariaDB 10.5. Para ello crearemos un nuevo archivo de repositorio, por ejemplo para la versión 10.6 (si te interesa otra versión, sustituye a continuación 10.6 por 10.x, según corresponda)::

	$ nano /etc/yum.repos.d/mariadb-10.6.repo

Y añadimos el siguiente contenido, cuidado es para CentOS, Almalinux RockyLinux 8::

	[mariadb]
	name = MariaDB
	baseurl = http://yum.mariadb.org/10.6/centos8-amd64
	gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
	gpgcheck=1

Guardamos los cambios y cerramos el archivo.

Actualización de los repositorios
Únicamente queda actualizar la información de los repositorios::

	$ dnf update -y
