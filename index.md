# Wiki

- [Wiki](#wiki)
  - [NGINX on windows](#nginx-on-windows)
    - [Download](#download)
  - [PM2](#pm2)
    - [PM2 Installation](#pm2-installation)
    - [Basic commands](#basic-commands)
    - [Start on system boot](#start-on-system-boot)
  - [Ubuntu CouchDb setup](#ubuntu-couchdb-setup)
    - [CouchDb Installation](#couchdb-installation)
    - [Configuration](#configuration)
    - [Password reset](#password-reset)
    - [Set port and bind](#set-port-and-bind)
    - [Uninstall](#uninstall)
    - [CouchDB on Ubuntu](#couchdb-on-ubuntu)
    - [Service commands](#service-commands)
    - [CouchDb on Windows](#couchdb-on-windows)
    - [Starting and stopping couchdb server](#starting-and-stopping-couchdb-server)
  - [ufw](#ufw)
    - [Enable and Disable](#enable-and-disable)
    - [Allow](#allow)
    - [Deny](#deny)
    - [Delete Existing Rule](#delete-existing-rule)
    - [Services](#services)

## NGINX on windows

### Download

## [PM2](https://www.npmjs.com/package/pm2)

### PM2 Installation

`npm install pm2@latest -g`

### Basic commands

`pm2 --help`

### Start on system boot

To start pm2 on system boot up, install the npm package [pm2-windows-startup](https://www.npmjs.com/package/pm2-windows-startup).

## Ubuntu CouchDb setup

### CouchDb Installation

```curl -L https://couchdb.apache.org/repo/bintray-pubkey.asc | sudo apt-key add -```

```echo "deb https://apache.bintray.com/couchdb-deb focal main" | sudo tee -a /etc/apt/sources.list```

```sudo apt update```  
```sudo apt install couchdb```

### Configuration

```/opt/couchdb/etc/local.ini```

### Password reset

Delete admin user entries in admin section of:

/opt/couchdb/etc/local.d/10-admins.ini

**example:**

```batch
[admins]  
admin = -pbkdf2-960eac5550a333409bd9d4a5d599396fda9c5ca90075b8b4f02444f47dcf5c3f4e958aaf,10
```

Then restart the couchdb service. At this point, access to couchdb is not possible any more.

Edit:

```/opt/couchdb/etc/local.ini```

and add a admin username and password

```batch
admin = password
```

CouchDB will use this file to recreate the admin account at every restart. To stop this from happening simply comment out or delete the password entry.

On Windows, the config files are located in:

```C:\Program Files\Apache CouchDB\etc```

### Set port and bind

port = 5984  
bind = 0.0.0.0 (exposes externally)  

remember to open port through ufw  
(without server need to specify: http-> <http://ip_address:5984/_utils>)

### Uninstall

```sudo apt-get remove couchdb couchdb-bin couchdb-common -yf```  
```sudo apt purge couchdb```

### CouchDB on Ubuntu

### Service commands

```sudo systemctl restart couchdb```  
```sudo systemctl start couchdb```  
```sudo systemctl stop couchdb```

### CouchDb on Windows

### Starting and stopping couchdb server

As administrator:

C:\>net.exe start "apache couchdb"  
C:\>net.exe stop "apache couchdb"  

## ufw

From: [Ubuntu community help wiki](https://help.ubuntu.com/community/UFW)

### Enable and Disable

To turn UFW on with the default set of rules:

```sudo ufw enable```

Check the status (optionally verbose):

```sudo ufw status (verbose)```

Turn ufw off:

```sudo ufw disable```

### Allow

```sudo ufw allow <port>/<optional: protocol>```

example: To allow incoming tcp and udp packet on port 53:

```sudo ufw allow 53```

example: To allow incoming tcp packets on port 53:

```sudo ufw allow 53/tcp```

example: To allow incoming udp packets on port 53:

```sudo ufw allow 53/udp```

### Deny

```sudo ufw deny <port>/<optional: protocol>```

**example:** To deny tcp and udp packets on port 53:

```sudo ufw deny 53```

**example:** To deny incoming tcp packets on port 53:

```sudo ufw deny 53/tcp```

**example:** To deny incoming udp packets on port 53:

```sudo ufw deny 53/udp```

### Delete Existing Rule

To delete a rule, simply prefix the original rule with delete. For example, if the original rule was:

```ufw deny 80/tcp```

Use this to delete it:

```sudo ufw delete deny 80/tcp```

### Services

List of running services:

```less /etc/services```

Allow by Service Name:

```sudo ufw allow <service name>```

**example:** to allow ssh:

```sudo ufw allow ssh```
