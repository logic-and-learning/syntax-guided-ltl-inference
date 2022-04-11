===========
Django_SGLTLL
===========

Django_SGLTLL is a Django-powered web front-end for the `Syntax-Guided LTL-Learing Toolkit <https://doi.org/10.1145/3276501>`__.

============
Requirements
============

* `Django <https://www.djangoproject.com/>`__ (4.0.3+)
* `RQ <https://github.com/nvie/rq>`__ (0.13.0+), which builds upon `Redis <https://redis.io/>`__ (3.0.0+)
* `django-rq <https://github.com/rq/django-rq>`__ (2.5.1+)
* The `Syntax-Guided LTL-Learing Toolkit <https://github.com/horn-ice/hice-dt>`__ 
============
Installation
============

* Install ``django_SGLTLL``:

  .. code-block:: console

    pip install django-hice-1.0.tar.gz

  See the section on distribution for information on how to generate a source distribution.
  Alternatively, you may simply clone the repository into the root directory of your Django project.

* Add ``django_rq`` and ``django_SGLTLL`` to ``INSTALLED_APPS`` in your site's ``settings.py``:

  .. code-block:: python

    INSTALLED_APPS = [
        # other apps
        'django_rq',
        'django_SGLTLL',
    ]

* Configure (i.e., add) a queue with name ``SGLTLL`` in your site's ``settings.py``:

  .. code-block:: python

    RQ_QUEUES = {
        'SGLTLL': {
            'HOST': 'localhost',
            'PORT': 6379,
            'DB': 0,
            'DEFAULT_TIMEOUT': 60,
        },
    }

* Configure (i.e., add) ``django_SGLTLL``'s custom exception handler in your site's ``settings.py``:

  .. code-block:: python

    RQ_EXCEPTION_HANDLERS = [
        'django_SGLTLL.rq_exception_handler.job_exception_handler'
    ]

* Configure (i.e., add) the path to the syntax-guided-LTL-learing's binaries in your site's ``settings.py``:

  .. code-block:: python

    LEARNER_PATH = 'path to syntax-guided-LTL-learing'

* Include ``django_SGLTLL.urls`` in your site's ``urls.py``:

  .. code-block:: python

    # Django >= 2.0
    urlpatterns += [
        path('SGLTLL/', include('django_SGLTLL.urls'))
    ]
    
  You might need to import ``path`` and ``include`` from ``django.urls``.

=====
Usage
=====

1. Set up database for ``django_SGLTLL``:

  .. code-block:: console

    python ./manage.py makemigrations django_SGLTLL
    python ./manage.py migrate
    
2. Create one (or more) ``django-rq`` workers:

  .. code-block:: console

    python ./manage.py rqworker SGLTLL

3. Run a webserver:

  .. code-block:: console
  
    python ./manage.py runserver
  
  and visit http://127.0.0.1:8000/SGLTLL/.
  
  Remember to never use Django's development server in a production environment.

============
Distribution
============

``django_SGLTLL`` uses ``setuptools`` to allow a simple distribution.

To create a source distribution (which can by installed with ``pip``), use the following command:

  .. code-block:: console

    python setup.py sdist

Refer to the `documentation of setuptools <https://setuptools.readthedocs.io/en/latest/>`__ for more information.
