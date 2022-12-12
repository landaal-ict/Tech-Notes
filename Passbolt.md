### Create admin user

docker-compose -f docker-compose-ce.yaml exec passbolt su -m -c "/usr/share/php/passbolt/bin/cake \
                                passbolt register_user \
                                -u <your_email> \
                                -f <first_name> \
                                -l <last_name>\
                                -r admin" -s /bin/sh www-data

### Backup options

