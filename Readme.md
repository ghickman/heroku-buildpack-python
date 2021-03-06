Heroku buildpack: Python
========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps.
It uses [virtualenv](http://www.virtualenv.org/) and [pip](http://www.pip-installer.org/).

Usage
-----

Example usage:

    $ ls
    Procfile  requirements.txt  web.py

    $ heroku create --stack cedar --buildpack git@github.com:heroku/heroku-buildpack-python.git

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom build pack... done
    -----> Python app detected
    -----> Preparing virtualenv version 1.6.4
           New python executable in ./bin/python
           Installing setuptools............done.
           Installing pip...............done.
    -----> Installing dependencies using pip version 1.0.2
           Downloading/unpacking Flask==0.7.2 (from -r requirements.txt (line 1))
           Downloading/unpacking Werkzeug>=0.6.1 (from Flask==0.7.2->-r requirements.txt (line 1))
           Downloading/unpacking Jinja2>=2.4 (from Flask==0.7.2->-r requirements.txt (line 1))
           Installing collected packages: Flask, Werkzeug, Jinja2
           Successfully installed Flask Werkzeug Jinja2
           Cleaning up...

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root. It will detect your app as Python/Django if there is an additional `settings.py` in a project subdirectory.

It will use virtualenv and pip to install your dependencies, vendoring a copy of the Python runtime into your slug.  The `bin/`, `include/` and `lib/` directories will be cached between builds to allow for faster pip install time.

Hacking
-------

To use this buildpack, fork it on Github.  Push up changes to your fork, then create a test app with `--buildpack <your-github-url>` and push to it.

To change the vendored virtualenv, unpack the desired version to the `src/` folder, and update the virtualenv() function in `bin/compile` to prepend the virtualenv module directory to the path. The virtualenv release vendors its own versions of pip and setuptools.

Extras
------

This Buildpack contains some extra steps for adding extra binaries to your Heroku App VM which were based upon Heroku's method of setting up memcached for pylibmc.

### GeoDjango

Looks for `django.contrib.gis` in your `settings.py` and installs GDAL, GEOS & PROJ4.

Set these options in your `settings.py` to specify the locations of GDAL & GEOS:

    GDAL_LIBRARY_PATH = '/app/.heroku/gdal/lib/libgdal.so'
    GEOS_LIBRARY_PATH = '/app/.heroku/geos/lib/libgeos_c.so'

This portion of the buildpack is an updated version of https://github.com/cirlabs/heroku-buildpack-geodjango

### wkthmltopdf

Looks for `django-wkthmltopdf` in your `requirements.txt` and installs wkhtmltopdf.

Set this option in your `settings.py` to specify the location of wkthmltopdf:

    WKHTMLTOPDF_CMD = '/app/.heroku/wkhtmltopdf'

