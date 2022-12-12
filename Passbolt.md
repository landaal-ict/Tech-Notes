### Create admin user

docker-compose -f docker-compose-ce.yaml exec passbolt su -m -c "/usr/share/php/passbolt/bin/cake \
                                passbolt register_user \
                                -u <your_email> \
                                -f <first_name> \
                                -l <last_name>\
                                -r admin" -s /bin/sh www-data

### Backup options

Backup database container

   ```
docker exec -i database-container bash -c \
  'mysqldump -u${MYSQL_USER} -p${MYSQL_PASSWORD} ${MYSQL_DATABASE}' \
  > /path/to/backup.sql
```

### Backup server public and private keys

   ```
docker cp passbolt-container:/etc/passbolt/gpg/serverkey_private.asc \
    /path/to/backup/serverkey_private.asc
docker cp passbolt-container:/etc/passbolt/gpg/serverkey.asc \
    /path/to/backup/serverkey.asc
```

### Backup The avatars

   ```
docker exec -i passbolt-container \
    tar cvfzp - -C /usr/share/php/passbolt/ webroot/img/avatar \
    > passbolt-avatars.tar.gz
```

