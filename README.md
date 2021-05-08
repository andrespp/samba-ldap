andrespp/samba-ldap
===================

# Introduction

Compose file for a SAMBA server with LDAP authetication using [andrespp/samba-ldap](https://hub.docker.com/r/andrespp/samba-ldap) docker image.

This compose provides three containers:
* OpenLDAP Server
* SAMBA Server
* PhpLdapAdmin

# Quick start

* **Clone repository**
    * `$ git clone https://github.com/andrespp/samba-ldap.git`


* **Set compose and other config files**
    * `docker-compose.yml`
    * `config/samba/smb.conf`: Samba server configuration
    * `config/samba/smbldap.conf`: smbldap tool settings
    * `config/samba/smbldap_bind.conf`: smbldap tool settings
    * `config/samba/libnss-ldap.conf`: samba server host ldap authentication
    * **Note**: this step may be skiped if the default settings are enougth for you (maybe for testing purposes).


* **Compose up**
    * `docker-compose up -d`
    * **Note**:
        * OpenLDAP server may take a while to be up due to DH parameters generation.
        * This may cause samba container to crash, which can be fixed with `docker-compose restart samba`.


* **Populate LDAP Tree**
    * `$ docker exec -it samba smbldap-populate`


* **Set shared directories permissions**

```
$ sudo chown +0 data/samba/documents
$ sudo chgrp +513 data/samba/documents # Domain users
$ sudo chmod 750 data/samba/documents
```

```
$ sudo chown +0 data/samba/public
$ sudo chgrp +514 data/samba/public # Domain guests
$ sudo chmod 777 data/samba/public/
```


* **Create first user**
    * `$ docker exec -it samba smbldap-useradd -a -P -m username`


* **Test share access**
    * `$ docker exec -it samba smbclient -U username //samba/Documents`


# User management

* **Add a new user with home directory**
    * `docker exec -it samba smbldap-useradd -a -P -m username`


* **Remove a user**
    * `docker exec -it samba smbldap-userdel username`
    * `docker exec -it samba smbldap-userdel -r username` # remove files


* **Add a group**
    * `docker exec -it samba smbldap-groupadd -a groupname`


* **Make an existing user a member of a group**
    * `docker exec -it samba smbldap-groupmod -m username groupname`


* **Remove a user from a group**
    * `docker exec -it samba  smbldap-groupmod -x username groupname`

**Note**: these actions may also be performed using phpLdapAdmin tool on [http://localhost:8000](http://localhost:8000)

# Issues

If you have any problems with or questions about this image, please contact me
through a [GitHub issue](https://github.com/andrespp/docker-samba-ldap/issues).
