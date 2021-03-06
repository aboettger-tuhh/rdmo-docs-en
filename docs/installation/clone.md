# Obtaining the app directory

The next step is to create the `rdmo-app` directory by cloning the corresponding repository into the home directory of you `rdmo` user:

```bash
git clone https://github.com/rdmorganiser/rdmo-app
```

Note that this is not the main `rdmo` repository, only the configuration files. Inside this directory, you will find:

* a `config` directory, containing the main settings of your RDMO installation,
* a `requirements` directory, containing shortcuts to install the different mandatory and optional dependencies, and
* a `manage.py` script, which is the main way to interact with your RDMO installation on the command line. Most of the following steps will use this script.

The `rdmo-app` directory corresponds to a [project](https://docs.djangoproject.com/en/stable/intro/tutorial01) in Django terms.
