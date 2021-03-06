# General settings

A few general settings should be included in your `config/settings/local.py`. The first, and probably most important one, is if you run RDMO in [debug mode](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-DEBUG) or not:

```python
DEBUG = True
```

In debug mode, verbose error pages are shown in the case something goes wrong and static assets such as CSS and JavaScript files are found by the development server automatically. The debug mode **must not** be enabled when running RDMO in production connected to the internet.

Django needs a [secret key](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-SECRET_KEY), which "should be set to a unique, unpredictable value":

```python
SECRET_KEY = 'this is not a very secret key'
```

This key must be kept secret since otherwise many of Django’s security protections fail.

In production, Django only [allows requests to certain urls](https://docs.djangoproject.com/en/1.10/ref/settings/#allowed-hosts), which you need to specify:

```python
ALLOWED_HOSTS = ['localhost', 'rdmo.example.com']
```

If you want to run RDMO under an alias like `http://example.com/rdmo`, you need to set the base URL:

```python
BASE_URL = '/rdmo'
```

Furthermore, you might want to choose the main language for RDMO and the timezone:

```python
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'Europe/Berlin'
```

If you run RDMO behind a reverse Proxy, which terminates the TLS/SSL trafic, you need to add the following:

```
USE_X_FORWARDED_HOST = True
SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
```

in order for RDMO to pick up the `X-Forwarded-Host` and `X-Forwarded-Proto` HTTP headers from the proxy.
