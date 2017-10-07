# glpi_sync
[Check_MK](https://mathias-kettner.de/check_mk.html) extension to synchronize information from [GLPI](http://www.glpi-project.org/).

### How it works
This extension makes a cron job that connects to the GLPI database, queries information on computers and synchronizes it with hosts' attributes in Check_MK. It can add new hosts to Check_MK. It can also run an inventory on new hosts.

### Cron job
Check_MK's internal cron is used to run this job. A user with admin role can also run this job manually.

### Synchronized attributes
* criticality, such as prod, uat or dev,
* physical or virtual computer,
* operating system.

### Requirements
* Check_MK 1.4.0,
* GLPI 0.84 or newer.
* libmysqlclient18

### How to install
#### Prepare new mysql user and allow remote connection to your GLPI DB
(todo: change bind address)
On your GLPI server run this mysql commands.
```mysql
GRANT SELECT ON glpidb.* TO glpi_sync@'%' IDENTIFIED BY 'super-password';
FLUSH PRIVILEGES;
```

#### GLPI
Computer State respective Status in GLPI needs to be one of: 'PROD', 'UAT' or 'DEV'
Computers need resolve via DNS.

#### Install requirements and plugin
I assume you have your cmk/omd/check-mk-raw instance working.
Install needed lib for connecting to your mysqldb/mariadb.
```bash
apt install libmysqlclient18
```

Download the release for your cmk version.
```bash
cmk -P install glpi_sync-1.2.mkp
```

Log in to your WATO and add GLPI sync snaping to the sidebar.
Edit configuration with your specific values.


create Host Tag groups os and type, and then add the Tag IDs you have in the plugins config, e.g.:
https://user-images.githubusercontent.com/8530934/31228878-a3cecf8e-a9e7-11e7-8290-682ebfe11f24.png
