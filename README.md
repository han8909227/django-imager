
[![Build Status](https://travis-ci.org/marco-zangari/django-imager.svg?branch=master)](https://travis-ci.org/marco-zangari/django-imager)

**Author**: Han Bao and Marco Zangari

**Version**: 1.2.7

## Overview
An instagram-like web application build using [Django](https://www.djangoproject.com/).

## Routes
| Route | Name | Description |
|:--|--|:--|
|`/`|home|the landing page|
|`/login`|login|GET: the login form page<br>POST: logs a user into their account, {username, password}|
|`/logout`|logout|log out the currently logged in user|
|`/profile/<username>`|profile|profile file for given user|
|`/profile/edit`|profile_edit| edit the current user's profile|
|`/images/library`|library|library of all the logged in user's albums and photos<br>GET: album_page: page number for albums, photo_page: page number for photos|
|`/images/photos`|photo_gallery|gallery of all public photos|
|`/images/albums`|album_gallery|gallery of all public albums|
|`/images/photos/<id>`|photo_detail|detail of a single photo|
|`/images/albums/<id>`|album_detail|detail of a single album<br>GET: page: page number for photos in the album|
|`/images/photos/<id>/edit`|photo_edit|edit a single photo|
|`/images/albums/<id>/edit`|album_edit|edit a single album|
|`/images/photos/add`|photos_create|upload new pictures|
|`/images/albums/add`|photos_create|create new albums|
|`/api/v1/photos`|api_photo_list|list of all the photos for authenticated user as JSON|
|`/accounts/*`|all registration routes| included from [django-registration](http://django-registration.readthedocs.io/en/stable/index.html)|
|`/admin/*`|all built-in admin routes| included from [Django admin](https://docs.djangoproject.com/en/1.11/ref/contrib/admin/#reversing-admin-urls)|

## Getting Started

Clone this repository to your local machine.
```
$ git clone https://github.com/han8909227/django-imager.git
```

Once downloaded, change directory into the `django-imager` directory.
```
$ cd django-imager
```

Begin a new virtual environment with Python 3 and activate it.
```
django-imager $ python3 -m venv ENV
django-imager $ source ENV/bin/activate
```

Install the application requirements with [`pip`](https://pip.pypa.io/en/stable/installing/).
```
(ENV) django-imager $ pip install -r requirements.txt
```

Create a [Postgres](https://wiki.postgresql.org/wiki/Detailed_installation_guides) database for use with this application.
```
(ENV) django-imager $ createdb imagersite
```

Export environmental variables pointing to the location of database, your username, hashed password, and secret
```
(ENV) django-imager $ export SECRET_KEY='secret'
(ENV) django-imager $ export DB_NAME='imagersite'
(ENV) django-imager $ export DB_USER='(your postgresql username)'
(ENV) django-imager $ export DB_PASS='(your postgresql password)'
(ENV) django-imager $ export DB_HOST='localhost'
(ENV) django-imager $ export DEBUG='True'
```

Then initialize the database with the `migrate` command from `manage.py`
```
(ENV) django-imager $ python imagersite/manage.py migrate
```

Once the package is installed and the database is created, start the server with the `runserver` command from `manage.py`
```
(ENV) django-imager $ python imagersite/manage.py runserver
```

Application is served on http://localhost:8000

## Testing
You can test this application by first exporting an environmental variable pointing to the location of a testing database, then running the `test` command from `manage.py`.
```
(ENV) django-imager $ export TEST_DB='test_imagersite'
(ENV) django-imager $ python imagersite/manage.py test imagersite
```

## Deploying
You can deploy this application to [AWS](https://aws.amazon.com/) using [Ansible](https://www.ansible.com/).

If you do not already have Ansible installed, install it outside your environment with `pip`
```
(ENV) django-imager $ deactivate
django-imager $ pip install ansible
django-imager $ source ENV/bin/activate
```

Create a `hosts` file in the root of `django-imager`
```yml
[us-west-2]
(your EC2 public IP address)

[us-west-2:vars]
ansible_ssh_user=(your EC2 user)
ansible_ssh_private_key_file=/path/to/your/key.pem

server_dns=(your EC2 public DNS)
secret_key='secret'
db_name='(your RDS database name)'
db_host='(your RDS endpoint)'
db_user='(your RDS username)'
db_pass='(your RDS password)'
test_db='test_imagersite'
allowed_hosts='(your EC2 public DNS) (your EC2 public IP address)'

aws_storage_bucket_name='(your S3 bucket name)'
aws_access_key_id='(your IAM user access key id)'
aws_secret_access_key='(your IAM user secret access key)'

admin_email='(your email address for admin)'
admin_email_host='(host for your admin email)'
admin_email_pass='(password for your admin email)'
```

Deploy the application with `ansible-playbook`
```
(ENV) django-imager $ ansible-playbook -i hosts playbook/imager_playbook.yml
```

## Architecture
Built with Python and Django framework. Tested through Django testing suite.
