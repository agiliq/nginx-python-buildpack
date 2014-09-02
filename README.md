A better heroku python buildpack
--------------------------------

Heroku has a [Python buildpack](https://github.com/heroku/heroku-buildpack-python) but it is a suboptimal in some ways.

1. [Gunicorn directly behind heroku router is succeptible to a ddos attack.](http://blog.etianen.com/blog/2014/01/19/gunicorn-heroku-django/)
2. The recommended static file server is [static](https://pypi.python.org/pypi/static). Which is a bit of cop out. Something like nginx is much better suited for this.
3. In search for Heroku's no configuration deploys, it doesn't allow setting things like the location of manage.py explicitly. To find the manage.py the buildpack is doing "MANAGE_FILE=$(find . -maxdepth 3 -type f -name 'manage.py' | head -1)". This goes against "explicit is better tha implicit" and will mean that different manage.py will be found in different heroku deploys.

What this does
----------------

To workaround this

1. The buildpack installs nginx, and gunicorn is used behind nginx.
2. Static files are served via nginx
3. It takes a heroku.yml file which allows overriding the location of `manage.py` and `requirements.txt`

Some other benefits you get with this

1. Since you have nginx, you can take take advantage of nginx tooling. In particular, the bundled nginx comes with pagespeed enabled, which can make a signifcant impact to your forntend performance.
2. You can use nginx to do rewrites. (For example to force ssl only).
3. You can host things like sphinx docs in a folder, and serve it directly from nginx.

Inspirations
-------------

1. [Heroku buildpack multi](https://github.com/ddollar/heroku-buildpack-multi)
2. [Nginx buildpack](https://github.com/ryandotsmith/nginx-buildpack)

