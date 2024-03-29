---
date: "2018-10-15T17:38:25+07:00"
title: Django Quick Setup Guide with GitLab and Heroku
description: Create Django app and automatically build and deploy to Heroku
tags: [tutorial, coding, django, heroku, gitlab]
---

Heroku is a cloud platform for a developer to deploy their apps and express their idea and design straight to URL.
It offers multiple plans, but usually the free tier is more than enough for experimenting or personal use.

## Outline & Focus

- Create a virtual environment in Python
- Start a Django project
- Create a Django app and add it to your project
- Deploy your Django app to Heroku

### PART A &bull; GitLab and Heroku

1. create a [new git repository](https://gitlab.com/projects/new) and [new heroku app](https://dashboard.heroku.com/new-app)
2. go to your GitLab repository CI/CD settings and add these to your variables

    | Variable        | Value          |
    | --------------- | -------------- |
    | HEROKU_APIKEY   | [your_apikey]  |
    | HEROKU_APPNAME  | [your_appname] |
    | HEROKU_APP_HOST | [your_webapp]  |

3. initialize git in your project root directory

    ```bash
    git init
    ```

4. set remote origin to your GitLab repository

    ```bash
    git remote add origin https://gitlab.com/username/git-repo-name.git
    ```

5. set remote heroku to your Heroku app

    ```bash
    heroku git:remote -a heroku-appname
    ```

6. configure your `.gitlab-ci.yml` to activate GitLab pipelines
7. create a **deployment.sh** file

    ```bash
    #$ file: deployment.sh
    #!/bin/bash
    python manage.py makemigrations
    python manage.py migrate
    ```

8. create **Procfile** to specify executed commands by Heroku app

    ```
    #$ file: Procfile
    migrate: bash deployment.sh
    web: gunicorn your_project_name.wsgi
    ```

    *Note* - **Procfile** without an extension, is an essential file for your Heroku app and must be placed in the app's root directory to explicitly declare a **process type** from a variety you can choose from. For more information, visit [Heroku's article about Procfile](https://devcenter.heroku.com/articles/procfile)

9. get your [**.gitignore** file](https://github.com/github/gitignore/blob/main/Python.gitignore) before you commit anything

### PART B &bull; Python Virtual Environment

We use Virtual Environment to avoid filling our base Python installation with a bunch of libraries we might use for only one project. Some projects might need different versions of the same libraries too, you couldn't possibly install every version of each dependencies, remember what they're for, and hope to always avoid conflicts, right?

Another reason to use this is so that other people could recreate the exact environment for your project if you're going to share it, look for bugs, and all sorts of stuff.

1. Install Python (I recommend Python3)
2. If you've installed Python before, make sure you add it to your PATH
3. Install virtualenv using pip

    ```bash
    pip install virtualenv
    ```

4. Install virtualenv using pip

    ```bash
    pip install virtualenvwrapper-win
    ```

5. create the your virtual environment

    ```bash
    mkvirtualenv your-env-name
    ```

    *Note* - To activate your env, use workon your-env-name. To see your envs, use workon

6. Create a text file called **requirements** and copy all dependencies in the code block below

    ```
    #$ file: requirements.txt
    astroid==2.0.4
    autopep8==1.4.2
    certifi==2018.8.24
    chardet==3.0.4
    colorama==0.3.9
    coverage==4.4.1
    dj-database-url==0.4.2
    Django==2.1.1
    django-environ==0.4.4
    gunicorn==19.7.1
    idna==2.6
    isort==4.2.15
    lazy-object-proxy==1.3.1
    mccabe==0.6.1
    mock==2.0.0
    pbr==5.1.1
    psycopg2==2.7.5
    pycodestyle==2.4.0
    pylint==2.1.1
    pytz==2017.2
    requests==2.18.4
    six==1.10.0
    typed-ast==1.1.0
    urllib3==1.22
    whitenoise==3.3.0
    wrapt==1.10.11
    ```

7. Make sure you're working in your virtualenv and install the dependencies from `requirements.txt`

    ```bash
    pip install -r requirements.txt
    ```

    *Tip* - If you get an error saying, for example psycopg2 can't be installed, remove it from the text file, install from the text file again, and install psycopg2 manually with `pip install psycopg2` and then run `pip freeze > requirements.txt` to update the `requirements.txt`

### PART C &bull; Django Project

1. create new project using command

    ```bash
    django-admin startproject your_project_name
    ```

2. create new app using command inside your project directory

    ```bash
    django-admin startapp your_app_name
    ```

3. Add and modify these lines in your project's settings file

    ```python
    #$ file: project/settings.py
    import os
    import dj_database_url

    # Build paths inside the project like this: os.path.join(BASE_DIR, ...)

    BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
    PRODUCTION = os.environ.get('DATABASE_URL') is not None

    ALLOWED_HOSTS = ['*']

    INSTALLED_APPS = [
        ...
        'your_app_name',
    ]

    MIDDLEWARE = [
        ...
        'whitenoise.middleware.WhiteNoiseMiddleware',
    ]

    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [os.path.join(BASE_DIR, 'templates')],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]
    ```

      - 2 & 8 &rarr; for Production in Heroku
      - 12 &rarr; registering your app to the project
      - 17 &rarr; to use the WhiteNoiseMiddleware
      - 23 &rarr; to set the global template in your root directory

      ```python
      #$ file: project/settings.py#92
      # If Using Heroku Environment, then Use Database Setting on Heroku
      if PRODUCTION:
          DATABASES['default'] = dj_database_url.config()
      ```

      - Set Database to Heroku's

      ```python
      #$ file: project/settings.py#130
      PROJECT_ROOT = os.path.dirname(os.path.abspath(__file__))

      # Static files (CSS, JavaScript, Images)

      # https://docs.djangoproject.com/en/2.1/howto/static-files/

      STATICFILES_DIRS = [
          os.path.join(BASE_DIR, 'assets')
      ]

      STATIC_ROOT = os.path.dirname(os.path.abspath(__file__))

      STATIC_URL = '/static/'

      STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
      ```

      - 130 &rarr; Add the project root directory
      - 135-137 &rarr; Set your static files such as CSS, JS, and images inside the `assets` directory

4. Add the path to your app in your project's urls file

    ```python
    #$ file: project/urls.py
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('app_name.urls')),
        ...
    ]
    ```

    - 2 &rarr; Import include and path for urlpatterns
    - 6 &rarr; Direct path to include your app's urls file, which you're going to make

    *Note* - We are trying to build a scalable website and that is why we're giving the url to our app's urls file. If we instead give the paths to all of our templates into the main url file, it would get crowded quickly and become hard to maintain

5. modify these files in your apps

    ```python
    #$ file: project/urls.py
    from django.urls import path
    from .views import *

    urlpatterns = [
        path('', home, name='home'),
    ]
    ```

    ```python
    #$ file: project/views.py
    from django.shortcuts import render

    def called_name(request):
        return render(request, 'your_template.html')
    ```

6. create a `templates` directory inside your app directory and fill it with your html files
