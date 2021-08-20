- pip3 install django
- django-admin startproject boutique .
- touch .gitignore
```
*.sqlite3
*./pyc
```
- python3 manage.py runserver

- python3 manage.py migrate
- python3 manage.py createsuperuser
  - username: user
  - Email address: user@user.com
  - password: adminaccount
- git add .
- git commit -m "initial commit"
- git push


- pip3 install django-allauth==0.41.0
- https://django-allauth.readthedocs.io/en/latest/installation.html
- settings.py
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount'
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',  # required by allauth
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# from webpage copy & paste https://django-allauth.readthedocs.io/en/latest/installation.html
AUTHENTICATION_BACKENDS = [
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
]

SITE_ID = 1
```
- urls.py
```
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('allauth.urls'))
]
```
- python3 manage.py migrate
- python3 manage.py runserver
- open browser 8000
  - add /admin to url in browser
- log in
 - open sites
 - update Domain name: example.com -> boutique.example.com
 - update Display name: example.com -> Boutique
- log out
- strg + C / stopping the development server

- settings.py
```
SITE_ID = 1

EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'

ACCOUNT_AUTHENTICATION_METHOD = 'username_email'
ACCOUNT_EMAIL_REQUIRED = True
ACCOUNT_EMAIL_VERIFICATION = 'mandatory'
ACCOUNT_SIGNUP_EMAIL_ENTER_TWICE = True
ACCOUNT_USERNAME_MIN_LENGTH = 4
LOGIN_URL = '/accounts/login/'
LOGIN_REDIRECT_URL = '/success'

WSGI_APPLICATION = 'boutique.wsgi.application'

```
- python3 manage.py runserver
- open browser
  - add /accounts/login to url in browser
  - log in 
    - username: user  
    - password: adminaccount

- change url in browser to  /admin
  - log in
    - username: user  
    - password: adminaccount
  - open email address
    - add email address
    - select username
    - add email address
    - check checkboxs Verified, Primary
    - save
  - log out
- change url in browser to  /accounts/login
  - log in 
    - username: user  
    - password: adminaccount
      - 404 page displays, but with /success in url confirms that authentication is working properly

- settings.py
```
LOGIN_REDIRECT_URL = '/'
```
- pip3 freeze > requirements.txt







- python3 manage.py runserver
