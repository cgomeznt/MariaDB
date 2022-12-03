Instalar MariaDB
=====================

Configurar los repositorios EPEL
+++++++++++++++++++++++++++++++++++

Abrimos un terminal y añadimos las herramientas necesarias al sistema::

	$ dnf -y install epel-release yum-utils
	
Configurar los repositorios para MariaDB
+++++++++++++++++++++++++++++++++++++++

Verificar las versiones que esten en los repo::

	dnf module list mariadb
	Last metadata expiration check: 0:08:44 ago on Sat 03 Dec 2022 12:15:38 AM UTC.
	AlmaLinux 8 - AppStream
	Name                     Stream                    Profiles                                   Summary
	mariadb                  10.3 [d]                  client, galera, server [d]                 MariaDB Module
	mariadb                  10.5                      client, galera, server [d]                 MariaDB Module

	Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled


Si te interesa, puedes añadir el repositorio para la última versión estable, MariaDB 10.6, o tal vez MariaDB 10.5. Para ello crearemos un nuevo archivo de repositorio, por ejemplo para la versión 10.6 (si te interesa otra versión, sustituye a continuación 10.6 por 10.x, según corresponda)::

	$ nano /etc/yum.repos.d/mariadb-10.6.repo

Y añadimos el siguiente contenido, cuidado es para CentOS, Almalinux RockyLinux 8::

	[mariadb]
	name = MariaDB 10.6
	baseurl = http://yum.mariadb.org/10.6/centos8-amd64
	gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
	gpgcheck=1

Limpiamod los module de dnf::

	dnf -y module reset mariadb
	
Consultamos que el repo este disponible::

	dnf repolist
	repo id                                repo name
	appstream                              AlmaLinux 8 - AppStream
	baseos                                 AlmaLinux 8 - BaseOS
	epel                                   Extra Packages for Enterprise Linux 8 - x86_64
	extras                                 AlmaLinux 8 - Extras
	mariadb                                MariaDB 10.6
	remi-modular                           Remi's Modular repository for Enterprise Linux 8 - x86_64
	remi-safe                              Safe Remi's RPM repository for Enterprise Linux 8 - x86_64

Consulamos la info de mariadb::

	dnf info mariadb
	Last metadata expiration check: 0:10:50 ago on Sat 03 Dec 2022 12:15:38 AM UTC.
	Available Packages
	Name         : MariaDB
	Version      : 10.6.11
	Release      : 1.el8
	Architecture : src
	Size         : 89 M
	Source       : None
	Repository   : mariadb
	Summary      : MariaDB: a very fast and robust SQL database server
	License      : GPLv2
	Description  : MariaDB: a very fast and robust SQL database server
		     :
		     : It is GPL v2 licensed, which means you can use the it free of charge under the
		     : conditions of the GNU General Public License Version 2 (http://www.gnu.org/licenses/).
		     :
		     : MariaDB documentation can be found at https://mariadb.com/kb
		     : MariaDB bug reports should be submitted through https://jira.mariadb.org

	Name         : mariadb
	Epoch        : 3
	Version      : 10.3.35
	Release      : 1.module_el8.6.0+3265+230ed96b
	Architecture : x86_64
	Size         : 6.0 M
	Source       : mariadb-10.3.35-1.module_el8.6.0+3265+230ed96b.src.rpm
	Repository   : appstream
	Summary      : A very fast and robust SQL database server
	URL          : http://mariadb.org
	License      : GPLv2 with exceptions and LGPLv2 and BSD
	Description  : MariaDB is a community developed branch of MySQL - a multi-user, multi-threaded
		     : SQL database server. It is a client/server implementation consisting of
		     : a server daemon (mysqld) and many different client programs and libraries.
		     : The base package contains the standard MariaDB/MySQL client programs and
		     : generic MySQL files.



Actualización de los repositorios
Únicamente queda actualizar la información de los repositorios::

	$ dnf update -y
	
Instalamos mariadb::

	dnf install MariaDB-server MariaDB-client MariaDB-backup
