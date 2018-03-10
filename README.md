Public People
=============

From the template
-------------------

Based on a template which makes it easy to build Django apps

On the server:

* easy to deploy on Heroku or Dokku
* uses [dj-database-url](https://crate.io/packages/dj-database-url/) for database URL injection
* uses [django-pipeline](https://django-pipeline.readthedocs.org/en/latest/) for asset compilation and fingerprinting
* uses [pyscss](http://pyscss.readthedocs.org/en/latest/) for compiling SCSS to CSS
* New Relic for monitoring
* Bower to install assets
* better debugging with ``python manage.py runserver_plus`` from [django-extensions](http://django-extensions.readthedocs.org/en/latest/)
* sane logging to stdout
* cookies for session storage

On the client:

* JQuery
* Google Analytics
* Bootstrap
* FontAwesome

Setting up
----------

```
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Development
-----------

* Put javascript into ``code4sa/static/javascript/app.js``
* Put SCSS stylesheets into ``code4sa/static/stylesheets/app.scss``
* Install new asset packs with Bower: ``bower install -Sp package-to-install``
* Get better debugging with ``python manage.py runserver_plus``

Production deployment
---------------------

Production deployment is based on running the app on dokku

### Intitial deployment

```bash
dokku apps:create publicpeople
dokku config:set DATABASE_URL=postgresql://.../publicpeople \
                 DJANGO_DEBUG=false \
                 DISABLE_COLLECTSTATIC=1 \
                 DJANGO_SECRET_KEY=some-secret-key \
                 NEW_RELIC_APP_NAME=cool app name \
                 NEW_RELIC_LICENSE_KEY=new relic license key
```

After pushing to the dokku git remote:

```
dokku run publicpeople python manage.py migrate
dokku run publicpeople python manage.py createsuperuser
```

### Regular change deployment

```
git push dokku master
```

Updating popolo data
--------------------

```
dokku run publicpeople python app/manage.py popolo_sources_update https://www.pa.org.za/media_root/popolo_json/pombola.json
```

License
-------

MIT License

Attribution
-----------

- Repo structure based on https://github.com/openupsa/django-template
- REST API based on https://github.com/openpolis/popolorest/blob/master/popolorest/views.py