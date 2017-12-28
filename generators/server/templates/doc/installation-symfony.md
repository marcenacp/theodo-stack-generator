## Installation

#### Create the vagrant
- Launch VM:
  - `vagrant up`
  - If you encounter error `ttyname failed: Inappropriate ioctl for devices`:
    - Update vagrant to the latest version from the website

- Add your ssh key in the vagrant:
  - `vagrant ssh`
  - `vim .ssh/authorized_keys` and copy-paste your public key.

- Launch the provisionning
  - `ansible-playbook devops/provisioning/playbook.yml -i devops/provisioning/hosts/vagrant`
  - If the command fails, run:
    - `ssh-keygen -R 10.0.0.10 && ssh ubuntu@10.0.0.10`
    - exit the vagrant


If you have a static frontend:
  - Connect to the vagrant: `vagrant ssh`

  - Build the frontend code: `cd /var/www/<%= appName %>/current/client && yarn build`

  - Symlink the frontend code in the web directory: `cd /var/www/<%= appName %>/current/web && ln -s ../client/build/ build`

  - Browse your static frontend: https://10.0.0.10


Change the driver to `pdo_pgsql`in `app/config.yml`:
```
# Doctrine Configuration
doctrine:
    dbal:
        driver:   pdo_pgsql
```

- Install the project

  - Connect to the vagrant as www-data:
    - `vagrant ssh`
    - `sudo su www-data`

  - Install dependencies
    - `cd /var/www/<%= appName %>/current && php composer.phar install`

  - Create the database schema
    - `cd /var/www/<%= appName %>/current && bin/console doctrine:schema:create`

  - Run migrations:
    - `cd /var/www/<%= appName %>/current && bin/console doctrine:generate:entities AppBundle`


  - Browse your API: https://10.0.0.10/app_dev.php

In the `app/config/routing.yml` add a prefix for your api, it can be somethings like that:

```
api:
    resource: '.'
    type:     'api_platform'
    prefix:   '/api'  # This line can be added
```
Then you api is available at https://10.0.0.10/app_dev.php/api


