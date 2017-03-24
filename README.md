Role mysql
==========
[![MySQL](img/logo-mysql-170x115.png)](https://www.mysql.com/)

Installation and configuration of MySQL 5.

Requirements
------------

MySQL repositories must be configured in a HTTP server.

Role Variables
--------------

- mysql_version: Version of MySQL to be installed (e.g. '5.7')
    -  Available versions: 5.5 | 5.6 | 5.7
- mysql_root_password: Password for root user in MySQL (e.g. 'Tempor@l123')
- mysql_data_path: Path to store data files (e.g. '/mysql_data')
- mysql_data_size: Size of the filesystem for data files. If not defined it'll be a directory, not a LVM volume (e.g. '10G')
- mysql_backup_path: Path to store backup files (e.g. '/mysql_backup')
- mysql_backup_size: Size of the filesystem for backup files. If not defined it'll be a directory, not a LVM volume (e.g. '5G')
- mysql_port: Listen port used by MySQL. By default 3306 (e.g. '3306')
- mysql_datavg: Name of Volume Group used to create filesystems. Needed only if volumes are used (e.g. 'datosvg')
- mysql_disks: List of disks used as Physical Volumes for data VG. If VG doesn't exists it will use these disks, if exists, any new disks especified will be added to VG (e.g. '/dev/sdb,dev/sdc')
- mysql_db_list: List of databases to be created
- mysql_user_list: List of users to be created and their permissions
    - name: Name for the user (e.g. 'user1')
    - pass: Password for the user (e.g. 'Tempor@l123')
    - host: The 'host' part of the user (e.g. '%')
    - priv: Privileges in format db.table:priv1,priv2 (e.g. '\*.\*:ALL')
- mysql_repo_url: URL of the MySQL repository (e.g. 'http://10.10.10.101/mysql')

Example Playbook
----------------

Install MySQL with a 5G data filesystem and 10G backup filesystem in a new VG named datosvg in two new disks
```
mysql_root_password: 'Tempor@l123'
mysql_data_path: '/mysql_data'
mysql_data_size: '5G'
mysql_backup_path: '/mysql_backup'
mysql_backup_size: '10G'
mysql_datavg: 'datosvg'
mysql_disks: '/dev/sdb,dev/sdc'
```
Install MySQL without any filesystem, just using directories in free disk space
```
mysql_root_password: 'Tempor@l123'
mysql_data_path: '/mysql_data'
mysql_backup_path: '/mysql_backup'
```
Install MySQL with default data path (/var/lib/mysql) and without creation of backup directory. Also create 3 databases and 3 users. 
```
mysql_root_password: 'Tempor@l123'

mysql_db_list:
  - 'database1'
  - 'database2'
  - 'database3'

mysql_user_list:
  - name: 'user1'
    pass: 'Tempor@l123'
    host: '%'
    priv: 'database1.*:ALL'
  - name: 'user2'
    pass: 'Tempor@l456'
    host: 'localhost'
    priv: '*.*:USAGE'
  - name: 'user3'
    pass: 'Tempor@l678'
    host: '10.10.20.30'
    priv: 'database2.*:ALL,GRANT'
```
Author Information
------------------

IT_AUTOMATION
