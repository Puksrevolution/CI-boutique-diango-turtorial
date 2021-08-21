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
- mkdir templates
- mkdir templates/allauth
- git add .
- git commit -m "setup allauth"
- git push

(didn't work
cp -r ../.pip-modules/lib/python3.7/site-packages/allauth/templates/* ./templates/allauth/

try
cp -r ../.pip-modules/lib/python3.8/site-packages/allauth/templates/* ./templates/allauth/
)

- python3 manage.py startapp home
- mkdir -p home/templates/home
- create index.html in home/templates/home folder
- home/views.py
```
def index(request):
    """ A view to return the index page """

    return render(request, 'home/index.html')

```

- copy urls.py and paste in home folder
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='home')
]

```

- update urls.py in boutique folder
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('allauth.urls')),
    path('', include('home.urls')),
]

```

- update settings.py
```
import os


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
    'allauth.socialaccount',
    'home',
]



TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            os.path.join(BASE_DIR, 'templates'),
            os.path.join(BASE_DIR, 'templates', 'allauth'),
        ],
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
```
- python3 manage.py runserver
- git add . 
- git commit -m " added home app and templates"
- git push


- costumize home/templates/home/index.html
- git add .
- git commit -m "added home page content"
- git push


- update templates/base.html
- git add .
- git commit -m "added main page header"
- git push


- mkdir static
- mkdir media
- mkdir static/css
- create base.css file in static/css

- add your own fontawesome.kit.js to templates/base.html
- settings.py
```
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/3.0/howto/static-files/

STATIC_URL = '/static/'
STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)

MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```
- boutique/urls.py
```
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('allauth.urls')),
    path('', include('home.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```
- git add .
- git commit -m "completed home page header and css"
- git push

- update templates/base.html
- mkdir templates/includes
- create templates/includes/main-nav.html
- create templates/includes/mobile-top-header.html
- update base.css
```
.bg-black {
    background: #000 !important;
}
```
- git add .
- git commit -m "added mobile_header and main navbar"
- git push

- add images for products into media folder
- python3 manage.py startapp products
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
    'allauth.socialaccount',
    'home',
    'products',
]
```
- mkdir products/fixtures
- copy & paste categories.jason and products.jason from [Kaggle](https://www.kaggle.com/)
- use [Json formatter](https://jsonformatter.org/) to formatt the .json code from [Kaggle](https://www.kaggle.com/)
- open products/models.py
```
from django.db import models


class Category(models.Model):
    name = models.CharField(max_length=254)
    friendly_name = models.CharField(max_length=254, null=True, blank=True)

    def __str__(self):
        return self.name

    def get_friendly_name(self):
        return self.friendly_name


class Product(models.Model):
    category = models.ForeignKey('Category', null=True, blank=True, on_delete=models.SET_NULL)
    sku = models.CharField(max_length=254, null=True, blank=True)
    name = models.CharField(max_length=254)
    description = models.TextField()
    price = models.DecimalField(max_digits=6, decimal_places=2)
    rating = models.DecimalField(max_digits=6, decimal_places=2, null=True, blank=True)
    image_url = models.URLField(max_length=1024, null=True, blank=True)
    image = models.ImageField(null=True, blank=True)

    def __str__(self):
        return self.name

```
- python3 manage.py makemigrations --dry-run
- pip3 install pillow

- python3 manage.py makemigrations
- python3 manage.py migrate --plan
- python3 manage.py migrate

- products/admin.py
```
from django.contrib import admin
from .models import Product, Category


# Register your models here.
admin.site.register(Product)
admin.site.register(Category)
```
- python3 manage.py loaddata categories
- python3 manage.py loaddata products
- python3 manage.py runserver
  - open admin in 8000 browser
    - check if all products loaded properly
- git add .
- git commit -m "added products app, models and fixtures"
- git push


- update database, fixing typos
  - products/models.py
```
class Category(models.Model):

    class Meta:
        verbose_name_plural = 'Categories'
        
    name = models.CharField(max_length=254)
    friendly_name = models.CharField(max_length=254, null=True, blank=True)

    def __str__(self):
        return self.name

    def get_friendly_name(self):
        return self.friendly_name
```
  - products/admin.py
```
from django.contrib import admin
from .models import Product, Category


class ProductAdmin(admin.ModelAdmin):
    list_display = (
        'sku',
        'name',
        'category',
        'price',
        'rating',
        'image',
    )

    ordering = ('sku',)


class CategoryAdmin(admin.ModelAdmin):
    list_display = (
        'friendly_name',
        'name',
    )

admin.site.register(Product, ProductAdmin)
admin.site.register(Category, CategoryAdmin)

```
- check admin account database
- git add .
- git commit -m "customized admin"
- git push


- products/views.py
```
from django.shortcuts import render
from .models import Product


def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()

    context = {
        'products': products,
    }

    return render(request, 'products/products.html', context)

```
- create urls.py in folder products
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.all_products, name='products')
]

```

- boutique/urls.py
```
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('allauth.urls')),
    path('', include('home.urls')),
    path('products/', include('products.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

```
- mkdir -p products/templates/products
- create products.html
- git add .
- git commit -m "added products views and templates"
- git push




- python3 manage.py runserver









































e-commerce web application
complete with product search and filter functionality.
A live fully functional payment system.
A full-featured authentication system including email confirmations and user profiles.
And real-time notifications that guide the user's experience.
To do this we'll use technologies that are popularly used in modern software development
Such as Stripe, Amazon Web Services, Heroku, and more.
This project is not designed to demonstrate general concepts.
Or leave you with something that looks good but doesn't do much.
By the time you've completed it you'll have everything you need to solve real-world
problems as well as build elegant web applications that not only look good and function well.
But even have the potential to become sustainable revenue-generating busines