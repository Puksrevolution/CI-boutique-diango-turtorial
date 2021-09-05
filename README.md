# Table of contents

+ [Stripe Part 1](#stripe-part-1)
+ [Stripe Part 2](#stripe-part-2)
+ [Stripe Part 3](#stripe-part-3)
+ [Stripe Part 4](#stripe-part-4)
+ [Stripe Part 5](#stripe-part-5)
+ [Stripe Part 6](#stripe-part-6)
+ [Stripe Part 7](#stripe-part-7)
+ [Stripe Part 8](#stripe-part-8)
+ [Stripe Part 9](#stripe-part-9)
+ [Stripe Part 10](#stripe-part-10)
+ [Stripe Part 11](#stripe-part-11)
+ [Stripe Part 12](#stripe-part-12)
+ [Stripe Part 13](#stripe-part-13)
+ [Stripe Part 14](#stripe-part-14)
+ [Stripe Part 15](#stripe-part-15)
+ [Stripe Part 16](#stripe-part-16)
+ [Stripe Part 17](#stripe-part-17)
+ [Profile App - Part 1](#profile-app-part-1)
+ [Profile App - Part 2](#profile-app-part-1)
+ [Profile App - Part 3](#profile-app-part-1)
+ [Profile App - Part 4](#profile-app-part-1)
+ [Profile App - Part 5](#profile-app-part-1)
+ [Profile App - Part 6](#profile-app-part-1)
+ [Profile App - Part 7](#profile-app-part-1)
+ [Profile App - Part 8](#profile-app-part-1)
+ [Profile App - Part 9](#profile-app-part-1)
+ [Profile App - Part 10](#profile-app-part-1)
+ [Product Form](#profile-app-part-1)
+ [Add Product View](#profile-app-part-1)
+ [Finishing the Add Product Functionality](#profile-app-part-1)
+ [Editing Products](#profile-app-part-1)
+ [Deleting Products](#profile-app-part-1)
+ [Securing the Views](#profile-app-part-1)
+ [Fixing The Image Field - Part 1](#profile-app-part-1)
+ [Fixing The Image Field - Part 2](#profile-app-part-1)
+ [Creating a Heroku App](#profile-app-part-1)


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


- costumize products.html
- git add .
- git commit -m "started product template"
- git push


- update index.html / home page button link
```
<a href="{% url 'products' %}" class="shop-now-button btn btn-lg rounded-0 text-uppercase py-3">Shop Now</a>
```
- update templates/includes/main-nav.html nav link
```
<a href="{% url 'products' %}" class="dropdown-item">All Products</a>
```
- update products/views.py
```
from django.shortcuts import render, get_object_or_404


def product_detail(request, product_id):
    """ A view to show individual product details """

    product = get_object_or_404(Product, pk=product_id)

    context = {
        'product': product,
    }

    return render(request, 'products/product_detail.html', context)
```
- update products/urls.py
```
urlpatterns = [
    path('', views.all_products, name='products'),
    path('<product_id>', views.product_detail, name='product_detail'),
]
```
- create product_detail.html
- update products.html
```
{% if product.image %}
    <a href="{% url 'product_detail' product.id %}">
        <img class="card-img-top img-fluid" src="{{ product.image.url }}" alt="{{ product.name }}">
    </a>
{% else %}
    <a href="{% url 'product_detail' product.id %}">
        <img class="card-img-top img-fluid" src="{{ MEDIA_URL }}noimage.png" alt="{{ product.name }}">
    </a>
{% endif %}
```
- update base.html
```
/* pad the top a bit when navbar is collapsed on mobile */
@media (max-width: 991px) {
    .header-container {
        padding-top: 116px;
    }

    body {
        height: calc(100vh - 116px);
    }
}
```
- git add . 
- git commit - m "added product details functionality"
- git push


- update templates/base.html
```
<form method="GET" action="{% url 'products' %}">
```
- update templates/includes/mobile-top-header.html
```
<form class="form" method="GET" action="{% url 'products' %}">
```
- update products/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.db.models import Q
from .models import Product


def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None

    if request.GET:
        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    context = {
        'products': products,
        'search_term': query,
    }

    return render(request, 'products/products.html', context)

```
- git add . 
- git commit - m "added search functionality"
- git push


- update templates/includes/main-nav.html nav link
```
<li class="nav-item dropdown">
    <a class="logo-font font-weight-bold nav-link text-black mr-5" href="#" id="clothing-link" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
        Clothing
    </a>
    <div class="dropdown-menu border-0" aria-labelledby="clothing-link">
        <a href="{% url 'products' %}?category=activewear,essentials" class="dropdown-item">Activewear &amp; Essentials</a>
        <a href="{% url 'products' %}?category=jeans" class="dropdown-item">Jeans</a>
        <a href="{% url 'products' %}?category=shirts" class="dropdown-item">Shirts</a>
        <a href="{% url 'products' %}?category=activewear,essentials,jeans,shirts" class="dropdown-item">All Clothing</a>
    </div>
</li>

<li class="nav-item dropdown">
    <a class="logo-font font-weight-bold nav-link text-black mr-5" href="#" id="homeware-link" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
        Homeware
    </a>
    <div class="dropdown-menu border-0" aria-labelledby="homeware-link">
        <a href="{% url 'products' %}?category=bed_bath" class="dropdown-item">Bed &amp; Bath</a>
        <a href="{% url 'products' %}?category=kitchen_dining" class="dropdown-item">Kitchen &amp; Dining</a>
        <a href="{% url 'products' %}?category=bed_bath,kitchen_dining" class="dropdown-item">All Homeware</a>
    </div>
</li>

<li class="nav-item dropdown">
    <a class="logo-font font-weight-bold nav-link text-black" href="#" id="specials-link" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
        Special Offers
    </a>
    <div class="dropdown-menu border-0" aria-labelledby="specials-link">
        <a href="{% url 'products' %}?category=new_arrivals" class="dropdown-item">New Arrivals</a>
        <a href="{% url 'products' %}?category=deals" class="dropdown-item">Deals</a>
        <a href="{% url 'products' %}?category=clearance" class="dropdown-item">Clearance</a>
        <a href="{% url 'products' %}?category=new_arrivals,deals,clearance" class="dropdown-item">All Specials</a>
    </div>
</li>
```
- update products/views.py
```
from .models import Product, Category


def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None

    if request.GET:
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
    }

    return render(request, 'products/products.html', context)

```
- git add . 
- git commit - m "added category filtering"
- git push


- update templates/includes/main-nav.html nav link
```
<div class="dropdown-menu border-0" aria-labelledby="all-products-link">
    <a href="{% url 'products' %}?sort=price&direction=asc" class="dropdown-item">By Price</a>
    <a href="{% url 'products' %}?sort=rating&direction=desc" class="dropdown-item ">By Rating</a>
    <a href="{% url 'products' %}?sort=category&direction=asc" class="dropdown-item ">By Category</a>
    <a href="{% url 'products' %}" class="dropdown-item">All Products</a>
</div>
```
- update products/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.db.models import Q
from django.db.models.functions import Lower

from .models import Product, Category


def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None
    sort = None
    direction = None

    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))

            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)
            
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    current_sorting = f'{sort}_{direction}'

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
        'current_sorting': current_sorting,
    }

    return render(request, 'products/products.html', context)
```
- git add . 
- git commit -m "added sorting"
- git push


- update products/temlates/products.html
- update products/temlates/product_detail.html
- products/views.py
```
def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None
    sort = None
    direction = None

    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))
            if sortkey == 'category':
                sortkey = 'category__name'
            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)

        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request,
                               ("You didn't enter any search criteria!"))
                return redirect(reverse('products'))

            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    current_sorting = f'{sort}_{direction}'

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
        'current_sorting': current_sorting,
    }

    return render(request, 'products/products.html', context)
```
- git add . 
- git commit -m "added sorting and product counts to product page"
- git push


- products/templates/products.html
```
    <div class="btt-button shadow-sm rounded-0 border border-black">
        <a class="btt-link d-flex h-100">
            <i class="fas fa-arrow-up text-black mx-auto my-auto"></i>
        </a>	
    </div>
{% endblock %}

{% block postloadjs %}
    {{ block.super }}
    <script type="text/javascript">
		$('.btt-link').click(function(e) {
			window.scrollTo(0,0)
		})
	</script>
    
    <script type="text/javascript">
        $('#sort-selector').change(function() {
            var selector = $(this);
            var currentUrl = new URL(window.location);

            var selectedVal = selector.val();
            if(selectedVal != "reset"){
                var sort = selectedVal.split("_")[0];
                var direction = selectedVal.split("_")[1];

                currentUrl.searchParams.set("sort", sort);
                currentUrl.searchParams.set("direction", direction);

                window.location.replace(currentUrl);
            } else {
                currentUrl.searchParams.delete("sort");
                currentUrl.searchParams.delete("direction");

                window.location.replace(currentUrl);
            }
        })
    </script>
{% endblock %}
```
- static/css/base.css
```
a.category-badge > span.badge:hover {
    background: #212529 !important;
    color: #fff !important;
}

.btt-button {
    height: 42px;
    width: 42px;
    position: fixed;
    bottom: 10px;
    right: 10px;
}

.btt-link {
    cursor: pointer;
}
```
- git add . 
- git commit -m "Added sorting JS and back to top link"
- git push


- python3 manage.py startapp bag
- bag/view.py
```
from django.shortcuts import render


def view_bag(request):
    """ A view that renders the bag contents page """

    return render(request, 'bag/bag.html')

```
- create bag/templates/bag/bag.html
- create bag/urls.py
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.view_bag, name='view_bag')
]

```
- update boutique/urls.py
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
    path('bag/', include('bag.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

```
- update templates/base.html
```
<a class="{% if grand_total %}text-info font-weight-bold{% else %}text-black{% endif %} nav-link" href="{% url 'view_bag' %}">
```
- update templates/includes/mobile-top-header.html
```
<a class="{% if grand_total %}text-primary font-weight-bold{% else %}text-black{% endif %} nav-link d-block d-lg-none" href="{% url 'view_bag' %}">
```
- boutique/settings.py
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
    'bag',
]
```
- git add . 
- git commit -m "Added shopping bag app, urls and template"
- git push


- creat bag/contexts.py
```
from decimal import Decimal
from django.conf import settings
from django.shortcuts import get_object_or_404
from products.models import Product


def bag_contents(request):

    bag_items = []
    total = 0
    product_count = 0
    bag = request.session.get('bag', {})

    for item_id, item_data in bag.items():
        if isinstance(item_data, int):
            product = get_object_or_404(Product, pk=item_id)
            total += item_data * product.price
            product_count += item_data
            bag_items.append({
                'item_id': item_id,
                'quantity': item_data,
                'product': product,
            })
        else:
            product = get_object_or_404(Product, pk=item_id)
            for size, quantity in item_data['items_by_size'].items():
                total += quantity * product.price
                product_count += quantity
                bag_items.append({
                    'item_id': item_id,
                    'quantity': quantity,
                    'product': product,
                    'size': size,
                })

    if total < settings.FREE_DELIVERY_THRESHOLD:
        delivery = total * Decimal(settings.STANDARD_DELIVERY_PERCENTAGE / 100)
        free_delivery_delta = settings.FREE_DELIVERY_THRESHOLD - total
    else:
        delivery = 0
        free_delivery_delta = 0

    grand_total = delivery + total

    context = {
        'bag_items': bag_items,
        'total': total,
        'product_count': product_count,
        'delivery': delivery,
        'free_delivery_delta': free_delivery_delta,
        'free_delivery_threshold': settings.FREE_DELIVERY_THRESHOLD,
        'grand_total': grand_total,
    }

    return context

```
- boutique/settings.py
```
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
                'django.template.context_processors.request', # required by allauth
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'bag.contexts.bag_contents',
            ],
        },
    },
]


# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/3.0/howto/static-files/

STATIC_URL = '/static/'
STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)

MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

FREE_DELIVERY_THRESHOLD = 50
STANDARD_DELIVERY_PERCENTAGE = 10
```
- templates/base.html
```
<h4 class="logo-font my-1">Free delivery on orders over ${{ free_delivery_threshold }}!</h4>
```
- git add . 
- git commit -m "Added bag context and delivery logic"
- git push


- bag/views.py
```
from django.shortcuts import render, redirect


def add_to_bag(request, item_id):
    """ Add a quantity of the specified product to the shopping bag """

    quantity = int(request.POST.get('quantity'))
    redirect_url = request.POST.get('redirect_url')
    bag = request.session.get('bag', {})

    if item_id in list(bag.keys()):
        bag[item_id] += quantity
    else:
        bag[item_id] = quantity

    request.session['bag'] = bag
    print(request.session['bag'])
    return redirect(redirect_url)

```
- bag/urls.py
```
urlpatterns = [
    path('', views.view_bag, name='view_bag'),
    path('add/<item_id>/', views.add_to_bag, name='add_to_bag'),
]
```
- products/templates/product_detail.html
```
<form class="form" action="{% url 'add_to_bag' product.id %}" method="POST">
    {% csrf_token %}
    <div class="form-row">
        <div class="col-12">
            <p class="mt-3"><strong>Quantity:</strong></p>
            <div class="form-group w-50">
                <div class="input-group">
                    <input class="form-control qty_input" type="number" name="quantity" value="1" min="1" max="99" data-item_id="{{ product.id }}" id="id_qty_{{ product.id }}">
                </div>
            </div>
        </div>

        <div class="col-12">
            <a href="{% url 'products' %}" class="btn btn-outline-black rounded-0 mt-5">
                <span class="icon">
                    <i class="fas fa-chevron-left"></i>
                </span>
                <span class="text-uppercase">Keep Shopping</span>
            </a>
            <input type="submit" class="btn btn-black rounded-0 text-uppercase mt-5" value="Add to Bag">
        </div>
        <input type="hidden" name="redirect_url" value="{{ request.path }}">
    </div>
</form>
```
- static/css/base.css
```
.btn-outline-black {
    background: white;
    color: black !important; /* use important to override link colors for <a> elements */
    border: 1px solid black;
}

.btn-outline-black:hover,
.btn-outline-black:active,
.btn-outline-black:focus {
    background: black;
    color: white !important;
}
```
- git add . 
- git commit -m "added add to bag functionality"
- git push


- update bag/templates/bag/bag.html
- git add . 
- git commit -m "updated shopping bag template"
- git push


- update products/models.py
```
class Product(models.Model):
    category = models.ForeignKey('Category', null=True, blank=True, on_delete=models.SET_NULL)
    sku = models.CharField(max_length=254, null=True, blank=True)
    name = models.CharField(max_length=254)
    description = models.TextField()
    has_sizes = models.BooleanField(default=False, null=True, blank=True)
    price = models.DecimalField(max_digits=6, decimal_places=2)
    rating = models.DecimalField(max_digits=6, decimal_places=2, null=True, blank=True)
    image_url = models.URLField(max_length=1024, null=True, blank=True)
    image = models.ImageField(null=True, blank=True)
```
- python3 manage.py makemigrations --dry-run
- python3 manage.py makemigrations
- python3 manage.py migrate --plan
- python3 manage.py migrate
- python3 manage.py shell
  In Terminal
    - from products.models import Product
    - kdbb = ['kitchen_dining', 'bed_bath']
    - clothes = Product.objects.exclude(category__name__in=kdbb)
    - clothes.count()
    - for item in clothes:
        item.has_sizes = True
        item.save()
    - Product.objects.filter(has_sizes=True)
    - Product.objects.filter(has_sizes=True).count()
    - exit()

- update products/product_detail.html
- update bag/templates/bag/bag.html
- git add . 
- git commit -m "added sizes to product model and size selctor box to product template"
- git push


- open admin account in browser
  - products list
  - sort by name
  - set Has sizes: No
- update bag/views.py
```
def add_to_bag(request, item_id):
    """ Add a quantity of the specified product to the shopping bag """

    quantity = int(request.POST.get('quantity'))
    redirect_url = request.POST.get('redirect_url')
    size = None
    if 'product_size' in request.POST:
        size = request.POST['product_size']
    bag = request.session.get('bag', {})

    if size:
        if item_id in list(bag.keys()):
            if size in bag[item_id]['items_by_size'].keys():
                bag[item_id]['items_by_size'][size] += quantity
            else:
                bag[item_id]['items_by_size'][size] = quantity
        else:
            bag[item_id] = {'items_by_size': {size: quantity}}
    else:
        if item_id in list(bag.keys()):
            bag[item_id] += quantity
        else:
            bag[item_id] = quantity

    request.session['bag'] = bag
    return redirect(redirect_url)
```
- update bag/contexts.py
```
def bag_contents(request):

    bag_items = []
    total = 0
    product_count = 0
    bag = request.session.get('bag', {})

    for item_id, item_data in bag.items():
        if isinstance(item_data, int):
            product = get_object_or_404(Product, pk=item_id)
            total += item_data * product.price
            product_count += item_data
            bag_items.append({
                'item_id': item_id,
                'quantity': item_data,
                'product': product,
            })
        else:
            product = get_object_or_404(Product, pk=item_id)
            for size, quantity in item_data['items_by_size'].items():
                total += quantity * product.price
                product_count += quantity
                bag_items.append({
                    'item_id': item_id,
                    'quantity': item_data,
                    'product': product,
                    'size': size,
                })

    if total < settings.FREE_DELIVERY_THRESHOLD:
        delivery = total * Decimal(settings.STANDARD_DELIVERY_PERCENTAGE / 100)
        free_delivery_delta = settings.FREE_DELIVERY_THRESHOLD - total
    else:
        delivery = 0
        free_delivery_delta = 0
    
    grand_total = delivery + total
    
    context = {
        'bag_items': bag_items,
        'total': total,
        'product_count': product_count,
        'delivery': delivery,
        'free_delivery_delta': free_delivery_delta,
        'free_delivery_threshold': settings.FREE_DELIVERY_THRESHOLD,
        'grand_total': grand_total,
    }

    return context

```
- for testing the code for shopping bag bag/templates/bag/bag.html
```
<div class="row">
    <div class="col">
        {% if bag_items %}
            {{ bag_items }}
            <br>
            <br>
            {{ request.session.bag }}
            <div class="table-responsive rounded">
                <table class="table table-sm table-borderless">
                    <thead class="text-black">
                        <tr>
                            <th scope="col">Product Info</th>
                            <th scope="col"></th>
                            <th scope="col">Price</th>
                            <th scope="col">Qty</th>
                            <th scope="col">Subtotal</th>
                        </tr>
                    </thead>
```
- git add . 
- git commit -m "Finished product size logic"
- git push


- update products/templates/products/product_detail.html
- create products/templates/products/includes/quantity_input_script.html
- git add . 
- git commit -m "Added quantity input +/- buttons"
- git push


- bag/contexts.py
```
def bag_contents(request):

    bag_items = []
    total = 0
    product_count = 0
    bag = request.session.get('bag', {})

    for item_id, item_data in bag.items():
        if isinstance(item_data, int):
            product = get_object_or_404(Product, pk=item_id)
            total += item_data * product.price
            product_count += item_data
            bag_items.append({
                'item_id': item_id,
                'quantity': item_data,
                'product': product,
            })
        else:
            product = get_object_or_404(Product, pk=item_id)
            for size, quantity in item_data['items_by_size'].items():
                total += quantity * product.price
                product_count += quantity
                bag_items.append({
                    'item_id': item_id,
                    'quantity': quantity,
                    'product': product,
                    'size': size,
                })

    if total < settings.FREE_DELIVERY_THRESHOLD:
        delivery = total * Decimal(settings.STANDARD_DELIVERY_PERCENTAGE / 100)
        free_delivery_delta = settings.FREE_DELIVERY_THRESHOLD - total
    else:
        delivery = 0
        free_delivery_delta = 0
    
    grand_total = delivery + total
    
    context = {
        'bag_items': bag_items,
        'total': total,
        'product_count': product_count,
        'delivery': delivery,
        'free_delivery_delta': free_delivery_delta,
        'free_delivery_threshold': settings.FREE_DELIVERY_THRESHOLD,
        'grand_total': grand_total,
    }

    return context

```
- update bag/templates/bag/bag.html
- update static/css/base.css
- git add . 
- git commit -m "Added quantity boxes to shopping bag page"
- git push


- update bag/views.py
```
from django.shortcuts import render, redirect, reverse, HttpResponse


def view_bag(request):
    """ A view that renders the bag contents page """

    return render(request, 'bag/bag.html')


def add_to_bag(request, item_id):
    """ Add a quantity of the specified product to the shopping bag """

    quantity = int(request.POST.get('quantity'))
    redirect_url = request.POST.get('redirect_url')
    size = None
    if 'product_size' in request.POST:
        size = request.POST['product_size']
    bag = request.session.get('bag', {})

    if size:
        if item_id in list(bag.keys()):
            if size in bag[item_id]['items_by_size'].keys():
                bag[item_id]['items_by_size'][size] += quantity
            else:
                bag[item_id]['items_by_size'][size] = quantity
        else:
            bag[item_id] = {'items_by_size': {size: quantity}}
    else:
        if item_id in list(bag.keys()):
            bag[item_id] += quantity
        else:
            bag[item_id] = quantity

    request.session['bag'] = bag
    return redirect(redirect_url)


def adjust_bag(request, item_id):
    """Adjust the quantity of the specified product to the specified amount"""

    quantity = int(request.POST.get('quantity'))
    size = None
    if 'product_size' in request.POST:
        size = request.POST['product_size']
    bag = request.session.get('bag', {})

    if size:
        if quantity > 0:
            bag[item_id]['items_by_size'][size] = quantity
        else:
            del bag[item_id]['items_by_size'][size]
            if not bag[item_id]['items_by_size']:
                bag.pop(item_id)
    else:
        if quantity > 0:
            bag[item_id] = quantity
        else:
            bag.pop(item_id)

    request.session['bag'] = bag
    return redirect(reverse('view_bag'))


def remove_from_bag(request, item_id):
    """Remove the item from the shopping bag"""

    try:
        size = None
        if 'product_size' in request.POST:
            size = request.POST['product_size']
        bag = request.session.get('bag', {})

        if size:
            del bag[item_id]['items_by_size'][size]
            if not bag[item_id]['items_by_size']:
                bag.pop(item_id)
        else:
            bag.pop(item_id)

        request.session['bag'] = bag
        return HttpResponse(status=200)

    except Exception as e:
        return HttpResponse(status=500)

```
- update bag/urls.py
```
urlpatterns = [
    path('', views.view_bag, name='view_bag'),
    path('add/<item_id>/', views.add_to_bag, name='add_to_bag'),
    path('adjust/<item_id>/', views.adjust_bag, name='adjust_bag'),
    path('remove/<item_id>/', views.remove_from_bag, name='remove_from_bag'),
]
```
- update bag/templates/bag/bag.html
- update templates/base.html
- create bag/templatetags/bag_tools.py
```
from django import template


register = template.Library()

@register.filter(name='calc_subtotal')
def calc_subtotal(price, quantity):
    return price * quantity

```
- git add . 
- git commit -m "Added adjust bag and remove from bag views, and calc subtotal template filter"
- git push


- create templates/includes/toasts/toast_error.html
- create templates/includes/toasts/toast_info.html
- create templates/includes/toasts/toast_success.html
- create templates/includes/toasts/toast_warning.html
- update templates/base.html
- update bag/views.py
```
from django.shortcuts import render, redirect, reverse, HttpResponse
from django.contrib import messages

from products.models import Product


def view_bag(request):
    """ A view that renders the bag contents page """

    return render(request, 'bag/bag.html')


def add_to_bag(request, item_id):
    """ Add a quantity of the specified product to the shopping bag """

    product = Product.objects.get(pk=item_id)
    quantity = int(request.POST.get('quantity'))
    redirect_url = request.POST.get('redirect_url')
    size = None
    if 'product_size' in request.POST:
        size = request.POST['product_size']
    bag = request.session.get('bag', {})

    if size:
        if item_id in list(bag.keys()):
            if size in bag[item_id]['items_by_size'].keys():
                bag[item_id]['items_by_size'][size] += quantity
            else:
                bag[item_id]['items_by_size'][size] = quantity
        else:
            bag[item_id] = {'items_by_size': {size: quantity}}
    else:
        if item_id in list(bag.keys()):
            bag[item_id] += quantity
        else:
            bag[item_id] = quantity
            messages.success(request, f'Added {product.name} to your bag')

    request.session['bag'] = bag
    return redirect(redirect_url)


def adjust_bag(request, item_id):
    """Adjust the quantity of the specified product to the specified amount"""

    quantity = int(request.POST.get('quantity'))
    size = None
    if 'product_size' in request.POST:
        size = request.POST['product_size']
    bag = request.session.get('bag', {})

    if size:
        if quantity > 0:
            bag[item_id]['items_by_size'][size] = quantity
        else:
            del bag[item_id]['items_by_size'][size]
            if not bag[item_id]['items_by_size']:
                bag.pop(item_id)
    else:
        if quantity > 0:
            bag[item_id] = quantity
        else:
            bag.pop(item_id)

    request.session['bag'] = bag
    return redirect(reverse('view_bag'))


def remove_from_bag(request, item_id):
    """Remove the item from the shopping bag"""

    try:
        size = None
        if 'product_size' in request.POST:
            size = request.POST['product_size']
        bag = request.session.get('bag', {})

        if size:
            del bag[item_id]['items_by_size'][size]
            if not bag[item_id]['items_by_size']:
                bag.pop(item_id)
        else:
            bag.pop(item_id)

        request.session['bag'] = bag
        return HttpResponse(status=200)

    except Exception as e:
        return HttpResponse(status=500)

```
- update boutique/settings.py
```
MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'
```
- update static/css/base.css
- git add . 
- git commit -m "Added toasts"
- git push


- update bag/views.py
- update static/css/base.css
- update templates/includes/toasts/toast_success.html
- git add . 
- git commit -m "Added notification Css, shopping bag preview, and additional messages"
- git push


- python3 manage.py startapp checkout
- update boutique/settings.py
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
    'bag',
    'checkout',
]
```
- checkout/models.py
```
import uuid

from django.db import models
from django.db.models import Sum
from django.conf import settings

from products.models import Product


class Order(models.Model):
    order_number = models.CharField(max_length=32, null=False, editable=False)
    full_name = models.CharField(max_length=50, null=False, blank=False)
    email = models.EmailField(max_length=254, null=False, blank=False)
    phone_number = models.CharField(max_length=20, null=False, blank=False)
    country = models.CharField(max_length=40, null=False, blank=False)
    postcode = models.CharField(max_length=20, null=True, blank=True)
    town_or_city = models.CharField(max_length=40, null=False, blank=False)
    street_address1 = models.CharField(max_length=80, null=False, blank=False)
    street_address2 = models.CharField(max_length=80, null=True, blank=True)
    county = models.CharField(max_length=80, null=True, blank=True)
    date = models.DateTimeField(auto_now_add=True)
    delivery_cost = models.DecimalField(max_digits=6, decimal_places=2, null=False, default=0)
    order_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)
    grand_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)

    def _generate_order_number(self):
        """
        Generate a random, unique order number using UUID
        """
        return uuid.uuid4().hex.upper()

    def update_total(self):
        """
        Update grand total each time a line item is added,
        accounting for delivery costs.
        """
        self.order_total = self.lineitems.aggregate(Sum('lineitem_total'))['lineitem_total__sum']
        if self.order_total < settings.FREE_DELIVERY_THRESHOLD:
            self.delivery_cost = self.order_total * settings.STANDARD_DELIVERY_PERCENTAGE / 100
        else:
            self.delivery_cost = 0
        self.grand_total = self.order_total + self.delivery_cost
        self.save()

    def save(self, *args, **kwargs):
        """
        Override the original save method to set the order number
        if it hasn't been set already.
        """
        if not self.order_number:
            self.order_number = self._generate_order_number()
        super().save(*args, **kwargs)

    def __str__(self):
        return self.order_number


class OrderLineItem(models.Model):
    order = models.ForeignKey(Order, null=False, blank=False, on_delete=models.CASCADE, related_name='lineitems')
    product = models.ForeignKey(Product, null=False, blank=False, on_delete=models.CASCADE)
    product_size = models.CharField(max_length=2, null=True, blank=True) # XS, S, M, L, XL
    quantity = models.IntegerField(null=False, blank=False, default=0)
    lineitem_total = models.DecimalField(max_digits=6, decimal_places=2, null=False, blank=False, editable=False)

    def save(self, *args, **kwargs):
        """
        Override the original save method to set the lineitem total
        and update the order total.
        """
        self.lineitem_total = self.product.price * self.quantity
        super().save(*args, **kwargs)

    def __str__(self):
        return f'SKU {self.product.sku} on order {self.order.order_number}'

```
- python3 manage.py makemigrations --dry-run
- python3 manage.py makemigrations
- python3 manage.py migrate --plan
- python3 manage.py migrate
- git add . 
- git commit -m "created checkout app and models"
- git push


- checkout/admin.py
```
from django.contrib import admin
from .models import Order, OrderLineItem


class OrderLineItemAdminInline(admin.TabularInline):
    model = OrderLineItem
    readonly_fields = ('lineitem_total',)


class OrderAdmin(admin.ModelAdmin):
    inlines = (OrderLineItemAdminInline,)

    readonly_fields = ('order_number', 'date',
                       'delivery_cost', 'order_total',
                       'grand_total',)

    fields = ('order_number', 'date', 'full_name',
              'email', 'phone_number', 'country',
              'postcode', 'town_or_city', 'street_address1',
              'street_address2', 'county', 'delivery_cost',
              'order_total', 'grand_total',)

    list_display = ('order_number', 'date', 'full_name',
                    'order_total', 'delivery_cost',
                    'grand_total',)

    ordering = ('-date',)

admin.site.register(Order, OrderAdmin)

```
- create checkout/signals.py
```
from django.db.models.signals import post_save, post_delete
from django.dispatch import receiver

from .models import OrderLineItem

@receiver(post_save, sender=OrderLineItem)
def update_on_save(sender, instance, created, **kwargs):
    """
    Update order total on lineitem update/create
    """
    instance.order.update_total()

@receiver(post_delete, sender=OrderLineItem)
def update_on_save(sender, instance, **kwargs):
    """
    Update order total on lineitem delete
    """
    instance.order.update_total()

```
- update checkout/apps.py
```
from django.apps import AppConfig


class CheckoutConfig(AppConfig):
    name = 'checkout'

    def ready(self):
        import checkout.signals

```
- create checkout/forms.py
```
from django import forms
from .models import Order


class OrderForm(forms.ModelForm):
    class Meta:
        model = Order
        fields = ('full_name', 'email', 'phone_number',
                  'street_address1', 'street_address2',
                  'town_or_city', 'postcode', 'country',
                  'county',)

    def __init__(self, *args, **kwargs):
        """
        Add placeholders and classes, remove auto-generated
        labels and set autofocus on first field
        """
        super().__init__(*args, **kwargs)
        placeholders = {
            'full_name': 'Full Name',
            'email': 'Email Address',
            'phone_number': 'Phone Number',
            'country': 'Country',
            'postcode': 'Postal Code',
            'town_or_city': 'Town or City',
            'street_address1': 'Street Address 1',
            'street_address2': 'Street Address 2',
            'county': 'County',
        }

        self.fields['full_name'].widget.attrs['autofocus'] = True
        for field in self.fields:
            if self.fields[field].required:
                placeholder = f'{placeholders[field]} *'
            else:
                placeholder = placeholders[field]
            self.fields[field].widget.attrs['placeholder'] = placeholder
            self.fields[field].widget.attrs['class'] = 'stripe-style-input'
            self.fields[field].label = False

```
- git add . 
- git commit -m "created checkout app and models"
- git push


- checkout/views.py
```
from django.shortcuts import render, redirect, reverse
from django.contrib import messages

from .forms import OrderForm


def checkout(request):
    bag = request.session.get('bag', {})
    if not bag:
        messages.error(request, "There's nothing in your bag at the moment")
        return redirect(reverse('products'))

    order_form = OrderForm()
    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
    }

    return render(request, template, context)

```
- create checkout/urls.py
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.checkout, name='checkout')
]

```
- update boutique/urls.py
```
urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('allauth.urls')),
    path('', include('home.urls')),
    path('products/', include('products.urls')),
    path('bag/', include('bag.urls')),
    path('checkout/', include('checkout.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

```
- create checkout/templates/checkout/checkout.html
- create checkout/static/checkout/css/checkout.css
- pip3 install django-crispy-forms
- update boutique/settings.py
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
    'bag',
    'checkout',

    # Other
    'crispy_forms',
]

CRISPY_TEMPLATE_PACK = 'bootstrap4'

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
                'django.template.context_processors.request', # required by allauth
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'django.template.context_processors.media',
                'bag.contexts.bag_contents',
            ],
            'builtins': [
                'crispy_forms.templatetags.crispy_forms_tags',
                'crispy_forms.templatetags.crispy_forms_field',
            ]
        },
    },
]
```
- pip3 freeze --local > requirements.txt
- update bag/templates/bag/bag.html
- git add . 
- git commit -m "added checkout views and templates"
- git push


### Stripe Part 1

checkout/static/checkout/css/checkout.css
```
.StripeElement,
.stripe-style-input {
  box-sizing: border-box;
  height: 40px;
  padding: 10px 12px;
  border: 1px solid transparent;
  border-radius: 0px;
  background-color: white;
  box-shadow: 0 1px 3px 0 #e6ebf1;
  -webkit-transition: box-shadow 150ms ease;
  transition: box-shadow 150ms ease;
}

.StripeElement--focus,
.stripe-style-input:focus,
.stripe-style-input:active {
  box-shadow: 0 1px 3px 0 #cfd7df;
}

.StripeElement--webkit-autofill {
  background-color: #fefde5 !important;
}

.stripe-style-input::placeholder {
  color: #aab7c4;
}

.fieldset-label {
  position: relative;
  right: .5rem;
}

#payment-form .form-control,
#card-element {
  color: #000;
  border: 1px solid #000;
}
```
### Stripe Part 2

templates/base.html
```
{% load static %}

<!doctype html>
<html lang="en">

<head>

    {% block meta %}
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    {% endblock %}

    {% block extra_meta %}
    {% endblock %}

    {% block corecss %}
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css"
        integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Lato&display=swap">
    <link rel="stylesheet" href="{% static 'css/base.css' %}">
    {% endblock %}

    {% block extra_css %}
    {% endblock %}

    {% block corejs %}
    <script src="https://kit.fontawesome.com/e9c73d7092.js" crossorigin="anonymous"></script>
    <script src="https://code.jquery.com/jquery-3.4.1.min.js"
        integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo=" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js"
        integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous">
    </script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"
        integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous">
    </script>
    <!-- Stripe -->
    <script src="https://js.stripe.com/v3/"></script>
    {% endblock %}

    {% block extra_js %}
    {% endblock %}

    <title>Boutique Ado {% block extra_title %}{% endblock %}</title>
</head>

<body>
    <header class="container-fluid fixed-top">
        <div id="topnav" class="row bg-white pt-lg-2 d-none d-lg-flex">
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0 text-center text-lg-left">
                <a href="{% url 'home' %}" class="nav-link main-logo-link">
                    <h2 class="logo-font text-black my-0"><strong>Boutique</strong> Ado</h2>
                </a>
            </div>
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0">
                <form method="GET" action="{% url 'products' %}">
                    <div class="input-group w-100">
                        <input class="form-control border border-black rounded-0" type="text" name="q"
                            placeholder="Search our site">
                        <div class="input-group-append">
                            <button class="form-control btn btn-black border border-black rounded-0" type="submit">
                                <span class="icon">
                                    <i class="fas fa-search"></i>
                                </span>
                            </button>
                        </div>
                    </div>
                </form>
            </div>
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0">
                <ul class="list-inline list-unstyled text-center text-lg-right my-0">
                    <li class="list-inline-item dropdown">
                        <a class="text-black nav-link" href="#" id="user-options" data-toggle="dropdown"
                            aria-haspopup="true" aria-expanded="false">
                            <div class="text-center">
                                <div><i class="fas fa-user fa-lg"></i></div>
                                <p class="my-0">My Account</p>
                            </div>
                        </a>
                        <div class="dropdown-menu border-0" aria-labelledby="user-options">
                            {% if request.user.is_authenticated %}
                            {% if request.user.is_superuser %}
                            <a href="" class="dropdown-item">Product Management</a>
                            {% endif %}
                            <a href="" class="dropdown-item">My Profile</a>
                            <a href="{% url 'account_logout' %}" class="dropdown-item">Logout</a>
                            {% else %}
                            <a href="{% url 'account_signup' %}" class="dropdown-item">Register</a>
                            <a href="{% url 'account_login' %}" class="dropdown-item">Login</a>
                            {% endif %}
                        </div>
                    </li>
                    <li class="list-inline-item">
                        <a class="{% if grand_total %}text-info font-weight-bold{% else %}text-black{% endif %} nav-link"
                            href="{% url 'view_bag' %}">
                            <div class="text-center">
                                <div><i class="fas fa-shopping-bag fa-lg"></i></div>
                                <p class="my-0">
                                    {% if grand_total %}
                                    ${{ grand_total|floatformat:2 }}
                                    {% else %}
                                    $0.00
                                    {% endif %}
                                </p>
                            </div>
                        </a>
                    </li>
                </ul>
            </div>
        </div>
        <div class="row bg-white">
            <nav class="navbar navbar-expand-lg navbar-light w-100">
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#main-nav"
                    aria-controls="main-nav" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                {% include 'includes/mobile-top-header.html' %}
                {% include 'includes/main-nav.html' %}
            </nav>
        </div>
        <div id="delivery-banner" class="row text-center">
            <div class="col bg-black text-white">
                <h4 class="logo-font my-1">Free delivery on orders over ${{ free_delivery_threshold }}!</h4>
            </div>
        </div>
    </header>

    {% if messages %}
    <div class="message-container">
        {% for message in messages %}
        {% with message.level as level %}
        {% if level == 40 %}
        {% include 'includes/toasts/toast_error.html' %}
        {% elif level == 30 %}
        {% include 'includes/toasts/toast_warning.html' %}
        {% elif level == 25 %}
        {% include 'includes/toasts/toast_success.html' %}
        {% else %}
        {% include 'includes/toasts/toast_info.html' %}
        {% endif %}
        {% endwith %}
        {% endfor %}
    </div>
    {% endif %}

    {% block page_header %}
    {% endblock %}

    {% block content %}
    {% endblock %}

    {% block postloadjs %}
    <script type="text/javascript">
        $('.toast').toast('show');
    </script>
    {% endblock %}


</body>

</html>
```


checkout/templates/checkout/checkout.html
```
{% extends "base.html" %}
{% load static %}
{% load bag_tools %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'checkout/css/checkout.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">Checkout</h2>
                <hr>
            </div>
        </div>

        <div class="row">
            <div class="col-12 col-lg-6 order-lg-last mb-5">
                <p class="text-muted">Order Summary ({{ product_count }})</p>
                <div class="row">
                    <div class="col-7 offset-2">
                        <p class="mb-1 mt-0 small text-muted">Item</p>
                    </div>
                    <div class="col-3 text-right">
                        <p class="mb-1 mt-0 small text-muted">Subtotal</p>
                    </div>
                </div>
                {% for item in bag_items %}
                    <div class="row">
                        <div class="col-2 mb-1">
                            <a href="{% url 'product_detail' item.product.id %}">
                                {% if item.product.image %}
                                    <img class="w-100" src="{{ item.product.image.url }}" alt="{{ product.name }}">
                                {% else %}
                                    <img class="w-100" src="{{ MEDIA_URL }}noimage.png" alt="{{ product.name }}">
                                {% endif %}
                            </a>
                        </div>
                        <div class="col-7">
                            <p class="my-0"><strong>{{ item.product.name }}</strong></p>
                            <p class="my-0 small">Size: {% if item.product.has_sizes %}{{ item.size|upper }}{% else %}N/A{% endif %}</p>
                            <p class="my-0 small text-muted">Qty: {{ item.quantity }}</p>
                        </div>
                        <div class="col-3 text-right">
                            <p class="my-0 small text-muted">${{ item.product.price | calc_subtotal:item.quantity }}</p>
                        </div>
                    </div>
                {% endfor %}
                <hr class="my-0">
                <div class="row text-black text-right">
                    <div class="col-7 offset-2">
                        <p class="my-0">Order Total:</p>
                        <p class="my-0">Delivery:</p>
                        <p class="my-0">Grand Total:</p>
                    </div>
                    <div class="col-3">
                        <p class="my-0">${{ total | floatformat:2 }}</p>
                        <p class="my-0">${{ delivery | floatformat:2 }}</p>
                        <p class="my-0"><strong>${{ grand_total | floatformat:2 }}</strong></p>
                    </div>
                </div>
            </div>
            <div class="col-12 col-lg-6">
                <p class="text-muted">Please fill out the form below to complete your order</p>
                <form action="{% url 'checkout' %}" method="POST" id="payment-form">
                    {% csrf_token %}
                    <fieldset class="rounded px-3 mb-5">
                        <legend class="fieldset-label small text-black px-2 w-auto">Details</legend>
                        {{ order_form.full_name | as_crispy_field }}
                        {{ order_form.email | as_crispy_field }}
                    </fieldset>
                    <fieldset class="rounded px-3 mb-5">
                        <legend class="fieldset-label small text-black px-2 w-auto">Delivery</legend>
                        {{ order_form.phone_number | as_crispy_field }}
                        {{ order_form.country | as_crispy_field }}
                        {{ order_form.postcode | as_crispy_field }}
                        {{ order_form.town_or_city | as_crispy_field }}
                        {{ order_form.street_address1 | as_crispy_field }}
                        {{ order_form.street_address2 | as_crispy_field }}
                        {{ order_form.county | as_crispy_field }}
                        <div class="form-check form-check-inline float-right mr-0">
							{% if user.is_authenticated %}
								<label class="form-check-label" for="id-save-info">Save this delivery information to my profile</label>
                                <input class="form-check-input ml-2 mr-0" type="checkbox" id="id-save-info" name="save-info" checked>
							{% else %}
								<label class="form-check-label" for="id-save-info">
                                    <a class="text-info" href="{% url 'account_signup' %}">Create an account</a> or 
                                    <a class="text-info" href="{% url 'account_login' %}">login</a> to save this information
                                </label>
							{% endif %}
						</div>
                    </fieldset>
                    <fieldset class="px-3">
                        <legend class="fieldset-label small text-black px-2 w-auto">Payment</legend>
                        <!-- A Stripe card element will go here -->
                        <div class="mb-3" id="card-element"></div>

                        <!-- Used to display form errors -->
                        <div class="mb-3 text-danger" id="card-errors" role="alert"></div>
                    </fieldset>

                    <div class="submit-button text-right mt-5 mb-2">                    
						<a href="{% url 'view_bag' %}" class="btn btn-outline-black rounded-0">
							<span class="icon">
								<i class="fas fa-chevron-left"></i>
							</span>
							<span class="font-weight-bold">Adjust Bag</span>
						</a>
						<button id="submit-button" class="btn btn-black rounded-0">
							<span class="font-weight-bold">Complete Order</span>
							<span class="icon">
								<i class="fas fa-lock"></i>
							</span>
						</button>
						<p class="small text-danger my-0">
							<span class="icon">
								<i class="fas fa-exclamation-circle"></i>
							</span>
							<span>Your card will be charged <strong>${{ grand_total|floatformat:2 }}</strong></span>
						</p>
					</div>
                </form>
            </div>
        </div>
    </div>
{% endblock %}

{% block postloadjs %}
    {{ block.super }}
    {{ stripe_public_key|json_script:"id_stripe_public_key" }}
    {{ client_secret|json_script:"id_client_secret" }}
    <script src="{% static 'checkout/js/stripe_elements.js' %}"></script>
{% endblock %}
```


checkout/views.py
```
from django.shortcuts import render, redirect, reverse
from django.contrib import messages

from .forms import OrderForm


def checkout(request):
    bag = request.session.get('bag', {})
    if not bag:
        messages.error(request, "There's nothing in your bag at the moment")
        return redirect(reverse('products'))

    order_form = OrderForm()
    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
        'stripe_public_key': 'pk_test_0SMREd7Vdweb1MGRi8S0EycR00JVzSAs5O',
        'client_secret': 'test client secret',
    }

    return render(request, template, context)
```


checkout/static/checkout/js/stripe_elements.js
```
/*
    Core logic/payment flow for this comes from here:
    https://stripe.com/docs/payments/accept-a-payment
    CSS from here: 
    https://stripe.com/docs/stripe-js
*/

var stripe_public_key = $('#id_stripe_public_key').text().slice(1, -1);
var client_secret = $('#id_client_secret').text().slice(1, -1);
var stripe = Stripe(stripe_public_key);
var elements = stripe.elements();
var style = {
    base: {
        color: '#000',
        fontFamily: '"Helvetica Neue", Helvetica, sans-serif',
        fontSmoothing: 'antialiased',
        fontSize: '16px',
        '::placeholder': {
            color: '#aab7c4'
        }
    },
    invalid: {
        color: '#dc3545',
        iconColor: '#dc3545'
    }
};
var card = elements.create('card', {style: style});
card.mount('#card-element');
```
- git add . 
- git commit -m "added stripe elements"
- git push


### Stripe Part 3

checkout/static/checkout/js/stripe_elements.js 
```
/*
    Core logic/payment flow for this comes from here:
    https://stripe.com/docs/payments/accept-a-payment
    CSS from here: 
    https://stripe.com/docs/stripe-js
*/

var stripePublicKey = $('#id_stripe_public_key').text().slice(1, -1);
var clientSecret = $('#id_client_secret').text().slice(1, -1);
var stripe = Stripe(stripePublicKey);
var elements = stripe.elements();
var style = {
    base: {
        color: '#000',
        fontFamily: '"Helvetica Neue", Helvetica, sans-serif',
        fontSmoothing: 'antialiased',
        fontSize: '16px',
        '::placeholder': {
            color: '#aab7c4'
        }
    },
    invalid: {
        color: '#dc3545',
        iconColor: '#dc3545'
    }
};
var card = elements.create('card', {style: style});
card.mount('#card-element');

// Handle realtime validation errors on the card element
card.addEventListener('change', function (event) {
    var errorDiv = document.getElementById('card-errors');
    if (event.error) {
        var html = `
            <span class="icon" role="alert">
                <i class="fas fa-times"></i>
            </span>
            <span>${event.error.message}</span>
        `;
        $(errorDiv).html(html);
    } else {
        errorDiv.textContent = '';
    }
});
```

checkout/views.py
```
from django.shortcuts import render, redirect, reverse
from django.contrib import messages

from .forms import OrderForm
from bag.contexts import bag_contents


def checkout(request):
    bag = request.session.get('bag', {})
    if not bag:
        messages.error(request, "There's nothing in your bag at the moment")
        return redirect(reverse('products'))

    current_bag = bag_contents(request)
    total = current_bag['grand_total']
    stripe_total = round(total * 100)

    order_form = OrderForm()
    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
        'stripe_public_key': 'pk_test_...',
        'client_secret': 'test client secret',
    }

    return render(request, template, context)

```

- pip3 install stripe

boutique/settings.py
```
"""
Django settings for boutique project.

Generated by 'django-admin startproject' using Django 3.2.6.

For more information on this file, see
https://docs.djangoproject.com/en/3.2/topics/settings/

For the full list of settings and their values, see
https://docs.djangoproject.com/en/3.2/ref/settings/
"""

import os
from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent


# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/3.2/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-_k=yq7z#2g*rzk@_s75e67f#m$h1u+#1@(g%!^%xitph)_ic%w'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []


# Application definition

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
    'bag',
    'checkout',

    # Other
    'crispy_forms',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'boutique.urls'

CRISPY_TEMPLATE_PACK = 'bootstrap4'

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
                'django.template.context_processors.media',
                'bag.contexts.bag_contents',
            ],
            'builtins': [
                'crispy_forms.templatetags.crispy_forms_tags',
                'crispy_forms.templatetags.crispy_forms_field',
            ]
        },
    },
]

MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'

AUTHENTICATION_BACKENDS = [
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
]

SITE_ID = 1

EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'

ACCOUNT_AUTHENTICATION_METHOD = 'username_email'
ACCOUNT_EMAIL_REQUIRED = True
ACCOUNT_EMAIL_VERIFICATION = 'mandatory'
ACCOUNT_SIGNUP_EMAIL_ENTER_TWICE = True
ACCOUNT_USERNAME_MIN_LENGTH = 4
LOGIN_URL = '/accounts/login/'
LOGIN_REDIRECT_URL = '/'

WSGI_APPLICATION = 'boutique.wsgi.application'


# Database
# https://docs.djangoproject.com/en/3.2/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}


# Password validation
# https://docs.djangoproject.com/en/3.2/ref/settings/#auth-password-validators

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]


# Internationalization
# https://docs.djangoproject.com/en/3.2/topics/i18n/

LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_L10N = True

USE_TZ = True


# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/3.0/howto/static-files/

STATIC_URL = '/static/'
STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)

MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

# Stripe
FREE_DELIVERY_THRESHOLD = 50
STANDARD_DELIVERY_PERCENTAGE = 10
STRIPE_CURRENCY = 'usd'
STRIPE_PUBLIC_KEY = os.getenv('STRIPE_PUBLIC_KEY', '')
STRIPE_SECRET_KEY = os.getenv('STRIPE_SECRET_KEY', '')

```

- export STRIPE_PUBLIC_KEY=pk_test_...
- set STRIPE_PUBLIC_KEY=pk_test_...

- export STRIPE_SECRET_KEY=sk_test_...
- set STRIPE_SECRET_KEY=sk_test_...

- Settings in GitPod Environment Variables
  - open GitPot Workspace Dashboard
  - open Settings
  - open Variables
    - New Variable
    - Name: STRIPE_PUBLIC_KEY
    - Value: pk_test_...
    - Scope: puksrevolution/*
    - New Variable
    - Name: STRIPE_SECRET_KEY
    - Value: sk_test_...
    - Scope: puksrevolution/*
- Stop the Workspace and start it again (do to be on the save site first a git add .)

### Stripe Part 4

checkout/views.py
```
from django.shortcuts import render, redirect, reverse
from django.contrib import messages
from django.conf import settings

from .forms import OrderForm
from bag.contexts import bag_contents

import stripe


def checkout(request):
    stripe_public_key = settings.STRIPE_PUBLIC_KEY
    stripe_secret_key = settings.STRIPE_SECRET_KEY

    bag = request.session.get('bag', {})
    if not bag:
        messages.error(request, "There's nothing in your bag at the moment")
        return redirect(reverse('products'))

    current_bag = bag_contents(request)
    total = current_bag['grand_total']
    stripe_total = round(total * 100)
    stripe.api_key = stripe_secret_key
    intent = stripe.PaymentIntent.create(
        amount=stripe_total,
        currency=settings.STRIPE_CURRENCY,
    )

    print(intent)

    order_form = OrderForm()
    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
        'stripe_public_key': 'pk_test_...',
        'client_secret': 'test client secret',
    }

    return render(request, template, context)

```

- python3 manage.py runserver

checkout/views.py
```
from django.shortcuts import render, redirect, reverse
from django.contrib import messages
from django.conf import settings

from .forms import OrderForm
from bag.contexts import bag_contents

import stripe


def checkout(request):
    stripe_public_key = settings.STRIPE_PUBLIC_KEY
    stripe_secret_key = settings.STRIPE_SECRET_KEY

    bag = request.session.get('bag', {})
    if not bag:
        messages.error(request, "There's nothing in your bag at the moment")
        return redirect(reverse('products'))

    current_bag = bag_contents(request)
    total = current_bag['grand_total']
    stripe_total = round(total * 100)
    stripe.api_key = stripe_secret_key
    intent = stripe.PaymentIntent.create(
        amount=stripe_total,
        currency=settings.STRIPE_CURRENCY,
    )

    order_form = OrderForm()

    if not stripe_public_key:
        messages.warning(request, 'Stripe public key is missing. \
            Did you forget to set it in your environment?')

    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
        'stripe_public_key': stripe_public_key,
        'client_secret': intent.client_secret,
    }

    return render(request, template, context)

```

### Stripe Part 5

checkout/static/checkout/js/stripe_elements.js
```
/*
    Core logic/payment flow for this comes from here:
    https://stripe.com/docs/payments/accept-a-payment
    CSS from here: 
    https://stripe.com/docs/stripe-js
*/

var stripePublicKey = $('#id_stripe_public_key').text().slice(1, -1);
var clientSecret = $('#id_client_secret').text().slice(1, -1);
var stripe = Stripe(stripePublicKey);
var elements = stripe.elements();
var style = {
    base: {
        color: '#000',
        fontFamily: '"Helvetica Neue", Helvetica, sans-serif',
        fontSmoothing: 'antialiased',
        fontSize: '16px',
        '::placeholder': {
            color: '#aab7c4'
        }
    },
    invalid: {
        color: '#dc3545',
        iconColor: '#dc3545'
    }
};
var card = elements.create('card', {style: style});
card.mount('#card-element');

// Handle realtime validation errors on the card element
card.addEventListener('change', function (event) {
    var errorDiv = document.getElementById('card-errors');
    if (event.error) {
        var html = `
            <span class="icon" role="alert">
                <i class="fas fa-times"></i>
            </span>
            <span>${event.error.message}</span>
        `;
        $(errorDiv).html(html);
    } else {
        errorDiv.textContent = '';
    }
});

// Handle form submit
var form = document.getElementById('payment-form');

form.addEventListener('submit', function(ev) {
    ev.preventDefault();
    card.update({ 'disabled': true});
    $('#submit-button').attr('disabled', true);
    stripe.confirmCardPayment(clientSecret, {
        payment_method: {
            card: card,
        }
    }).then(function(result) {
        if (result.error) {
            var errorDiv = document.getElementById('card-errors');
            var html = `
                <span class="icon" role="alert">
                <i class="fas fa-times"></i>
                </span>
                <span>${result.error.message}</span>`;
            $(errorDiv).html(html);
            card.update({ 'disabled': false});
            $('#submit-button').attr('disabled', false);
        } else {
            if (result.paymentIntent.status === 'succeeded') {
                form.submit();
            }
        }
    });
});
```
- git add . 
- git commit -m "Added basic checkout functionality"
- git push


### Stripe Part 6

checkout/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.conf import settings

from .forms import OrderForm
from .models import Order, OrderLineItem
from products.models import Product
from bag.contexts import bag_contents

import stripe


def checkout(request):
    stripe_public_key = settings.STRIPE_PUBLIC_KEY
    stripe_secret_key = settings.STRIPE_SECRET_KEY

    if request.method == 'POST':
        bag = request.session.get('bag', {})

        form_data = {
            'full_name': request.POST['full_name'],
            'email': request.POST['email'],
            'phone_number': request.POST['phone_number'],
            'country': request.POST['country'],
            'postcode': request.POST['postcode'],
            'town_or_city': request.POST['town_or_city'],
            'street_address1': request.POST['street_address1'],
            'street_address2': request.POST['street_address2'],
            'county': request.POST['county'],
        }
        order_form = OrderForm(form_data)
        if order_form.is_valid():
            order = order_form.save()
            for item_id, item_data in bag.items():
                try:
                    product = Product.objects.get(id=item_id)
                    if isinstance(item_data, int):
                        order_line_item = OrderLineItem(
                            order=order,
                            product=product,
                            quantity=item_data,
                        )
                        order_line_item.save()
                    else:
                        for size, quantity in item_data['items_by_size'].items():
                            order_line_item = OrderLineItem(
                                order=order,
                                product=product,
                                quantity=quantity,
                                product_size=size,
                            )
                            order_line_item.save()
                except Product.DoesNotExist:
                    messages.error(request, (
                        "One of the products in your bag wasn't found in our database. "
                        "Please call us for assistance!")
                    )
                    order.delete()
                    return redirect(reverse('view_bag'))

            request.session['save_info'] = 'save-info' in request.POST
            return redirect(reverse('checkout_success', args=[order.order_number]))
        else:
            messages.error(request, 'There was an error with your form. \
                Please double check your information.')
    else:
        bag = request.session.get('bag', {})
        if not bag:
            messages.error(request, "There's nothing in your bag at the moment")
            return redirect(reverse('products'))

        current_bag = bag_contents(request)
        total = current_bag['grand_total']
        stripe_total = round(total * 100)
        stripe.api_key = stripe_secret_key
        intent = stripe.PaymentIntent.create(
            amount=stripe_total,
            currency=settings.STRIPE_CURRENCY,
        )

        order_form = OrderForm()

    if not stripe_public_key:
        messages.warning(request, 'Stripe public key is missing. \
            Did you forget to set it in your environment?')

    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
        'stripe_public_key': stripe_public_key,
        'client_secret': intent.client_secret,
    }

    return render(request, template, context)


def checkout_success(request, order_number):
    """
    Handle successful checkouts
    """
    save_info = request.session.get('save_info')
    order = get_object_or_404(Order, order_number=order_number)
    messages.success(request, f'Order successfully processed! \
        Your order number is {order_number}. A confirmation \
        email will be sent to {order.email}.')

    if 'bag' in request.session:
        del request.session['bag']

    template = 'checkout/checkout_success.html'
    context = {
        'order': order,
    }

    return render(request, template, context)

```

checkout/urls.py
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.checkout, name='checkout'),
    path('checkout_success/<order_number>', views.checkout_success, name='checkout_success'),
]

```

### Stripe Part 7

checkout/templates/checkout/checkout_success.html
```
{% extends "base.html" %}
{% load static %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'checkout/css/checkout.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">Thank You</h2>
                <hr>
                <p class="text-black">Your order information is below. A confirmation email will be sent to <strong>{{ order.email }}</strong>.</p>
            </div>
        </div>

        <div class="row">
            <div class="col-12 col-lg-7"></div>
        </div>
        <div class="row">
			<div class="col-12 col-lg-7 text-right">
				<a href="{% url 'products' %}?category=new_arrivals,deals,clearance" class="btn btn-black rounded-0 my-2">
					<span class="icon mr-2">
						<i class="fas fa-gifts"></i>
					</span>
					<span class="text-uppercase">Now check out the latest deals!</span>
				</a>
			</div>
		</div>
    </div>
{% endblock %}
```

checkout/__init__.py
```
default_app_config = 'checkout.apps.CheckoutConfig'
```

checkout/models.py
```
import uuid

from django.db import models
from django.db.models import Sum
from django.conf import settings

from products.models import Product


class Order(models.Model):
    order_number = models.CharField(max_length=32, null=False, editable=False)
    full_name = models.CharField(max_length=50, null=False, blank=False)
    email = models.EmailField(max_length=254, null=False, blank=False)
    phone_number = models.CharField(max_length=20, null=False, blank=False)
    country = models.CharField(max_length=40, null=False, blank=False)
    postcode = models.CharField(max_length=20, null=True, blank=True)
    town_or_city = models.CharField(max_length=40, null=False, blank=False)
    street_address1 = models.CharField(max_length=80, null=False, blank=False)
    street_address2 = models.CharField(max_length=80, null=True, blank=True)
    county = models.CharField(max_length=80, null=True, blank=True)
    date = models.DateTimeField(auto_now_add=True)
    delivery_cost = models.DecimalField(max_digits=6, decimal_places=2, null=False, default=0)
    order_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)
    grand_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)

    def _generate_order_number(self):
        """
        Generate a random, unique order number using UUID
        """
        return uuid.uuid4().hex.upper()

    def update_total(self):
        """
        Update grand total each time a line item is added,
        accounting for delivery costs.
        """
        self.order_total = self.lineitems.aggregate(Sum('lineitem_total'))['lineitem_total__sum'] or 0
        if self.order_total < settings.FREE_DELIVERY_THRESHOLD:
            self.delivery_cost = self.order_total * settings.STANDARD_DELIVERY_PERCENTAGE / 100
        else:
            self.delivery_cost = 0
        self.grand_total = self.order_total + self.delivery_cost
        self.save()

    def save(self, *args, **kwargs):
        """
        Override the original save method to set the order number
        if it hasn't been set already.
        """
        if not self.order_number:
            self.order_number = self._generate_order_number()
        super().save(*args, **kwargs)

    def __str__(self):
        return self.order_number


class OrderLineItem(models.Model):
    order = models.ForeignKey(Order, null=False, blank=False, on_delete=models.CASCADE, related_name='lineitems')
    product = models.ForeignKey(Product, null=False, blank=False, on_delete=models.CASCADE)
    product_size = models.CharField(max_length=2, null=True, blank=True) # XS, S, M, L, XL
    quantity = models.IntegerField(null=False, blank=False, default=0)
    lineitem_total = models.DecimalField(max_digits=6, decimal_places=2, null=False, blank=False, editable=False)

    def save(self, *args, **kwargs):
        """
        Override the original save method to set the lineitem total
        and update the order total.
        """
        self.lineitem_total = self.product.price * self.quantity
        super().save(*args, **kwargs)

    def __str__(self):
        return f'SKU {self.product.sku} on order {self.order.order_number}'

```

checkout/signals.py
```
from django.db.models.signals import post_save, post_delete
from django.dispatch import receiver

from .models import OrderLineItem


@receiver(post_save, sender=OrderLineItem)
def update_on_save(sender, instance, created, **kwargs):
    """
    Update order total on lineitem update/create
    """
    instance.order.update_total()


@receiver(post_delete, sender=OrderLineItem)
def update_on_delete(sender, instance, **kwargs):
    """
    Update order total on lineitem delete
    """
    instance.order.update_total()

```
- git add . 
- git commit -m "Added checkout success logic"
- git push


### Stripe Part 8

checkout/templates/checkout/checkout_success.html
```
{% extends "base.html" %}
{% load static %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'checkout/css/checkout.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">Thank You</h2>
                <hr>
                <p class="text-black">Your order information is below. A confirmation email will be sent to <strong>{{ order.email }}</strong>.</p>
            </div>
        </div>

        <div class="row">
            <div class="col-12 col-lg-7">
                <div class="order-confirmation-wrapper p-2 border">
                    <div class="row">
                        <div class="col">
                            <small class="text-muted">Order Info:</small>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Order Number</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.order_number }}</p>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Order Date</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.date }}</p>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col">
                            <small class="text-muted">Order Details:</small>
                        </div>
                    </div>

                    {% for item in order.lineitems.all %}
                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="small mb-0 text-black font-weight-bold">
                                {{ item.product.name }}{% if item.product_size %} - Size {{ item.product_size|upper }}{% endif %}
                            </p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="small mb-0">{{ item.quantity }} @ ${{ item.product.price }} each</p>
                        </div>
                    </div>
                    {% endfor %}

                    <div class="row">
                        <div class="col">
                            <small class="text-muted">Delivering To:</small>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Full Name</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.full_name }}</p>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Address 1</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.street_address1 }}</p>
                        </div>
                    </div>

                    {% if order.street_address2 %}
                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Address 2</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.street_address1 }}</p>
                        </div>
                    </div>
                    {% endif %}

                    {% if order.county %}
                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">County</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.county }}</p>
                        </div>
                    </div>
                    {% endif %}

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Town or City</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.town_or_city }}</p>
                        </div>
                    </div>

                    {% if order.postcode %}
                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Postal Code</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.postcode }}</p>
                        </div>
                    </div>
                    {% endif %}

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Country</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.country }}</p>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Phone Number</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.phone_number }}</p>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col">
                            <small class="text-muted">Billing Info:</small>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Order Total</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.order_total }}</p>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Delivery</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.delivery_cost }}</p>
                        </div>
                    </div>

                    <div class="row">
                        <div class="col-12 col-md-4">
                            <p class="mb-0 text-black font-weight-bold">Grand Total</p>
                        </div>
                        <div class="col-12 col-md-8 text-md-right">
                            <p class="mb-0">{{ order.grand_total }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="row">
			<div class="col-12 col-lg-7 text-right">
				<a href="{% url 'products' %}?category=new_arrivals,deals,clearance" class="btn btn-black rounded-0 my-2">
					<span class="icon mr-2">
						<i class="fas fa-gifts"></i>
					</span>
					<span class="text-uppercase">Now check out the latest deals!</span>
				</a>
			</div>
		</div>
    </div>
{% endblock %}
```
- git add . 
- git commit -m "Added order summary to checkout success page"
- git push


### Stripe Part 9

checkout/templates/checkout/checkout.html
```
{% extends "base.html" %}
{% load static %}
{% load bag_tools %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'checkout/css/checkout.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">Checkout</h2>
                <hr>
            </div>
        </div>

        <div class="row">
            <div class="col-12 col-lg-6 order-lg-last mb-5">
                <p class="text-muted">Order Summary ({{ product_count }})</p>
                <div class="row">
                    <div class="col-7 offset-2">
                        <p class="mb-1 mt-0 small text-muted">Item</p>
                    </div>
                    <div class="col-3 text-right">
                        <p class="mb-1 mt-0 small text-muted">Subtotal</p>
                    </div>
                </div>
                {% for item in bag_items %}
                    <div class="row">
                        <div class="col-2 mb-1">
                            <a href="{% url 'product_detail' item.product.id %}">
                                {% if item.product.image %}
                                    <img class="w-100" src="{{ item.product.image.url }}" alt="{{ product.name }}">
                                {% else %}
                                    <img class="w-100" src="{{ MEDIA_URL }}noimage.png" alt="{{ product.name }}">
                                {% endif %}
                            </a>
                        </div>
                        <div class="col-7">
                            <p class="my-0"><strong>{{ item.product.name }}</strong></p>
                            <p class="my-0 small">Size: {% if item.product.has_sizes %}{{ item.size|upper }}{% else %}N/A{% endif %}</p>
                            <p class="my-0 small text-muted">Qty: {{ item.quantity }}</p>
                        </div>
                        <div class="col-3 text-right">
                            <p class="my-0 small text-muted">${{ item.product.price | calc_subtotal:item.quantity }}</p>
                        </div>
                    </div>
                {% endfor %}
                <hr class="my-0">
                <div class="row text-black text-right">
                    <div class="col-7 offset-2">
                        <p class="my-0">Order Total:</p>
                        <p class="my-0">Delivery:</p>
                        <p class="my-0">Grand Total:</p>
                    </div>
                    <div class="col-3">
                        <p class="my-0">${{ total | floatformat:2 }}</p>
                        <p class="my-0">${{ delivery | floatformat:2 }}</p>
                        <p class="my-0"><strong>${{ grand_total | floatformat:2 }}</strong></p>
                    </div>
                </div>
            </div>
            <div class="col-12 col-lg-6">
                <p class="text-muted">Please fill out the form below to complete your order</p>
                <form action="{% url 'checkout' %}" method="POST" id="payment-form">
                    {% csrf_token %}
                    <fieldset class="rounded px-3 mb-5">
                        <legend class="fieldset-label small text-black px-2 w-auto">Details</legend>
                        {{ order_form.full_name | as_crispy_field }}
                        {{ order_form.email | as_crispy_field }}
                    </fieldset>
                    <fieldset class="rounded px-3 mb-5">
                        <legend class="fieldset-label small text-black px-2 w-auto">Delivery</legend>
                        {{ order_form.phone_number | as_crispy_field }}
                        {{ order_form.country | as_crispy_field }}
                        {{ order_form.postcode | as_crispy_field }}
                        {{ order_form.town_or_city | as_crispy_field }}
                        {{ order_form.street_address1 | as_crispy_field }}
                        {{ order_form.street_address2 | as_crispy_field }}
                        {{ order_form.county | as_crispy_field }}
                        <div class="form-check form-check-inline float-right mr-0">
							{% if user.is_authenticated %}
								<label class="form-check-label" for="id-save-info">Save this delivery information to my profile</label>
                                <input class="form-check-input ml-2 mr-0" type="checkbox" id="id-save-info" name="save-info" checked>
							{% else %}
								<label class="form-check-label" for="id-save-info">
                                    <a class="text-info" href="{% url 'account_signup' %}">Create an account</a> or 
                                    <a class="text-info" href="{% url 'account_login' %}">login</a> to save this information
                                </label>
							{% endif %}
						</div>
                    </fieldset>
                    <fieldset class="px-3">
                        <legend class="fieldset-label small text-black px-2 w-auto">Payment</legend>
                        <!-- A Stripe card element will go here -->
                        <div class="mb-3" id="card-element"></div>

                        <!-- Used to display form errors -->
                        <div class="mb-3 text-danger" id="card-errors" role="alert"></div>
                    </fieldset>

                    <div class="submit-button text-right mt-5 mb-2">                    
						<a href="{% url 'view_bag' %}" class="btn btn-outline-black rounded-0">
							<span class="icon">
								<i class="fas fa-chevron-left"></i>
							</span>
							<span class="font-weight-bold">Adjust Bag</span>
						</a>
						<button id="submit-button" class="btn btn-black rounded-0">
							<span class="font-weight-bold">Complete Order</span>
							<span class="icon">
								<i class="fas fa-lock"></i>
							</span>
						</button>
						<p class="small text-danger my-0">
							<span class="icon">
								<i class="fas fa-exclamation-circle"></i>
							</span>
							<span>Your card will be charged <strong>${{ grand_total|floatformat:2 }}</strong></span>
						</p>
					</div>
                </form>
            </div>
        </div>
    </div>
    <div id="loading-overlay">
        <h1 class="text-light logo-font loading-spinner">
            <span class="icon">
                <i class="fas fa-3x fa-sync-alt fa-spin"></i>
            </span>
        </h1>
    </div>
{% endblock %}

{% block postloadjs %}
    {{ block.super }}
    {{ stripe_public_key|json_script:"id_stripe_public_key" }}
    {{ client_secret|json_script:"id_client_secret" }}
    <script src="{% static 'checkout/js/stripe_elements.js' %}"></script>
{% endblock %}
```

checkout/static/checkout/css/checkout.css
```
#loading-overlay {
	display: none;
	position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(23, 162, 184, .85);
    z-index: 9999;
}

.loading-spinner {
	display: flex;
    align-items: center;
    justify-content: center;
    margin: 0;
    height: 100%;
}
```

checkout/static/checkout/js/stripe_elements.js 
```
// Handle form submit
var form = document.getElementById('payment-form');

form.addEventListener('submit', function(ev) {
    ev.preventDefault();
    card.update({ 'disabled': true});
    $('#submit-button').attr('disabled', true);
    $('#payment-form').fadeToggle(100);
    $('#loading-overlay').fadeToggle(100);
    stripe.confirmCardPayment(clientSecret, {
        payment_method: {
            card: card,
        }
    }).then(function(result) {
        if (result.error) {
            var errorDiv = document.getElementById('card-errors');
            var html = `
                <span class="icon" role="alert">
                <i class="fas fa-times"></i>
                </span>
                <span>${result.error.message}</span>`;
            $(errorDiv).html(html);
            $('#payment-form').fadeToggle(100);
            $('#loading-overlay').fadeToggle(100);
            card.update({ 'disabled': false});
            $('#submit-button').attr('disabled', false);
        } else {
            if (result.paymentIntent.status === 'succeeded') {
                form.submit();
            }
        }
    });
});
```
- git add . 
- git commit -m "Added loading overlay"
- git push


### Stripe Part 10

checkout/webhook_handler.py
```
from django.http import HttpResponse


class StripeWH_Handler:
    """Handle Stripe webhooks"""

    def __init__(self, request):
        self.request = request

    def handle_event(self, event):
        """
        Handle a generic/unknown/unexpected webhook event
        """
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)

```
- git add . 
- git commit -m "Added webhook handler"
- git push


### Stripe Part 11

checkout/webhook_handler.py
```
from django.http import HttpResponse


class StripeWH_Handler:
    """Handle Stripe webhooks"""

    def __init__(self, request):
        self.request = request

    def handle_event(self, event):
        """
        Handle a generic/unknown/unexpected webhook event
        """
        return HttpResponse(
            content=f'Unhandled webhook received: {event["type"]}',
            status=200)

    def handle_payment_intent_succeeded(self, event):
        """
        Handle the payment_intent.succeeded webhook from Stripe
        """
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)

    def handle_payment_intent_payment_failed(self, event):
        """
        Handle the payment_intent.payment_failed webhook from Stripe
        """
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)

```

checkout/urls.py
```
from django.urls import path
from . import views
from .webhooks import webhook

urlpatterns = [
    path('', views.checkout, name='checkout'),
    path('checkout_success/<order_number>', views.checkout_success, name='checkout_success'),
    path('wh/', webhook, name='webhook'),
]

```

checkout/webhooks.py
```
from django.conf import settings
from django.http import HttpResponse
from django.views.decorators.http import require_POST
from django.views.decorators.csrf import csrf_exempt

from checkout.webhook_handler import StripeWH_Handler

import stripe


@require_POST
@csrf_exempt
def webhook(request):
    """Listen for webhooks from Stripe"""
    # Setup
    wh_secret = settings.STRIPE_WH_SECRET
    stripe.api_key = settings.STRIPE_SECRET_KEY

    # Get the webhook data and verify its signature
    payload = request.body
    sig_header = request.META['HTTP_STRIPE_SIGNATURE']
    event = None

    try:
        event = stripe.Webhook.construct_event(
        payload, sig_header, wh_secret
        )
    except ValueError as e:
        # Invalid payload
        return HttpResponse(status=400)
    except stripe.error.SignatureVerificationError as e:
        # Invalid signature
        return HttpResponse(status=400)
    except Exception as e:
        return HttpResponse(content=e, status=400)

    print('Success!')
    return HttpResponse(status=200)

```

boutique/settings.py 
```
STRIPE_WH_SECRET = os.getenv('STRIPE_WH_SECRET', '')

```

- creating endpoint
  - get hompage url https://8000-green-elephant-3el2rqbx.ws-eu16.gitpod.io/
  - Stripe:
    - Developer
    - Add an endpoint
    - Endpoint URL: https://8000-green-elephant-3el2rqbx.ws-eu16.gitpod.io/checkout/wh/
    - +select events
      - select all events
      - add events
    - Add endpoint
    - get the Signing secret   

- export STRIPE_WH_SECRET=whsec_0eauEPVhTEE4jpXNT0y2IIMDInW0WrwD
- set STRIPE_WH_SECRET=whsec_0eauEPVhTEE4jpXNT0y2IIMDInW0WrwD

- Settings in GitPod Environment Variables
  - open GitPot Workspace Dashboard
  - open Settings
  - open Variables
    - New Variable
    - Name: STRIPE_PUBLIC_KEY
    - Value: pk_test_...
    - Scope: puksrevolution/*
    - New Variable
    - Name: STRIPE_SECRET_KEY
    - Value: sk_test_...
    - Scope: puksrevolution/*
- Stop the Workspace and start it again (do to be on the save site first a git add .)

- python3 manage.py runserver
  - in terminal needs to see the message:
    Success!
    [01/Sep/2021 16:34:41] "POST /checkout/wh/ HTTP/1.1" 200 0


### Stripe Part 12

checkout/webhooks.py
```
from django.conf import settings
from django.http import HttpResponse
from django.views.decorators.http import require_POST
from django.views.decorators.csrf import csrf_exempt

from checkout.webhook_handler import StripeWH_Handler

import stripe


@require_POST
@csrf_exempt
def webhook(request):
    """Listen for webhooks from Stripe"""
    # Setup
    wh_secret = settings.STRIPE_WH_SECRET
    stripe.api_key = settings.STRIPE_SECRET_KEY

    # Get the webhook data and verify its signature
    payload = request.body
    sig_header = request.META['HTTP_STRIPE_SIGNATURE']
    event = None

    try:
        event = stripe.Webhook.construct_event(
        payload, sig_header, wh_secret
        )
    except ValueError as e:
        # Invalid payload
        return HttpResponse(status=400)
    except stripe.error.SignatureVerificationError as e:
        # Invalid signature
        return HttpResponse(status=400)
    except Exception as e:
        return HttpResponse(content=e, status=400)

    # Set up a webhook handler
    handler = StripeWH_Handler(request)

    # Map webhook events to relevant handler functions
    event_map = {
        'payment_intent.succeeded': handler.handle_payment_intent_succeeded,
        'payment_intent.payment_failed': handler.handle_payment_intent_payment_failed,
    }

    # Get the webhook type from Stripe
    event_type = event['type']

    # If there's a handler for it, get it from the event map
    # Use the generic one by default
    event_handler = event_map.get(event_type, handler.handle_event)

    # Call the event handler with the event
    response = event_handler(event)
    return response

```

just for testing checkout/webhooks.py
```
from django.http import HttpResponse


class StripeWH_Handler:
    """Handle Stripe webhooks"""

    def __init__(self, request):
        self.request = request

    def handle_event(self, event):
        """
        Handle a generic/unknown/unexpected webhook event
        """
        return HttpResponse(
            content=f'Unhandled webhook received: {event["type"]}',
            status=200)

    def handle_payment_intent_succeeded(self, event):
        """
        Handle the payment_intent.succeeded webhook from Stripe
        """
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)

    def handle_payment_intent_payment_failed(self, event):
        """
        Handle the payment_intent.payment_failed webhook from Stripe
        """
        return HttpResponse(
            content=f'Payment Failed Webhook received: {event["type"]}',
            status=200)

```
- test Webhook on Stripe
  - account.external_account.created
    Response
    Test webhook sent successfully
    Unhandled webhook received: account.updated
  - payment_intend.succeeded:
    Response
    Test webhook sent successfully
    Webhook received: payment_intent.succeeded
  - payment_intend.payment_failed
    Response
    Test webhook sent successfully
    Payment Failed Webhook received: payment_intent.payment_failed
- git add . 
- git commit -m "Added class methods to webhook handler and webhook view"
- git push


### Stripe Part 13

checkout/static/checkout/js/stripe_elements.js
```
/*
    Core logic/payment flow for this comes from here:
    https://stripe.com/docs/payments/accept-a-payment
    CSS from here: 
    https://stripe.com/docs/stripe-js
*/

var stripePublicKey = $('#id_stripe_public_key').text().slice(1, -1);
var clientSecret = $('#id_client_secret').text().slice(1, -1);
var stripe = Stripe(stripePublicKey);
var elements = stripe.elements();
var style = {
    base: {
        color: '#000',
        fontFamily: '"Helvetica Neue", Helvetica, sans-serif',
        fontSmoothing: 'antialiased',
        fontSize: '16px',
        '::placeholder': {
            color: '#aab7c4'
        }
    },
    invalid: {
        color: '#dc3545',
        iconColor: '#dc3545'
    }
};
var card = elements.create('card', {style: style});
card.mount('#card-element');

// Handle realtime validation errors on the card element
card.addEventListener('change', function (event) {
    var errorDiv = document.getElementById('card-errors');
    if (event.error) {
        var html = `
            <span class="icon" role="alert">
                <i class="fas fa-times"></i>
            </span>
            <span>${event.error.message}</span>
        `;
        $(errorDiv).html(html);
    } else {
        errorDiv.textContent = '';
    }
});

// Handle form submit
var form = document.getElementById('payment-form');

form.addEventListener('submit', function(ev) {
    ev.preventDefault();
    card.update({ 'disabled': true});
    $('#submit-button').attr('disabled', true);
    $('#payment-form').fadeToggle(100);
    $('#loading-overlay').fadeToggle(100);

    var saveInfo = Boolean($('#id-save-info').attr('checked'));
    // From using {% csrf_token %} in the form
    var csrfToken = $('input[name="csrfmiddlewaretoken"]').val();
    var postData = {
        'csrfmiddlewaretoken': csrfToken,
        'client_secret': clientSecret,
        'save_info': saveInfo,
    };
    var url = '/checkout/cache_checkout_data/';

    $.post(url, postData).done(function () {
        stripe.confirmCardPayment(clientSecret, {
            payment_method: {
                card: card,
                billing_details: {
                    name: $.trim(form.full_name.value),
                    phone: $.trim(form.phone_number.value),
                    email: $.trim(form.email.value),
                    address:{
                        line1: $.trim(form.street_address1.value),
                        line2: $.trim(form.street_address2.value),
                        city: $.trim(form.town_or_city.value),
                        country: $.trim(form.country.value),
                        state: $.trim(form.county.value),
                    }
                }
            },
            shipping: {
                name: $.trim(form.full_name.value),
                phone: $.trim(form.phone_number.value),
                address: {
                    line1: $.trim(form.street_address1.value),
                    line2: $.trim(form.street_address2.value),
                    city: $.trim(form.town_or_city.value),
                    country: $.trim(form.country.value),
                    postal_code: $.trim(form.postcode.value),
                    state: $.trim(form.county.value),
                }
            },
        }).then(function(result) {
            if (result.error) {
                var errorDiv = document.getElementById('card-errors');
                var html = `
                    <span class="icon" role="alert">
                    <i class="fas fa-times"></i>
                    </span>
                    <span>${result.error.message}</span>`;
                $(errorDiv).html(html);
                $('#payment-form').fadeToggle(100);
                $('#loading-overlay').fadeToggle(100);
                card.update({ 'disabled': false});
                $('#submit-button').attr('disabled', false);
            } else {
                if (result.paymentIntent.status === 'succeeded') {
                    form.submit();
                }
            }
        });
    }).fail(function () {
        // just reload the page, the error will be in django messages
        location.reload();
    })
});
```

checkout/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404, HttpResponse
from django.views.decorators.http import require_POST
from django.contrib import messages
from django.conf import settings

from .forms import OrderForm
from .models import Order, OrderLineItem
from products.models import Product
from bag.contexts import bag_contents

import stripe
import json

@require_POST
def cache_checkout_data(request):
    try:
        pid = request.POST.get('client_secret').split('_secret')[0]
        stripe.api_key = settings.STRIPE_SECRET_KEY
        stripe.PaymentIntent.modify(pid, metadata={
            'bag': json.dumps(request.session.get('bag', {})),
            'save_info': request.POST.get('save_info'),
            'username': request.user,
        })
        return HttpResponse(status=200)
    except Exception as e:
        messages.error(request, 'Sorry, your payment cannot be \
            processed right now. Please try again later.')
        return HttpResponse(content=e, status=400)
```

### Stripe Part 14

checkout/urls.py 
```
from django.urls import path
from . import views
from .webhooks import webhook

urlpatterns = [
    path('', views.checkout, name='checkout'),
    path('checkout_success/<order_number>', views.checkout_success, name='checkout_success'),
    path('cache_checkout_data/', views.cache_checkout_data, name='cache_checkout_data'),
    path('wh/', webhook, name='webhook'),
]

```

checkout/webhook_handler.py
```
from django.http import HttpResponse


class StripeWH_Handler:
    """Handle Stripe webhooks"""

    def __init__(self, request):
        self.request = request

    def handle_event(self, event):
        """
        Handle a generic/unknown/unexpected webhook event
        """
        return HttpResponse(
            content=f'Unhandled webhook received: {event["type"]}',
            status=200)

    def handle_payment_intent_succeeded(self, event):
        """
        Handle the payment_intent.succeeded webhook from Stripe
        """
        intent = event.data.object
        print(intent)
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)

    def handle_payment_intent_payment_failed(self, event):
        """
        Handle the payment_intent.payment_failed webhook from Stripe
        """
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)

```
- git add . 
- git commit -m "Added checkout form data caching in payment intent"
- git push

### Stripe Part 15, 16, 17

checkout/webhook_handler.py 
```
from django.http import HttpResponse

from .models import Order, OrderLineItem
from products.models import Product

import json
import time

class StripeWH_Handler:
    """Handle Stripe webhooks"""

    def __init__(self, request):
        self.request = request

    def handle_event(self, event):
        """
        Handle a generic/unknown/unexpected webhook event
        """
        return HttpResponse(
            content=f'Unhandled webhook received: {event["type"]}',
            status=200)

    def handle_payment_intent_succeeded(self, event):
        """
        Handle the payment_intent.succeeded webhook from Stripe
        """
        intent = event.data.object
        pid = intent.id
        bag = intent.metadata.bag
        save_info = intent.metadata.save_info

        billing_details = intent.charges.data[0].billing_details
        shipping_details = intent.shipping
        grand_total = round(intent.charges.data[0].amount / 100, 2)

        # Clean data in the shipping details
        for field, value in shipping_details.address.items():
            if value == "":
                shipping_details.address[field] = None

        order_exists = False
        attempt = 1
        while attempt <= 5:
            try:
                order = Order.objects.get(
                    full_name__iexact=shipping_details.name,
                    email__iexact=billing_details.email,
                    phone_number__iexact=shipping_details.phone,
                    country__iexact=shipping_details.address.country,
                    postcode__iexact=shipping_details.address.postal_code,
                    town_or_city__iexact=shipping_details.address.city,
                    street_address1__iexact=shipping_details.address.line1,
                    street_address2__iexact=shipping_details.address.line2,
                    county__iexact=shipping_details.address.state,
                    grand_total=grand_total,
                    original_bag=bag,
                    stripe_pid=pid,
                )
                order_exists = True
                break
            except Order.DoesNotExist:
                attempt += 1
                time.sleep(1)
        if order_exists:
            return HttpResponse(
                content=f'Webhook received: {event["type"]} | SUCCESS: Verified order already in database',
                status=200)
        else:
            order = None
            try:
                order = Order.objects.create(
                    full_name=shipping_details.name,
                    email=billing_details.email,
                    phone_number=shipping_details.phone,
                    country=shipping_details.address.country,
                    postcode=shipping_details.address.postal_code,
                    town_or_city=shipping_details.address.city,
                    street_address1=shipping_details.address.line1,
                    street_address2=shipping_details.address.line2,
                    county=shipping_details.address.state,
                    original_bag=bag,
                    stripe_pid=pid,
                )
                for item_id, item_data in json.loads(bag).items():
                    product = Product.objects.get(id=item_id)
                    if isinstance(item_data, int):
                        order_line_item = OrderLineItem(
                            order=order,
                            product=product,
                            quantity=item_data,
                        )
                        order_line_item.save()
                    else:
                        for size, quantity in item_data['items_by_size'].items():
                            order_line_item = OrderLineItem(
                                order=order,
                                product=product,
                                quantity=quantity,
                                product_size=size,
                            )
                            order_line_item.save()
            except Exception as e:
                if order:
                    order.delete()
                return HttpResponse(
                    content=f'Webhook received: {event["type"]} | ERROR: {e}',
                    status=500)
        return HttpResponse(
            content=f'Webhook received: {event["type"]} | SUCCESS: Created order in webhook',
            status=200)

    def handle_payment_intent_payment_failed(self, event):
        """
        Handle the payment_intent.payment_failed webhook from Stripe
        """
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)

```

checkout/models.py
```
import uuid

from django.db import models
from django.db.models import Sum
from django.conf import settings

from products.models import Product


class Order(models.Model):
    order_number = models.CharField(max_length=32, null=False, editable=False)
    full_name = models.CharField(max_length=50, null=False, blank=False)
    email = models.EmailField(max_length=254, null=False, blank=False)
    phone_number = models.CharField(max_length=20, null=False, blank=False)
    country = models.CharField(max_length=40, null=False, blank=False)
    postcode = models.CharField(max_length=20, null=True, blank=True)
    town_or_city = models.CharField(max_length=40, null=False, blank=False)
    street_address1 = models.CharField(max_length=80, null=False, blank=False)
    street_address2 = models.CharField(max_length=80, null=True, blank=True)
    county = models.CharField(max_length=80, null=True, blank=True)
    date = models.DateTimeField(auto_now_add=True)
    delivery_cost = models.DecimalField(max_digits=6, decimal_places=2, null=False, default=0)
    order_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)
    grand_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)
    original_bag = models.TextField(null=False, blank=False, default='')
    stripe_pid = models.CharField(max_length=254, null=False, blank=False, default='')

    def _generate_order_number(self):
        """
        Generate a random, unique order number using UUID
        """
        return uuid.uuid4().hex.upper()

    def update_total(self):
        """
        Update grand total each time a line item is added,
        accounting for delivery costs.
        """
        self.order_total = self.lineitems.aggregate(Sum('lineitem_total'))['lineitem_total__sum'] or 0
        if self.order_total < settings.FREE_DELIVERY_THRESHOLD:
            self.delivery_cost = self.order_total * settings.STANDARD_DELIVERY_PERCENTAGE / 100
        else:
            self.delivery_cost = 0
        self.grand_total = self.order_total + self.delivery_cost
        self.save()

    def save(self, *args, **kwargs):
        """
        Override the original save method to set the order number
        if it hasn't been set already.
        """
        if not self.order_number:
            self.order_number = self._generate_order_number()
        super().save(*args, **kwargs)

    def __str__(self):
        return self.order_number


class OrderLineItem(models.Model):
    order = models.ForeignKey(Order, null=False, blank=False, on_delete=models.CASCADE, related_name='lineitems')
    product = models.ForeignKey(Product, null=False, blank=False, on_delete=models.CASCADE)
    product_size = models.CharField(max_length=2, null=True, blank=True) # XS, S, M, L, XL
    quantity = models.IntegerField(null=False, blank=False, default=0)
    lineitem_total = models.DecimalField(max_digits=6, decimal_places=2, null=False, blank=False, editable=False)

    def save(self, *args, **kwargs):
        """
        Override the original save method to set the lineitem total
        and update the order total.
        """
        self.lineitem_total = self.product.price * self.quantity
        super().save(*args, **kwargs)

    def __str__(self):
        return f'SKU {self.product.sku} on order {self.order.order_number}'

```
- python3 manage.py makemigrations --dry-run
- python3 manage.py makemigrations
- python3 manage.py migrate --plan
- python3 manage.py migrate

checkout/admin.py
```
from django.contrib import admin
from .models import Order, OrderLineItem


class OrderLineItemAdminInline(admin.TabularInline):
    model = OrderLineItem
    readonly_fields = ('lineitem_total',)


class OrderAdmin(admin.ModelAdmin):
    inlines = (OrderLineItemAdminInline,)

    readonly_fields = ('order_number', 'date',
                       'delivery_cost', 'order_total',
                       'grand_total', 'original_bag',
                       'stripe_pid')

    fields = ('order_number', 'date', 'full_name',
              'email', 'phone_number', 'country',
              'postcode', 'town_or_city', 'street_address1',
              'street_address2', 'county', 'delivery_cost',
              'order_total', 'grand_total', 'original_bag',
              'stripe_pid')

    list_display = ('order_number', 'date', 'full_name',
                    'order_total', 'delivery_cost',
                    'grand_total',)

    ordering = ('-date',)


admin.site.register(Order, OrderAdmin)

```

checkout/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404, HttpResponse
from django.views.decorators.http import require_POST
from django.contrib import messages
from django.conf import settings

from .forms import OrderForm
from .models import Order, OrderLineItem
from products.models import Product
from bag.contexts import bag_contents

import stripe
import json

@require_POST
def cache_checkout_data(request):
    try:
        pid = request.POST.get('client_secret').split('_secret')[0]
        stripe.api_key = settings.STRIPE_SECRET_KEY
        stripe.PaymentIntent.modify(pid, metadata={
            'bag': json.dumps(request.session.get('bag', {})),
            'save_info': request.POST.get('save_info'),
            'username': request.user,
        })
        return HttpResponse(status=200)
    except Exception as e:
        messages.error(request, 'Sorry, your payment cannot be \
            processed right now. Please try again later.')
        return HttpResponse(content=e, status=400)


def checkout(request):
    stripe_public_key = settings.STRIPE_PUBLIC_KEY
    stripe_secret_key = settings.STRIPE_SECRET_KEY

    if request.method == 'POST':
        bag = request.session.get('bag', {})

        form_data = {
            'full_name': request.POST['full_name'],
            'email': request.POST['email'],
            'phone_number': request.POST['phone_number'],
            'country': request.POST['country'],
            'postcode': request.POST['postcode'],
            'town_or_city': request.POST['town_or_city'],
            'street_address1': request.POST['street_address1'],
            'street_address2': request.POST['street_address2'],
            'county': request.POST['county'],
        }
        order_form = OrderForm(form_data)
        if order_form.is_valid():
            order = order_form.save(commit=False)
            pid = request.POST.get('client_secret').split('_secret')[0]
            order.stripe_pid = pid
            order.original_bag = json.dumps(bag)
            order.save()
            for item_id, item_data in bag.items():
                try:
                    product = Product.objects.get(id=item_id)
                    if isinstance(item_data, int):
                        order_line_item = OrderLineItem(
                            order=order,
                            product=product,
                            quantity=item_data,
                        )
                        order_line_item.save()
                    else:
                        for size, quantity in item_data['items_by_size'].items():
                            order_line_item = OrderLineItem(
                                order=order,
                                product=product,
                                quantity=quantity,
                                product_size=size,
                            )
                            order_line_item.save()
                except Product.DoesNotExist:
                    messages.error(request, (
                        "One of the products in your bag wasn't found in our database. "
                        "Please call us for assistance!")
                    )
                    order.delete()
                    return redirect(reverse('view_bag'))

            request.session['save_info'] = 'save-info' in request.POST
            return redirect(reverse('checkout_success', args=[order.order_number]))
        else:
            messages.error(request, 'There was an error with your form. \
                Please double check your information.')
    else:
        bag = request.session.get('bag', {})
        if not bag:
            messages.error(request, "There's nothing in your bag at the moment")
            return redirect(reverse('products'))

        current_bag = bag_contents(request)
        total = current_bag['grand_total']
        stripe_total = round(total * 100)
        stripe.api_key = stripe_secret_key
        intent = stripe.PaymentIntent.create(
            amount=stripe_total,
            currency=settings.STRIPE_CURRENCY,
        )

        order_form = OrderForm()

    if not stripe_public_key:
        messages.warning(request, 'Stripe public key is missing. \
            Did you forget to set it in your environment?')

    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
        'stripe_public_key': stripe_public_key,
        'client_secret': intent.client_secret,
    }

    return render(request, template, context)


def checkout_success(request, order_number):
    """
    Handle successful checkouts
    """
    save_info = request.session.get('save_info')
    order = get_object_or_404(Order, order_number=order_number)
    messages.success(request, f'Order successfully processed! \
        Your order number is {order_number}. A confirmation \
        email will be sent to {order.email}.')

    if 'bag' in request.session:
        del request.session['bag']

    template = 'checkout/checkout_success.html'
    context = {
        'order': order,
    }

    return render(request, template, context)

```

checkout/templates/checkout/checkout.html
```
<fieldset class="px-3">
    <legend class="fieldset-label small text-black px-2 w-auto">Payment</legend>
    <!-- A Stripe card element will go here -->
    <div class="mb-3" id="card-element"></div>
    <!-- Used to display form errors -->
    <div class="mb-3 text-danger" id="card-errors" role="alert"></div>
    <!-- Pass the client secret to the view so we can get the payment intent id -->
    <input type="hidden" value="{{ client_secret }}" name="client_secret">
</fieldset>
```
- git add . 
- git commit -m "completed webhook handler"
- git push


### Profile App - Part 1

checkout/templates/checkout/checkout.html
```
{{ order_form.phone_number | as_crispy_field }}
{{ order_form.street_address1 | as_crispy_field }}
{{ order_form.street_address2 | as_crispy_field }}
{{ order_form.town_or_city | as_crispy_field }}
{{ order_form.county | as_crispy_field }}
{{ order_form.postcode | as_crispy_field }}
{{ order_form.country | as_crispy_field }}
```

checkout/forms.py
```
'full_name': 'Full Name',
'email': 'Email Address',
'phone_number': 'Phone Number',
'postcode': 'Postal Code',
'town_or_city': 'Town or City',
'street_address1': 'Street Address 1',
'street_address2': 'Street Address 2',
'county': 'County, State or Locality',
```
- pip3 install django-countries
- pip3 freeze --local > requirements.txt

checkout/models.py
```
import uuid

from django.db import models
from django.db.models import Sum
from django.conf import settings

from django_countries.fields import CountryField

from products.models import Product


class Order(models.Model):
    order_number = models.CharField(max_length=32, null=False, editable=False)
    full_name = models.CharField(max_length=50, null=False, blank=False)
    email = models.EmailField(max_length=254, null=False, blank=False)
    phone_number = models.CharField(max_length=20, null=False, blank=False)
    country = CountryField(blank_label='Country *', null=False, blank=False)
    postcode = models.CharField(max_length=20, null=True, blank=True)
    town_or_city = models.CharField(max_length=40, null=False, blank=False)
    street_address1 = models.CharField(max_length=80, null=False, blank=False)
    street_address2 = models.CharField(max_length=80, null=True, blank=True)
    county = models.CharField(max_length=80, null=True, blank=True)
    date = models.DateTimeField(auto_now_add=True)
    delivery_cost = models.DecimalField(max_digits=6, decimal_places=2, null=False, default=0)
    order_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)
    grand_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)
    original_bag = models.TextField(null=False, blank=False, default='')
    stripe_pid = models.CharField(max_length=254, null=False, blank=False, default='')
```
- python3 manage.py makemigrations --dry-run
- python3 manage.py makemigrations
- python3 manage.py migrate --plan
- python3 manage.py migrate
- git add . 
- git commit -m "completed webhook handler"
- git push


### Profile App - Part 2

- python3 manage.py startapp profiles

profiles/models.py
```
from django.db import models
from django.contrib.auth.models import User
from django.db.models.signals import post_save
from django.dispatch import receiver

from django_countries.fields import CountryField


class UserProfile(models.Model):
    """
    A user profile model for maintaining default
    delivery information and order history
    """
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    default_phone_number = models.CharField(max_length=20, null=True, blank=True)
    default_country = CountryField(blank_label='Country *', null=True, blank=True)
    default_postcode = models.CharField(max_length=20, null=True, blank=True)
    default_town_or_city = models.CharField(max_length=40, null=True, blank=True)
    default_street_address1 = models.CharField(max_length=80, null=True, blank=True)
    default_street_address2 = models.CharField(max_length=80, null=True, blank=True)
    default_county = models.CharField(max_length=80, null=True, blank=True)

    def __str__(self):
        return self.user.username


@receiver(post_save, sender=User)
def create_or_update_user_profile(sender, instance, created, **kwargs):
    """
    Create or update the user profile
    """
    if created:
        UserProfile.objects.create(user=instance)
    # Existing users: just save the profile
    instance.userprofile.save()

```

checkout/models.py
```
import uuid

from django.db import models
from django.db.models import Sum
from django.conf import settings

from django_countries.fields import CountryField

from products.models import Product
from profiles.models import UserProfile


class Order(models.Model):
    order_number = models.CharField(max_length=32, null=False, editable=False)
    user_profile = models.ForeignKey(UserProfile, on_delete=models.SET_NULL,
                                     null=True, blank=True, related_name='orders')
    full_name = models.CharField(max_length=50, null=False, blank=False)
    email = models.EmailField(max_length=254, null=False, blank=False)
    phone_number = models.CharField(max_length=20, null=False, blank=False)
    country = CountryField(blank_label='Country *', null=False, blank=False)
    postcode = models.CharField(max_length=20, null=True, blank=True)
    town_or_city = models.CharField(max_length=40, null=False, blank=False)
    street_address1 = models.CharField(max_length=80, null=False, blank=False)
    street_address2 = models.CharField(max_length=80, null=True, blank=True)
    county = models.CharField(max_length=80, null=True, blank=True)
    date = models.DateTimeField(auto_now_add=True)
    delivery_cost = models.DecimalField(max_digits=6, decimal_places=2, null=False, default=0)
    order_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)
    grand_total = models.DecimalField(max_digits=10, decimal_places=2, null=False, default=0)
    original_bag = models.TextField(null=False, blank=False, default='')
    stripe_pid = models.CharField(max_length=254, null=False, blank=False, default='')

    def _generate_order_number(self):
        """
        Generate a random, unique order number using UUID
        """
        return uuid.uuid4().hex.upper()

    def update_total(self):
        """
        Update grand total each time a line item is added,
        accounting for delivery costs.
        """
        self.order_total = self.lineitems.aggregate(Sum('lineitem_total'))['lineitem_total__sum'] or 0
        if self.order_total < settings.FREE_DELIVERY_THRESHOLD:
            self.delivery_cost = self.order_total * settings.STANDARD_DELIVERY_PERCENTAGE / 100
        else:
            self.delivery_cost = 0
        self.grand_total = self.order_total + self.delivery_cost
        self.save()

    def save(self, *args, **kwargs):
        """
        Override the original save method to set the order number
        if it hasn't been set already.
        """
        if not self.order_number:
            self.order_number = self._generate_order_number()
        super().save(*args, **kwargs)

    def __str__(self):
        return self.order_number


class OrderLineItem(models.Model):
    order = models.ForeignKey(Order, null=False, blank=False, on_delete=models.CASCADE, related_name='lineitems')
    product = models.ForeignKey(Product, null=False, blank=False, on_delete=models.CASCADE)
    product_size = models.CharField(max_length=2, null=True, blank=True) # XS, S, M, L, XL
    quantity = models.IntegerField(null=False, blank=False, default=0)
    lineitem_total = models.DecimalField(max_digits=6, decimal_places=2, null=False, blank=False, editable=False)

    def save(self, *args, **kwargs):
        """
        Override the original save method to set the lineitem total
        and update the order total.
        """
        self.lineitem_total = self.product.price * self.quantity
        super().save(*args, **kwargs)

    def __str__(self):
        return f'SKU {self.product.sku} on order {self.order.order_number}'

```

profiles/views.py
```
from django.shortcuts import render


def profile(request):
    """ Display the user's profile. """

    template = 'profiles/profile.html'
    context = {}

    return render(request, template, context)

```

profiles/urls.py
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.profile, name='profile')
]

```

profiles/templates/profiles/profile.html
```
{% extends "base.html" %}
{% load static %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'profiles/css/profile.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">My Profile</h2>
                <hr>
            </div>
        </div>
{% endblock %}
```

profiles/static/profiles/css/profile.css
```

```

boutique/urls.py
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
    path('bag/', include('bag.urls')),
    path('checkout/', include('checkout.urls')),
    path('profile/', include('profiles.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

```

boutique/settings.py
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
    'bag',
    'checkout',
    'profiles',

    # Other
    'crispy_forms',
]
```
- python3 manage.py makemigrations --dry-run
- python3 manage.py makemigrations
- python3 manage.py migrate --plan
- python3 manage.py migrate
- git add . 
- git commit -m "Added profiles app"
- git push


### Profile App - Part 3

templates/allauth/account/base.html
```
{% extends "base.html" %}

{% block content %}
    <div class="container header-container">
        <div class="overlay"></div>
        <div class="row">
            <div class="col-12 col-md-6">
                <div class="allauth-form-inner-content">
                    {% block inner_content %}
                    {% endblock %}
                </div>
            </div>
        </div>
    </div>
{% endblock %}
```

templates/allauth/account/login.html
```
{% extends "account/base.html" %}

{% load i18n %}
{% load account socialaccount %}

{% block head_title %}{% trans "Sign In" %}{% endblock %}

{% block inner_content %}

<hr>
<h2 class="logo-font mb-4">{% trans "Sign In" %}</h2>
<hr>

{% get_providers as socialaccount_providers %}

{% if socialaccount_providers %}
<p>{% blocktrans with site.name as site_name %}Please sign in with one
of your existing third party accounts. Or, <a href="{{ signup_url }}">sign up</a>
for a {{ site_name }} account and sign in below:{% endblocktrans %}</p>

<div class="socialaccount_ballot">

  <ul class="socialaccount_providers">
    {% include "socialaccount/snippets/provider_list.html" with process="login" %}
  </ul>

  <div class="login-or">{% trans 'or' %}</div>

</div>

{% include "socialaccount/snippets/login_extra.html" %}

{% else %}
<p>{% blocktrans %}If you have not created an account yet, then please
<a href="{{ signup_url }}">sign up</a> first.{% endblocktrans %}</p>
{% endif %}

<form class="login" method="POST" action="{% url 'account_login' %}">
  {% csrf_token %}
  {{ form|crispy }}
  {% if redirect_field_value %}
  <input type="hidden" name="{{ redirect_field_name }}" value="{{ redirect_field_value }}" />
  {% endif %}
  <a class="btn btn-outline-black rounded-0" href="{% url 'home' %}">Home</a>
  <button class="primaryAction" type="submit">{% trans "Sign In" %}</button>
  <p class="mt-2">
    <a class="button secondaryAction" href="{% url 'account_reset_password' %}">{% trans "Forgot Password?" %}</a>
  </p>
</form>

{% endblock %}
```
- update all .html files in account folder


### Profile App - Part 4

static/css/base.css
```
html {
    height: 100%;
}

body {
    background: url('/media/homepage_background_cropped.jpg') no-repeat center center fixed;
    background-size: cover;
    height: calc(100vh - 164px);
    color: #555;
    font-family: 'Lato';
}

/* from Bulma */
.icon {
    align-items: center;
    display: inline-flex;
    justify-content: center;
    height: 1.5rem;
    width: 1.5rem;
}

.logo-font {
    text-transform: uppercase;
}

.main-logo-link {
    width: fit-content;
}

.shop-now-button {
    background: black;
    color: white;
    min-width: 260px;
}

.btn-black {
    background: black;
    color: white;
}

.btn-outline-black {
    background: white;
    color: black !important; /* use important to override link colors for <a> elements */
    border: 1px solid black;
}

.btn-outline-black:hover,
.btn-outline-black:active,
.btn-outline-black:focus {
    background: black;
    color: white !important;
}

.shop-now-button:hover,
.shop-now-button:active,
.shop-now-button:focus,
.btn-black:hover,
.btn-black:active,
.btn-black:focus {
    background: #222;
    color: white;
}

.text-black {
    color: #000 !important;
}

.border-black {
    border: 1px solid black !important;
}

.bg-black {
    background: #000 !important;
}

.overlay {
	height: 100%;
	width: 100%;
	top: 0;
	left: 0;
	position: fixed;
	background: white;
	z-index: -1;
}

a.category-badge > span.badge:hover {
    background: #212529 !important;
    color: #fff !important;
}

.btt-button {
    height: 42px;
    width: 42px;
    position: fixed;
    bottom: 10px;
    right: 10px;
}

.btt-link,
.update-link,
.remove-item {
    cursor: pointer;
}

input[name='q']::placeholder {
    color: #aab7c4;
}

/* ------------------------------- bootstrap toasts */

.message-container {
    position: fixed;
    top: 72px;
    right: 15px;
    z-index: 99999999999;
}

.custom-toast {
    overflow: visible;
}

.toast-capper {
    height: 2px;
}

/* from CSS-tricks.com: https://css-tricks.com/snippets/css/css-triangle/ */
.arrow-up {
    width: 0; 
    height: 0; 
    border-left: 4px solid transparent;
    border-right: 4px solid transparent; 
    border-bottom: 10px solid black;
    position: absolute;
    top: -10px;
    right: 36px;
}

/* Convenience classes - colors copied from Bootstrap */
.arrow-primary {
    border-bottom-color: #007bff !important;
}

.arrow-secondary {
    border-bottom-color: #6c757d !important;
}

.arrow-success {
    border-bottom-color: #28a745 !important;
}

.arrow-danger {
    border-bottom-color: #dc3545 !important;
}

.arrow-warning {
    border-bottom-color: #ffc107 !important;
}

.arrow-info {
    border-bottom-color: #17a2b8 !important;
}

.arrow-light {
    border-bottom-color: #f8f9fa !important;
}

.arrow-dark {
    border-bottom-color: #343a40 !important;
}

.bag-notification-wrapper {
    height: 100px;
    overflow-x: hidden;
    overflow-y: auto;
}

/* -------------------------------- Media Queries */

/* Slightly larger container on xl screens */
@media (min-width: 1200px) {
  .container {
    max-width: 80%;
  }
}

/* Allauth form formatting */

.allauth-form-inner-content p {
    margin-top: 1.5rem; /* mt-4 */
    color: #6c757d; /* text-secondary */
}

.allauth-form-inner-content input {
    border-color: #000;
    border-radius: 0;
}

.allauth-form-inner-content label:not([for='id_remember']) {
    display: none;
}

.allauth-form-inner-content input::placeholder {
    color: #aab7c4;
}

.allauth-form-inner-content button,
.allauth-form-inner-content input[type='submit'] {
	/* btn */
	display: inline-block;
    font-weight: 400;
    color: #fff;
    text-align: center;
    vertical-align: middle;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    background-color: #000;
    border: 1px solid #000;
    padding: .375rem .75rem;
    font-size: 1rem;
    line-height: 1.5;
    border-radius: 0;

    /* standard bootstrap btn transitions */
    transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
}

.allauth-form-inner-content button:hover,
.allauth-form-inner-content input[type='submit']:hover {	
	color: #fff;
    background-color: #222;
    border-color: #222;
}

.allauth-form-inner-content a {
	color: #17a2b8; /* text-info */
}

/* fixed top navbar only on medium and up */
@media (min-width: 992px) {
    .fixed-top-desktop-only {
        position: fixed;
        top: 0;
        right: 0;
        left: 0;
        z-index: 1030;
    }

    .header-container {
        padding-top: 164px;
    }
}

/* pad the top a bit when navbar is collapsed on mobile */
@media (max-width: 991px) {
    .header-container {
        padding-top: 116px;
    }

    body {
        height: calc(100vh - 116px);
    }
}
```

- test register
  - sign in
  - get "confirm email" from terminal:
    http://localhost:8000/accounts/confirm-email/...../
    - take /accounts/confirm-email/...../
    - and add at the end of https://8000-green-elephant-3el2rqbx.ws-eu16.gitpod.io
      https://8000-green-elephant-3el2rqbx.ws-eu16.gitpod.io/accounts/confirm-email/...../
  - confirm email
  - sign in

templates/base.html
```
{% load static %}

<!doctype html>
<html lang="en">
  <head>

    {% block meta %}
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    {% endblock %}

    {% block extra_meta %}
    {% endblock %}

    {% block corecss %}
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
        <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Lato&display=swap">
        <link rel="stylesheet" href="{% static 'css/base.css' %}">
    {% endblock %}

    {% block extra_css %}
    {% endblock %}

    {% block corejs %}
        <script src="https://kit.fontawesome.com/e9c73d7092.js" crossorigin="anonymous"></script>
        <script src="https://code.jquery.com/jquery-3.4.1.min.js" integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo=" crossorigin="anonymous"></script>
        <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
        <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
        <!-- Stripe -->
        <script src="https://js.stripe.com/v3/"></script>
    {% endblock %}

    {% block extra_js %}
    {% endblock %}

    <title>Boutique Ado {% block extra_title %}{% endblock %}</title>
  </head>
  <body>
    <header class="container-fluid fixed-top">
        <div id="topnav" class="row bg-white pt-lg-2 d-none d-lg-flex">
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0 text-center text-lg-left">
                <a href="{% url 'home' %}" class="nav-link main-logo-link">
                    <h2 class="logo-font text-black my-0"><strong>Boutique</strong> Ado</h2>
                </a>
            </div>
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0">
                <form method="GET" action="{% url 'products' %}">
                    <div class="input-group w-100">
                        <input class="form-control border border-black rounded-0" type="text" name="q" placeholder="Search our site">
                        <div class="input-group-append">
                            <button class="form-control btn btn-black border border-black rounded-0" type="submit">
                                <span class="icon">
                                    <i class="fas fa-search"></i>
                                </span>
                            </button>
                        </div>
                    </div>
                </form>
            </div>
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0">
                <ul class="list-inline list-unstyled text-center text-lg-right my-0">
                    <li class="list-inline-item dropdown">
                        <a class="text-black nav-link" href="#" id="user-options" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                            <div class="text-center">
                                <div><i class="fas fa-user fa-lg"></i></div>
                                <p class="my-0">My Account</p>
                            </div>
                        </a>
                        <div class="dropdown-menu border-0" aria-labelledby="user-options">
                            {% if request.user.is_authenticated %}
                                {% if request.user.is_superuser %}
                                    <a href="" class="dropdown-item">Product Management</a>
                                {% endif %}
                                <a href="{% url 'profile' %}" class="dropdown-item">My Profile</a>
                                <a href="{% url 'account_logout' %}" class="dropdown-item">Logout</a>
                            {% else %}
                                <a href="{% url 'account_signup' %}" class="dropdown-item">Register</a>
                                <a href="{% url 'account_login' %}" class="dropdown-item">Login</a>
                            {% endif %}
                        </div>
                    </li>
                    <li class="list-inline-item">
                        <a class="{% if grand_total %}text-info font-weight-bold{% else %}text-black{% endif %} nav-link" href="{% url 'view_bag' %}">
                            <div class="text-center">
                                <div><i class="fas fa-shopping-bag fa-lg"></i></div>
                                <p class="my-0">
                                    {% if grand_total %}
                                        ${{ grand_total|floatformat:2 }}
                                    {% else %}
                                        $0.00
                                    {% endif %}
                                </p>
                            </div>
                        </a>
                    </li>
                </ul>
            </div>
        </div>
        <div class="row bg-white">
            <nav class="navbar navbar-expand-lg navbar-light w-100">
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#main-nav" aria-controls="main-nav" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                {% include 'includes/mobile-top-header.html' %}
                {% include 'includes/main-nav.html' %}
            </nav>
        </div>
        <div id="delivery-banner" class="row text-center">
            <div class="col bg-black text-white">
                <h4 class="logo-font my-1">Free delivery on orders over ${{ free_delivery_threshold }}!</h4>                
            </div>            
        </div>
    </header>

    {% if messages %}
        <div class="message-container">
            {% for message in messages %}
                {% with message.level as level %}
                    {% if level == 40 %}
                        {% include 'includes/toasts/toast_error.html' %}
                    {% elif level == 30 %}
                        {% include 'includes/toasts/toast_warning.html' %}
                    {% elif level == 25 %}
                        {% include 'includes/toasts/toast_success.html' %}
                    {% else %}
                        {% include 'includes/toasts/toast_info.html' %}
                    {% endif %}
                {% endwith %}
            {% endfor %}
        </div>
    {% endif %}

    {% block page_header %}
    {% endblock %}

    {% block content %}
    {% endblock %}

    {% block postloadjs %}
    <script type="text/javascript">
        $('.toast').toast('show');
    </script>
    {% endblock %}

    
  </body>
</html>
```

profiles/views.py
```
from django.shortcuts import render, get_object_or_404

from .models import UserProfile


def profile(request):
    """ Display the user's profile. """
    profile = get_object_or_404(UserProfile, user=request.user)

    template = 'profiles/profile.html'
    context = {
        'profile': profile,
    }

    return render(request, template, context)

```

profiles/templates/profiles/profile.html
```
{% extends "base.html" %}
{% load static %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'profiles/css/profile.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">My Profile</h2>
                <hr>
            </div>
        </div>
        {{ profile }}
{% endblock %}
```
- git add . 
- git commit -m "Updated allauth templates and tested profiles"
- git push


### Profile App - Part 5

profiles/forms.py
```
from django import forms
from .models import UserProfile


class UserProfileForm(forms.ModelForm):
    class Meta:
        model = UserProfile
        exclude = ('user',)

    def __init__(self, *args, **kwargs):
        """
        Add placeholders and classes, remove auto-generated
        labels and set autofocus on first field
        """
        super().__init__(*args, **kwargs)
        placeholders = {
            'default_phone_number': 'Phone Number',
            'default_postcode': 'Postal Code',
            'default_town_or_city': 'Town or City',
            'default_street_address1': 'Street Address 1',
            'default_street_address2': 'Street Address 2',
            'default_county': 'County, State or Locality',
        }

        self.fields['default_phone_number'].widget.attrs['autofocus'] = True
        for field in self.fields:
            if field != 'default_country':
                if self.fields[field].required:
                    placeholder = f'{placeholders[field]} *'
                else:
                    placeholder = placeholders[field]
                self.fields[field].widget.attrs['placeholder'] = placeholder
            self.fields[field].widget.attrs['class'] = 'border-black rounded-0 profile-form-input'
            self.fields[field].label = False

```

profiles/views.py
```
from django.shortcuts import render, get_object_or_404
from django.contrib import messages

from .models import UserProfile
from .forms import UserProfileForm


def profile(request):
    """ Display the user's profile. """
    profile = get_object_or_404(UserProfile, user=request.user)

    if request.method == 'POST':
        form = UserProfileForm(request.POST, instance=profile)
        if form.is_valid():
            form.save()
            messages.success(request, 'Profile updated successfully')

    form = UserProfileForm(instance=profile)
    orders = profile.orders.all()

    template = 'profiles/profile.html'
    context = {
        'form': form,
        'orders': orders,
        'on_profile_page': True
    }

    return render(request, template, context)

```
profiles/templates/profiles/profile.html
```
{% extends "base.html" %}
{% load static %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'profiles/css/profile.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">My Profile</h2>
                <hr>
            </div>
        </div>
        <div class="row">
            <div class="col-12 col-lg-6">
                <p class="text-muted">Default Delivery Information</p>
                <form class="mt-3" action="{% url 'profile' %}" method="POST" id="profile-update-form">
                    {% csrf_token %}
                    {{ form|crispy }}
                    <button class="btn btn-black rounded-0 text-uppercase float-right">Update Information</button>
                </form>
            </div>
            <div class="col-12 col-lg-6">
                <p class="text-muted">Order History</p>
                {{ orders }}
            </div>
        </div>
{% endblock %}

{% block postloadjs %}
    {{ block.super }}
    <script type="text/javascript" src="{% static 'profiles/js/countryfield.js' %}"></script>
{% endblock %}
```

profiles/models.py
```
from django.db import models
from django.contrib.auth.models import User
from django.db.models.signals import post_save
from django.dispatch import receiver

from django_countries.fields import CountryField


class UserProfile(models.Model):
    """
    A user profile model for maintaining default
    delivery information and order history
    """
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    default_phone_number = models.CharField(max_length=20, null=True, blank=True)
    default_street_address1 = models.CharField(max_length=80, null=True, blank=True)
    default_street_address2 = models.CharField(max_length=80, null=True, blank=True)
    default_town_or_city = models.CharField(max_length=40, null=True, blank=True)
    default_county = models.CharField(max_length=80, null=True, blank=True)
    default_postcode = models.CharField(max_length=20, null=True, blank=True)
    default_country = CountryField(blank_label='Country', null=True, blank=True)

    def __str__(self):
        return self.user.username


@receiver(post_save, sender=User)
def create_or_update_user_profile(sender, instance, created, **kwargs):
    """
    Create or update the user profile
    """
    if created:
        UserProfile.objects.create(user=instance)
    # Existing users: just save the profile
    instance.userprofile.save()

```

profiles/static/profiles/css/profile.css
```
#profile-update-form .form-control {
    color: #000;
}

#profile-update-form input::placeholder {
    color: #aab7c4;
}

#id_default_country,
#id_default_country option:not(:first-child) {
    color: #000;
}

#id_default_country option:first-child {
    color: #aab7c4;
}
```

### Profile App - Part 6

profiles/static/profiles/js/countryfield.js
```
let countrySelected = $('#id_default_country').val();
if(!countrySelected) {
    $('#id_default_country').css('color', '#aab7c4');
};
$('#id_default_country').change(function() {
    countrySelected = $(this).val();
    if(!countrySelected) {
        $(this).css('color', '#aab7c4');
    } else {
        $(this).css('color', '#000');
    }
});
```
- git add . 
- git commit -m "Added profile form, views and updated template"
- git push


### Profile App - Part 7

profiles/templates/profiles/profile.html
```
{% extends "base.html" %}
{% load static %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'profiles/css/profile.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">My Profile</h2>
                <hr>
            </div>
        </div>
        <div class="row">
            <div class="col-12 col-lg-6">
                <p class="text-muted">Default Delivery Information</p>
                <form class="mt-3" action="{% url 'profile' %}" method="POST" id="profile-update-form">
                    {% csrf_token %}
                    {{ form|crispy }}
                    <button class="btn btn-black rounded-0 text-uppercase float-right">Update Information</button>
                </form>
            </div>
            <div class="col-12 col-lg-6">
                <p class="text-muted">Order History</p>
                <div class="order-history table-responsive">
                    <table class="table table-sm table-borderless">
                        <thead>
                            <tr>
                                <th>Order Number</th>
                                <th>Date</th>
                                <th>Items</th>
                                <th>Order Total</th>
                            </tr>
                        </thead>
                        <tbody>
                            {% for order in orders %}
                                <tr>
                                    <td>
                                        <a href="{% url 'order_history' order.order_number %}"
                                        title="{{ order.order_number }}">
                                            {{ order.order_number|truncatechars:6 }}
                                        </a>
                                    </td>
                                    <td>{{ order.date }}</td>
                                    <td>
                                        <ul class="list-unstyled">
                                            {% for item in order.lineitems.all %}
                                                <li class="small">
                                                    {% if item.product.has_sizes %}
                                                        Size {{ item.product.size|upper }}
                                                    {% endif %}{{ item.product.name }} x{{ item.quantity }}
                                                </li>
                                            {% endfor %}
                                        </ul>
                                    </td>
                                    <td>${{ order.grand_total }}</td>
                                </tr>
                            {% endfor %}
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
{% endblock %}

{% block postloadjs %}
    {{ block.super }}
    <script type="text/javascript" src="{% static 'profiles/js/countryfield.js' %}"></script>
{% endblock %}
```

profiles/static/profiles/css/profile.css
```
.order-history {
    max-height: 416px; /* height of profile form + submit button */
    overflow-y: auto;
}

#profile-update-form .form-control {
    color: #000;
}

#profile-update-form input::placeholder {
    color: #aab7c4;
}

#id_default_country,
#id_default_country option:not(:first-child) {
    color: #000;
}

#id_default_country option:first-child {
    color: #aab7c4;
}
```

profiles/views.py
```
from django.shortcuts import render, get_object_or_404
from django.contrib import messages

from .models import UserProfile
from .forms import UserProfileForm

from checkout.models import Order

def profile(request):
    """ Display the user's profile. """
    profile = get_object_or_404(UserProfile, user=request.user)

    if request.method == 'POST':
        form = UserProfileForm(request.POST, instance=profile)
        if form.is_valid():
            form.save()
            messages.success(request, 'Profile updated successfully')

    form = UserProfileForm(instance=profile)
    orders = profile.orders.all()

    template = 'profiles/profile.html'
    context = {
        'form': form,
        'orders': orders,
        'on_profile_page': True
    }

    return render(request, template, context)


def order_history(request, order_number):
    order = get_object_or_404(Order, order_number=order_number)

    messages.info(request, (
        f'This is a past confirmation for order number {order_number}. '
        'A confirmation email was sent on the order date.'
    ))

    template = 'checkout/checkout_success.html'
    context = {
        'order': order,
        'from_profile': True,
    }

    return render(request, template, context)

```

profiles/urls.py
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.profile, name='profile'),
    path('order_history/<order_number>', views.order_history, name='order_history'),
]

```


### Profile App - Part 8

checkout/admin.py
```
from django.contrib import admin
from .models import Order, OrderLineItem


class OrderLineItemAdminInline(admin.TabularInline):
    model = OrderLineItem
    readonly_fields = ('lineitem_total',)


class OrderAdmin(admin.ModelAdmin):
    inlines = (OrderLineItemAdminInline,)

    readonly_fields = ('order_number', 'date',
                       'delivery_cost', 'order_total',
                       'grand_total', 'original_bag',
                       'stripe_pid')

    fields = ('order_number', 'user_profile', 'date', 'full_name',
              'email', 'phone_number', 'country',
              'postcode', 'town_or_city', 'street_address1',
              'street_address2', 'county', 'delivery_cost',
              'order_total', 'grand_total', 'original_bag',
              'stripe_pid')

    list_display = ('order_number', 'date', 'full_name',
                    'order_total', 'delivery_cost',
                    'grand_total',)

    ordering = ('-date',)


admin.site.register(Order, OrderAdmin)

```

checkout/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404, HttpResponse
from django.views.decorators.http import require_POST
from django.contrib import messages
from django.conf import settings

from .forms import OrderForm
from .models import Order, OrderLineItem

from products.models import Product
from profiles.models import UserProfile
from profiles.forms import UserProfileForm
from bag.contexts import bag_contents

import stripe
import json


@require_POST
def cache_checkout_data(request):
    try:
        pid = request.POST.get('client_secret').split('_secret')[0]
        stripe.api_key = settings.STRIPE_SECRET_KEY
        stripe.PaymentIntent.modify(pid, metadata={
            'bag': json.dumps(request.session.get('bag', {})),
            'save_info': request.POST.get('save_info'),
            'username': request.user,
        })
        return HttpResponse(status=200)
    except Exception as e:
        messages.error(request, 'Sorry, your payment cannot be \
            processed right now. Please try again later.')
        return HttpResponse(content=e, status=400)


def checkout(request):
    stripe_public_key = settings.STRIPE_PUBLIC_KEY
    stripe_secret_key = settings.STRIPE_SECRET_KEY

    if request.method == 'POST':
        bag = request.session.get('bag', {})

        form_data = {
            'full_name': request.POST['full_name'],
            'email': request.POST['email'],
            'phone_number': request.POST['phone_number'],
            'country': request.POST['country'],
            'postcode': request.POST['postcode'],
            'town_or_city': request.POST['town_or_city'],
            'street_address1': request.POST['street_address1'],
            'street_address2': request.POST['street_address2'],
            'county': request.POST['county'],
        }

        order_form = OrderForm(form_data)
        if order_form.is_valid():
            order = order_form.save(commit=False)
            pid = request.POST.get('client_secret').split('_secret')[0]
            order.stripe_pid = pid
            order.original_bag = json.dumps(bag)
            order.save()
            for item_id, item_data in bag.items():
                try:
                    product = Product.objects.get(id=item_id)
                    if isinstance(item_data, int):
                        order_line_item = OrderLineItem(
                            order=order,
                            product=product,
                            quantity=item_data,
                        )
                        order_line_item.save()
                    else:
                        for size, quantity in item_data['items_by_size'].items():
                            order_line_item = OrderLineItem(
                                order=order,
                                product=product,
                                quantity=quantity,
                                product_size=size,
                            )
                            order_line_item.save()
                except Product.DoesNotExist:
                    messages.error(request, (
                        "One of the products in your bag wasn't found in our database. "
                        "Please call us for assistance!")
                    )
                    order.delete()
                    return redirect(reverse('view_bag'))

            # Save the info to the user's profile if all is well
            request.session['save_info'] = 'save-info' in request.POST
            return redirect(reverse('checkout_success', args=[order.order_number]))
        else:
            messages.error(request, 'There was an error with your form. \
                Please double check your information.')
    else:
        bag = request.session.get('bag', {})
        if not bag:
            messages.error(request, "There's nothing in your bag at the moment")
            return redirect(reverse('products'))

        current_bag = bag_contents(request)
        total = current_bag['grand_total']
        stripe_total = round(total * 100)
        stripe.api_key = stripe_secret_key
        intent = stripe.PaymentIntent.create(
            amount=stripe_total,
            currency=settings.STRIPE_CURRENCY,
        )

        # Attempt to prefill the form with any info the user maintains in their profile
        if request.user.is_authenticated:
            try:
                profile = UserProfile.objects.get(user=request.user)
                order_form = OrderForm(initial={
                    'full_name': profile.user.get_full_name(),
                    'email': profile.user.email,
                    'phone_number': profile.default_phone_number,
                    'country': profile.default_country,
                    'postcode': profile.default_postcode,
                    'town_or_city': profile.default_town_or_city,
                    'street_address1': profile.default_street_address1,
                    'street_address2': profile.default_street_address2,
                    'county': profile.default_county,
                })
            except UserProfile.DoesNotExist:
                order_form = OrderForm()
        else:
            order_form = OrderForm()

    if not stripe_public_key:
        messages.warning(request, 'Stripe public key is missing. \
            Did you forget to set it in your environment?')

    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
        'stripe_public_key': stripe_public_key,
        'client_secret': intent.client_secret,
    }

    return render(request, template, context)


def checkout_success(request, order_number):
    """
    Handle successful checkouts
    """
    save_info = request.session.get('save_info')
    order = get_object_or_404(Order, order_number=order_number)

    if request.user.is_authenticated:
        profile = UserProfile.objects.get(user=request.user)
        # Attach the user's profile to the order
        order.user_profile = profile
        order.save()

        # Save the user's info
        if save_info:
            profile_data = {
                'default_phone_number': order.phone_number,
                'default_country': order.country,
                'default_postcode': order.postcode,
                'default_town_or_city': order.town_or_city,
                'default_street_address1': order.street_address1,
                'default_street_address2': order.street_address2,
                'default_county': order.county,
            }
            user_profile_form = UserProfileForm(profile_data, instance=profile)
            if user_profile_form.is_valid():
                user_profile_form.save()

    messages.success(request, f'Order successfully processed! \
        Your order number is {order_number}. A confirmation \
        email will be sent to {order.email}.')

    if 'bag' in request.session:
        del request.session['bag']

    template = 'checkout/checkout_success.html'
    context = {
        'order': order,
    }

    return render(request, template, context)

```
- git add . 
- git commit -m "added order history and updated checkout views"
- git push


### Profile App - Part 9

checkout/webhook_handler.py
```
from django.http import HttpResponse

from .models import Order, OrderLineItem
from products.models import Product
from profiles.models import UserProfile

import json
import time

class StripeWH_Handler:
    """Handle Stripe webhooks"""

    def __init__(self, request):
        self.request = request

    def handle_event(self, event):
        """
        Handle a generic/unknown/unexpected webhook event
        """
        return HttpResponse(
            content=f'Unhandled webhook received: {event["type"]}',
            status=200)

    def handle_payment_intent_succeeded(self, event):
        """
        Handle the payment_intent.succeeded webhook from Stripe
        """
        intent = event.data.object
        pid = intent.id
        bag = intent.metadata.bag
        save_info = intent.metadata.save_info

        billing_details = intent.charges.data[0].billing_details
        shipping_details = intent.shipping
        grand_total = round(intent.charges.data[0].amount / 100, 2)

        # Clean data in the shipping details
        for field, value in shipping_details.address.items():
            if value == "":
                shipping_details.address[field] = None

        # Update profile information if save_info was checked
        profile = None
        username = intent.metadata.username
        if username != 'AnonymousUser':
            profile = UserProfile.objects.get(user__username=username)
            if save_info:
                profile.default_phone_number = shipping_details.phone
                profile.default_country = shipping_details.address.country
                profile.default_postcode = shipping_details.address.postal_code
                profile.default_town_or_city = shipping_details.address.city
                profile.default_street_address1 = shipping_details.address.line1
                profile.default_street_address2 = shipping_details.address.line2
                profile.default_county = shipping_details.address.state
                profile.save()

        order_exists = False
        attempt = 1
        while attempt <= 5:
            try:
                order = Order.objects.get(
                    full_name__iexact=shipping_details.name,
                    email__iexact=billing_details.email,
                    phone_number__iexact=shipping_details.phone,
                    country__iexact=shipping_details.address.country,
                    postcode__iexact=shipping_details.address.postal_code,
                    town_or_city__iexact=shipping_details.address.city,
                    street_address1__iexact=shipping_details.address.line1,
                    street_address2__iexact=shipping_details.address.line2,
                    county__iexact=shipping_details.address.state,
                    grand_total=grand_total,
                    original_bag=bag,
                    stripe_pid=pid,
                )
                order_exists = True
                break
            except Order.DoesNotExist:
                attempt += 1
                time.sleep(1)
        if order_exists:
            return HttpResponse(
                content=f'Webhook received: {event["type"]} | SUCCESS: Verified order already in database',
                status=200)
        else:
            order = None
            try:
                order = Order.objects.create(
                    full_name=shipping_details.name,
                    user_profile=profile,
                    email=billing_details.email,
                    phone_number=shipping_details.phone,
                    country=shipping_details.address.country,
                    postcode=shipping_details.address.postal_code,
                    town_or_city=shipping_details.address.city,
                    street_address1=shipping_details.address.line1,
                    street_address2=shipping_details.address.line2,
                    county=shipping_details.address.state,
                    original_bag=bag,
                    stripe_pid=pid,
                )
                for item_id, item_data in json.loads(bag).items():
                    product = Product.objects.get(id=item_id)
                    if isinstance(item_data, int):
                        order_line_item = OrderLineItem(
                            order=order,
                            product=product,
                            quantity=item_data,
                        )
                        order_line_item.save()
                    else:
                        for size, quantity in item_data['items_by_size'].items():
                            order_line_item = OrderLineItem(
                                order=order,
                                product=product,
                                quantity=quantity,
                                product_size=size,
                            )
                            order_line_item.save()
            except Exception as e:
                if order:
                    order.delete()
                return HttpResponse(
                    content=f'Webhook received: {event["type"]} | ERROR: {e}',
                    status=500)
        return HttpResponse(
            content=f'Webhook received: {event["type"]} | SUCCESS: Created order in webhook',
            status=200)

    def handle_payment_intent_payment_failed(self, event):
        """
        Handle the payment_intent.payment_failed webhook from Stripe
        """
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)

```
- git add . 
- git commit -m "Updated webhook handler to handle profiles"
- git push


### Profile App - Part 10

checkout/templates/checkout/confirmation_emails/confirmation_email_body.txt
```
Hello {{ order.full_name }}!

This is a confirmation of your order at Boutique Ado. Your order information is below:

Order Number: {{ order.order_number }}
Order Date: {{ order.date }}

Order Total: ${{ order.order_total }}
Delivery: ${{ order.delivery_cost }}
Grand Total: ${{ order.grand_total }}

Your order will be shipped to {{ order.street_address1 }} in {{ order.town_or_city }}, {{ order.country }}.

We've got your phone number on file as {{ order.phone_number }}.

If you have any questions, feel free to contact us at {{ contact_email }}.

Thank you for your order!

Sincerely,

Boutique Ado
```

checkout/templates/checkout/confirmation_emails/confirmation_email_subject.txt
```
Boutique Ado Confirmation for Order Number {{ order.order_number }}
```

checkout/webhook_handler.py
```
from django.http import HttpResponse
from django.core.mail import send_mail
from django.template.loader import render_to_string
from django.conf import settings

from .models import Order, OrderLineItem
from products.models import Product
from profiles.models import UserProfile

import json
import time

class StripeWH_Handler:
    """Handle Stripe webhooks"""

    def __init__(self, request):
        self.request = request

    def _send_confirmation_email(self, order):
        """Send the user a confirmation email"""
        cust_email = order.email
        subject = render_to_string(
            'checkout/confirmation_emails/confirmation_email_subject.txt',
            {'order': order})
        body = render_to_string(
            'checkout/confirmation_emails/confirmation_email_body.txt',
            {'order': order, 'contact_email': settings.DEFAULT_FROM_EMAIL})
        
        send_mail(
            subject,
            body,
            settings.DEFAULT_FROM_EMAIL,
            [cust_email]
        )        

    def handle_event(self, event):
        """
        Handle a generic/unknown/unexpected webhook event
        """
        return HttpResponse(
            content=f'Unhandled webhook received: {event["type"]}',
            status=200)

    def handle_payment_intent_succeeded(self, event):
        """
        Handle the payment_intent.succeeded webhook from Stripe
        """
        intent = event.data.object
        pid = intent.id
        bag = intent.metadata.bag
        save_info = intent.metadata.save_info

        billing_details = intent.charges.data[0].billing_details
        shipping_details = intent.shipping
        grand_total = round(intent.charges.data[0].amount / 100, 2)

        # Clean data in the shipping details
        for field, value in shipping_details.address.items():
            if value == "":
                shipping_details.address[field] = None

        # Update profile information if save_info was checked
        profile = None
        username = intent.metadata.username
        if username != 'AnonymousUser':
            profile = UserProfile.objects.get(user__username=username)
            if save_info:
                profile.default_phone_number = shipping_details.phone
                profile.default_country = shipping_details.address.country
                profile.default_postcode = shipping_details.address.postal_code
                profile.default_town_or_city = shipping_details.address.city
                profile.default_street_address1 = shipping_details.address.line1
                profile.default_street_address2 = shipping_details.address.line2
                profile.default_county = shipping_details.address.state
                profile.save()

        order_exists = False
        attempt = 1
        while attempt <= 5:
            try:
                order = Order.objects.get(
                    full_name__iexact=shipping_details.name,
                    email__iexact=billing_details.email,
                    phone_number__iexact=shipping_details.phone,
                    country__iexact=shipping_details.address.country,
                    postcode__iexact=shipping_details.address.postal_code,
                    town_or_city__iexact=shipping_details.address.city,
                    street_address1__iexact=shipping_details.address.line1,
                    street_address2__iexact=shipping_details.address.line2,
                    county__iexact=shipping_details.address.state,
                    grand_total=grand_total,
                    original_bag=bag,
                    stripe_pid=pid,
                )
                order_exists = True
                break
            except Order.DoesNotExist:
                attempt += 1
                time.sleep(1)
        if order_exists:
            self._send_confirmation_email(order)
            return HttpResponse(
                content=f'Webhook received: {event["type"]} | SUCCESS: Verified order already in database',
                status=200)
        else:
            order = None
            try:
                order = Order.objects.create(
                    full_name=shipping_details.name,
                    user_profile=profile,
                    email=billing_details.email,
                    phone_number=shipping_details.phone,
                    country=shipping_details.address.country,
                    postcode=shipping_details.address.postal_code,
                    town_or_city=shipping_details.address.city,
                    street_address1=shipping_details.address.line1,
                    street_address2=shipping_details.address.line2,
                    county=shipping_details.address.state,
                    original_bag=bag,
                    stripe_pid=pid,
                )
                for item_id, item_data in json.loads(bag).items():
                    product = Product.objects.get(id=item_id)
                    if isinstance(item_data, int):
                        order_line_item = OrderLineItem(
                            order=order,
                            product=product,
                            quantity=item_data,
                        )
                        order_line_item.save()
                    else:
                        for size, quantity in item_data['items_by_size'].items():
                            order_line_item = OrderLineItem(
                                order=order,
                                product=product,
                                quantity=quantity,
                                product_size=size,
                            )
                            order_line_item.save()
            except Exception as e:
                if order:
                    order.delete()
                return HttpResponse(
                    content=f'Webhook received: {event["type"]} | ERROR: {e}',
                    status=500)
        self._send_confirmation_email(order)
        return HttpResponse(
            content=f'Webhook received: {event["type"]} | SUCCESS: Created order in webhook',
            status=200)

    def handle_payment_intent_payment_failed(self, event):
        """
        Handle the payment_intent.payment_failed webhook from Stripe
        """
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)

```

boutique/settings.py
```
DEFAULT_FROM_EMAIL = 'boutiqueado@example.com'
```
- git add . 
- git commit -m "completed confirmation emails"
- git push


### Product Form

products/forms.py
```
from django import forms
from .models import Product, Category


class ProductForm(forms.ModelForm):

    class Meta:
        model = Product
        fields = '__all__'

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        categories = Category.objects.all()
        friendly_names = [(c.id, c.get_friendly_name()) for c in categories]

        self.fields['category'].choices = friendly_names
        for field_name, field in self.fields.items():
            field.widget.attrs['class'] = 'border-black rounded-0'

```
- git add . 
- git commit -m "Product Admin - Product Form"
- git push


### Add Product View

products/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.db.models import Q
from django.db.models.functions import Lower

from .models import Product, Category
from .forms import ProductForm


def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None
    sort = None
    direction = None

    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))
            if sortkey == 'category':
                sortkey = 'category__name'
            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)
            
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    current_sorting = f'{sort}_{direction}'

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
        'current_sorting': current_sorting,
    }

    return render(request, 'products/products.html', context)


def product_detail(request, product_id):
    """ A view to show individual product details """

    product = get_object_or_404(Product, pk=product_id)

    context = {
        'product': product,
    }

    return render(request, 'products/product_detail.html', context)


def add_product(request):
    """ Add a product to the store """
    form = ProductForm()
    template = 'products/add_product.html'
    context = {
        'form': form,
    }

    return render(request, template, context)
```

products/urls.py
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.all_products, name='products'),
    path('<int:product_id>/', views.product_detail, name='product_detail'),
    path('add/', views.add_product, name='add_product'),
]

```

products/templates/products/add_product.html
```
{% extends "base.html" %}
{% load static %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col-12 col-md-6">
                <hr>
                <h2 class="logo-font mb-4">Product Management</h2>
                <h5 class="text-muted">Add a Product</h5>
                <hr>
            </div>
        </div>

        <div class="row">
            <div class="col-12 col-md-6">
                <form method="POST" action="{% url 'add_product' %}" class="form mb-2" enctype="multipart/form-data">
                    {% csrf_token %}
                    {{ form | crispy }}
                    <div class="text-right">
                        <a class="btn btn-outline-black rounded-0" href="{% url 'products' %}">Cancel</a>
                        <button class="btn btn-black rounded-0" type="submit">Add Product</button>
                    </div>
                </form>
            </div>            
        </div>
    </div>
{% endblock %}
```
- git add . 
- git commit -m "Product Admin - add product view, url and template"
- git push


### Finishing the Add Product Functionality

products/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.db.models import Q
from django.db.models.functions import Lower

from .models import Product, Category
from .forms import ProductForm

# Create your views here.

def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None
    sort = None
    direction = None

    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))
            if sortkey == 'category':
                sortkey = 'category__name'
            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)
            
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    current_sorting = f'{sort}_{direction}'

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
        'current_sorting': current_sorting,
    }

    return render(request, 'products/products.html', context)


def product_detail(request, product_id):
    """ A view to show individual product details """

    product = get_object_or_404(Product, pk=product_id)

    context = {
        'product': product,
    }

    return render(request, 'products/product_detail.html', context)


def add_product(request):
    """ Add a product to the store """
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            messages.success(request, 'Successfully added product!')
            return redirect(reverse('add_product'))
        else:
            messages.error(request, 'Failed to add product. Please ensure the form is valid.')
    else:
        form = ProductForm()
        
    template = 'products/add_product.html'
    context = {
        'form': form,
    }

    return render(request, template, context)

```

profiles/views.py
```
from django.shortcuts import render, get_object_or_404
from django.contrib import messages

from .models import UserProfile
from .forms import UserProfileForm

from checkout.models import Order


def profile(request):
    """ Display the user's profile. """
    profile = get_object_or_404(UserProfile, user=request.user)

    if request.method == 'POST':
        form = UserProfileForm(request.POST, instance=profile)
        if form.is_valid():
            form.save()
            messages.success(request, 'Profile updated successfully')
        else:
            messages.error(request, 'Update failed. Please ensure the form is valid.')
    else:
        form = UserProfileForm(instance=profile)
    orders = profile.orders.all()

    template = 'profiles/profile.html'
    context = {
        'form': form,
        'orders': orders,
        'on_profile_page': True
    }

    return render(request, template, context)


def order_history(request, order_number):
    order = get_object_or_404(Order, order_number=order_number)

    messages.info(request, (
        f'This is a past confirmation for order number {order_number}. '
        'A confirmation email was sent on the order date.'
    ))

    template = 'checkout/checkout_success.html'
    context = {
        'order': order,
        'from_profile': True,
    }

    return render(request, template, context)

```

templates/includes/toasts/toast_success.html
```
<div class="toast custom-toast rounded-0 border-top-0" data-autohide="false">
    <div class="arrow-up arrow-success"></div>
    <div class="w-100 toast-capper bg-success"></div>
    <div class="toast-header bg-white text-dark">
        <strong class="mr-auto">Success!</strong>
        <button type="button" class="ml-2 mb-1 close text-dark" data-dismiss="toast" aria-label="Close">
            <span aria-hidden="true">&times;</span>
        </button>
    </div>
    <div class="toast-body bg-white">
        <div class="row">
            <div class="col">
                {{ message }}
                <hr class="mt-1 mb-3">
            </div>
        </div>
        {% if grand_total and not on_profile_page %}
            <p class="logo-font bg-white text-black py-1">Your Bag ({{ product_count }})</p>
            <div class="bag-notification-wrapper">
                {% for item in bag_items %}
                    <div class="row">
                        <div class="col-3 my-1">
                            {% if item.product.image %}
                            <img class="w-100" src="{{ item.product.image.url }}" alt="{{ item.product.name }}">
                            {% else %}
                            <img class="w-100" src="{{ MEDIA_URL }}noimage.png" alt="{{ item.product.name }}">
                            {% endif %}
                        </div>
                        <div class="col-9">
                            <p class="my-0"><strong>{{ item.product.name }}</strong></p>
                            <p class="my-0 small">Size: {% if item.product.has_sizes %}{{ item.size|upper }}{% else %}N/A{% endif %}</p>
                            <p class="my-0 small text-muted">Qty: {{ item.quantity }}</p>
                        </div>
                    </div>
                {% endfor %}
            </div>
            <div class="row">
                <div class="col">
                    <strong><p class="mt-3 mb-1 text-black">
                        Total{% if free_delivery_delta > 0 %} (Exc. delivery){% endif %}: 
                        <span class="float-right">${{ total|floatformat:2 }}</span>
                    </p></strong>
                    {% if free_delivery_delta > 0 %}
                        <p class="mb-0 p-2 bg-warning shadow-sm text-black text-center">
                            Spend <strong>${{ free_delivery_delta }}</strong> more to get free next day delivery!
                        </p>
                    {% endif %}
                    <a href="{% url 'view_bag' %}" class="btn btn-black btn-block rounded-0">
                        <span class="text-uppercase">Go To Secure Checkout</span>
                        <span class="icon">
                            <i class="fas fa-lock"></i>
                        </span>
                    </a>
                </div>
            </div>
        {% endif %}
    </div>
</div>
```

bag/templates/bag/bag.html
```
{% extends "base.html" %}
{% load static %}
{% load bag_tools %}

{% block page_header %}
<div class="container header-container">
    <div class="row">
        <div class="col"></div>
    </div>
</div>
{% endblock %}

{% block content %}
<div class="overlay"></div>
<div class="container mb-2">
    <div class="row">
        <div class="col">
            <hr>
            <h2 class="logo-font mb-4">Shopping Bag</h2>
            <hr>
        </div>
    </div>

    <div class="row">
        <div class="col">
            {% if bag_items %}
            <div class="table-responsive rounded">
                <table class="table table-sm table-borderless">
                    <thead class="text-black">
                        <tr>
                            <th scope="col">Product Info</th>
                            <th scope="col"></th>
                            <th scope="col">Price</th>
                            <th scope="col">Qty</th>
                            <th scope="col">Subtotal</th>
                        </tr>
                    </thead>

                    {% for item in bag_items %}
                    <tr>
                        <td class="p-3 w-25">
                            {% if item.product.image %}
                            <img class="img-fluid rounded" src="{{ item.product.image.url }}"
                                alt="{{ item.product.name }}">
                            {% else %}
                            <img class="img-fluid rounded" src="{{ MEDIA_URL }}noimage.png"
                                alt="{{ item.product.name }}">
                            {% endif %}
                        </td>
                        <td class="py-3">
                            <p class="my-0"><strong>{{ item.product.name }}</strong></p>
                            <p class="my-0"><strong>Size:
                                </strong>{% if item.product.has_sizes %}{{ item.size|upper }}{% else %}N/A{% endif %}
                            </p>
                            <p class="my-0 small text-muted">SKU: {{ item.product.sku|upper }}</p>
                        </td>
                        <td class="py-3">
                            <p class="my-0">${{ item.product.price }}</p>
                        </td>
                        <td class="py-3 w-25">
                            <form class="form update-form" method="POST" action="{% url 'adjust_bag' item.item_id %}">
                                {% csrf_token %}
                                <div class="form-group">
                                    <div class="input-group">
                                        <div class="input-group-prepend">
                                            <button class="decrement-qty btn btn-sm btn-black rounded-0"
                                                data-item_id="{{ item.item_id }}" id="decrement-qty_{{ item.item_id }}">
                                                <span>
                                                    <i class="fas fa-minus fa-sm"></i>
                                                </span>
                                            </button>
                                        </div>
                                        <input class="form-control form-control-sm qty_input" type="number"
                                            name="quantity" value="{{ item.quantity }}" min="1" max="99"
                                            data-item_id="{{ item.item_id }}" id="id_qty_{{ item.item_id }}">
                                        <div class="input-group-append">
                                            <button class="increment-qty btn btn-sm btn-black rounded-0"
                                                data-item_id="{{ item.item_id }}" id="increment-qty_{{ item.item_id }}">
                                                <span>
                                                    <i class="fas fa-plus fa-sm"></i>
                                                </span>
                                            </button>
                                        </div>
                                        {% if item.product.has_sizes %}
                                        <input type="hidden" name="product_size" value="{{ item.size }}">
                                        {% endif %}
                                    </div>
                                </div>
                            </form>
                            <a class="update-link text-info"><small>Update</small></a>
                            <a class="remove-item text-danger float-right" id="remove_{{ item.item_id }}"
                                data-product_size="{{ item.size }}"><small>Remove</small></a>
                        </td>
                        <td class="py-3">
                            <p class="my-0">${{ item.product.price | calc_subtotal:item.quantity }}</p>
                        </td>
                    </tr>
                    {% endfor %}
                    <tr>
                        <td colspan="5" class="pt-5 text-right">
                            <h6><strong>Bag Total: ${{ total|floatformat:2 }}</strong></h6>
                            <h6>Delivery: ${{ delivery|floatformat:2 }}</h6>
                            <h4 class="mt-4"><strong>Grand Total: ${{ grand_total|floatformat:2 }}</strong></h4>
                            {% if free_delivery_delta > 0 %}
                            <p class="mb-1 text-danger">
                                You could get free delivery by spending just <strong>${{ free_delivery_delta }}</strong>
                                more!
                            </p>
                            {% endif %}
                        </td>
                    </tr>
                    <tr>
                        <td colspan="5" class="text-right">
                            <a href="{% url 'products' %}" class="btn btn-outline-black rounded-0 btn-lg">
                                <span class="icon">
                                    <i class="fas fa-chevron-left"></i>
                                </span>
                                <span class="text-uppercase">Keep Shopping</span>
                            </a>
                            <a href="{% url 'checkout' %}" class="btn btn-black rounded-0 btn-lg">
                                <span class="text-uppercase">Secure Checkout</span>
                                <span class="icon">
                                    <i class="fas fa-lock"></i>
                                </span>
                            </a>
                        </td>
                    </tr>
                </table>
            </div>
            {% else %}
            <p class="lead mb-5">Your bag is empty.</p>
            <a href="{% url 'products' %}" class="btn btn-outline-black rounded-0 btn-lg">
                <span class="icon">
                    <i class="fas fa-chevron-left"></i>
                </span>
                <span class="text-uppercase">Keep Shopping</span>
            </a>
            {% endif %}
        </div>
    </div>
</div>
{% endblock %}

{% block postloadjs %}
{{ block.super }}
{% include 'products/includes/quantity_input_script.html' %}

<script type="text/javascript">
    // Update quantity on click
    $('.update-link').click(function (e) {
        var form = $(this).prev('.update-form');
        form.submit();
    })

    // Remove item and reload on click
    $('.remove-item').click(function (e) {
        var csrfToken = "{{ csrf_token }}";
        var itemId = $(this).attr('id').split('remove_')[1];
        var size = $(this).data('product_size');
        var url = `/bag/remove/${itemId}/`;
        var data = {
            'csrfmiddlewaretoken': csrfToken,
            'product_size': size
        };

        $.post(url, data)
            .done(function () {
                location.reload();
            });
    })
</script>
{% endblock %}
```

templates/base.html
```
{% load static %}

<!doctype html>
<html lang="en">
  <head>

    {% block meta %}
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    {% endblock %}

    {% block extra_meta %}
    {% endblock %}

    {% block corecss %}
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
        <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Lato&display=swap">
        <link rel="stylesheet" href="{% static 'css/base.css' %}">
    {% endblock %}

    {% block extra_css %}
    {% endblock %}

    {% block corejs %}
        <script src="https://kit.fontawesome.com/e9c73d7092.js" crossorigin="anonymous"></script>
        <script src="https://code.jquery.com/jquery-3.4.1.min.js" integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo=" crossorigin="anonymous"></script>
        <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
        <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
        <!-- Stripe -->
        <script src="https://js.stripe.com/v3/"></script>
    {% endblock %}

    {% block extra_js %}
    {% endblock %}

    <title>Boutique Ado {% block extra_title %}{% endblock %}</title>
  </head>
  <body>
    <header class="container-fluid fixed-top">
        <div id="topnav" class="row bg-white pt-lg-2 d-none d-lg-flex">
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0 text-center text-lg-left">
                <a href="{% url 'home' %}" class="nav-link main-logo-link">
                    <h2 class="logo-font text-black my-0"><strong>Boutique</strong> Ado</h2>
                </a>
            </div>
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0">
                <form method="GET" action="{% url 'products' %}">
                    <div class="input-group w-100">
                        <input class="form-control border border-black rounded-0" type="text" name="q" placeholder="Search our site">
                        <div class="input-group-append">
                            <button class="form-control btn btn-black border border-black rounded-0" type="submit">
                                <span class="icon">
                                    <i class="fas fa-search"></i>
                                </span>
                            </button>
                        </div>
                    </div>
                </form>
            </div>
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0">
                <ul class="list-inline list-unstyled text-center text-lg-right my-0">
                    <li class="list-inline-item dropdown">
                        <a class="text-black nav-link" href="#" id="user-options" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                            <div class="text-center">
                                <div><i class="fas fa-user fa-lg"></i></div>
                                <p class="my-0">My Account</p>
                            </div>
                        </a>
                        <div class="dropdown-menu border-0" aria-labelledby="user-options">
                            {% if request.user.is_authenticated %}
                                {% if request.user.is_superuser %}
                                    <a href="{% url 'add_product' %}" class="dropdown-item">Product Management</a>
                                {% endif %}
                                <a href="{% url 'profile' %}" class="dropdown-item">My Profile</a>
                                <a href="{% url 'account_logout' %}" class="dropdown-item">Logout</a>
                            {% else %}
                                <a href="{% url 'account_signup' %}" class="dropdown-item">Register</a>
                                <a href="{% url 'account_login' %}" class="dropdown-item">Login</a>
                            {% endif %}
                        </div>
                    </li>
                    <li class="list-inline-item">
                        <a class="{% if grand_total %}text-info font-weight-bold{% else %}text-black{% endif %} nav-link" href="{% url 'view_bag' %}">
                            <div class="text-center">
                                <div><i class="fas fa-shopping-bag fa-lg"></i></div>
                                <p class="my-0">
                                    {% if grand_total %}
                                        ${{ grand_total|floatformat:2 }}
                                    {% else %}
                                        $0.00
                                    {% endif %}
                                </p>
                            </div>
                        </a>
                    </li>
                </ul>
            </div>
        </div>
        <div class="row bg-white">
            <nav class="navbar navbar-expand-lg navbar-light w-100">
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#main-nav" aria-controls="main-nav" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                {% include 'includes/mobile-top-header.html' %}
                {% include 'includes/main-nav.html' %}
            </nav>
        </div>
        <div id="delivery-banner" class="row text-center">
            <div class="col bg-black text-white">
                <h4 class="logo-font my-1">Free delivery on orders over ${{ free_delivery_threshold }}!</h4>                
            </div>            
        </div>
    </header>

    {% if messages %}
        <div class="message-container">
            {% for message in messages %}
                {% with message.level as level %}
                    {% if level == 40 %}
                        {% include 'includes/toasts/toast_error.html' %}
                    {% elif level == 30 %}
                        {% include 'includes/toasts/toast_warning.html' %}
                    {% elif level == 25 %}
                        {% include 'includes/toasts/toast_success.html' %}
                    {% else %}
                        {% include 'includes/toasts/toast_info.html' %}
                    {% endif %}
                {% endwith %}
            {% endfor %}
        </div>
    {% endif %}

    {% block page_header %}
    {% endblock %}

    {% block content %}
    {% endblock %}

    {% block postloadjs %}
    <script type="text/javascript">
        $('.toast').toast('show');
    </script>
    {% endblock %}

    
  </body>
</html>
```

templates/includes/mobile-top-header.html
```
<li class="list-inline-item">
    <a class="text-black nav-link d-block d-lg-none" href="#" id="mobile-search" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
        <div class="text-center">
            <div><i class="fas fa-search fa-lg"></i></div>
            <p class="my-0">Search</p>
        </div>
    </a>
    <div class="dropdown-menu border-0 w-100 p-3 rounded-0 my-0" aria-labelledby="mobile-search">
        <form class="form" method="GET" action="{% url 'products' %}">
            <div class="input-group w-100">
                <input class="form-control border border-black rounded-0" type="text" name="q" placeholder="Search our site">
                <div class="input-group-append">
                    <button class="form-control form-control btn btn-black border border-black rounded-0" type="submit">
                        <span class="icon">
                            <i class="fas fa-search"></i>
                        </span>
                    </button>
                </div>
            </div>
        </form>
    </div>
</li>
<li class="list-inline-item dropdown">
    <a class="text-black nav-link d-block d-lg-none" href="#" id="user-options" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
        <div class="text-center">
            <div><i class="fas fa-user fa-lg"></i></div>
            <p class="my-0">My Account</p>
        </div>
    </a>
    <div class="dropdown-menu border-0" aria-labelledby="user-options">
        {% if request.user.is_authenticated %}
            {% if request.user.is_superuser %}
                <a href="{% url 'add_product' %}" class="dropdown-item">Product Management</a>
            {% endif %}
            <a href="" class="dropdown-item">My Profile</a>
            <a href="{% url 'account_logout' %}" class="dropdown-item">Logout</a>
        {% else %}
            <a href="{% url 'account_signup' %}" class="dropdown-item">Register</a>
            <a href="{% url 'account_login' %}" class="dropdown-item">Login</a>
        {% endif %}
    </div>
</li>
<li class="list-inline-item">
    <a class="{% if grand_total %}text-primary font-weight-bold{% else %}text-black{% endif %} nav-link d-block d-lg-none" href="{% url 'view_bag' %}">
        <div class="text-center">
            <div><i class="fas fa-shopping-bag fa-lg"></i></div>
            <p class="my-0">
                {% if grand_total %}
                    ${{ grand_total|floatformat:2 }}
                {% else %}
                    $0.00
                {% endif %}
            </p>
        </div>
    </a>
</li>
```

To use the current superuser account created at the start, head over to your profiles > models.py and comment out the the code below. Then log in with your superuser account (from frontend login; or the url with /admin). Once logged in, remember to REVERT your changes - i.e. un-comment the if statement and re-indent the UserProfile.objects.create(user=instance)
```
@receiver(post_save, sender=User)
def create_or_update_user_profile(sender, instance, created, **kwargs):
    """
    Create or update the user profile
    """
    # if created:
    UserProfile.objects.create(user=instance)
    # Existing users: just save the profile
    # instance.userprofile.save()

```

OR
you can create a new superuser account with python3 manage.py createsuperuser
- python3 manage.py createsuperuser
  - username: user
  - Email address: user@user.com
  - password: adminaccount

- git add . 
- git commit -m "Product admin - Finished add product functionality."
- git push


### Editing Products

products/templates/products/edit_product.html
```
{% extends "base.html" %}
{% load static %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col-12 col-md-6">
                <hr>
                <h2 class="logo-font mb-4">Product Management</h2>
                <h5 class="text-muted">Edit a Product</h5>
                <hr>
            </div>
        </div>

        <div class="row">
            <div class="col-12 col-md-6">
                <form method="POST" action="{% url 'edit_product' product.id %}" class="form mb-2" enctype="multipart/form-data">
                    {% csrf_token %}
                    {{ form | crispy }}
                    <div class="text-right">
                        <a class="btn btn-outline-black rounded-0" href="{% url 'products' %}">Cancel</a>
                        <button class="btn btn-black rounded-0" type="submit">Update Product</button>
                    </div>
                </form>
            </div>            
        </div>
    </div>
{% endblock %}
```

products/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.db.models import Q
from django.db.models.functions import Lower

from .models import Product, Category
from .forms import ProductForm


def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None
    sort = None
    direction = None

    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))
            if sortkey == 'category':
                sortkey = 'category__name'
            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)
            
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    current_sorting = f'{sort}_{direction}'

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
        'current_sorting': current_sorting,
    }

    return render(request, 'products/products.html', context)


def product_detail(request, product_id):
    """ A view to show individual product details """

    product = get_object_or_404(Product, pk=product_id)

    context = {
        'product': product,
    }

    return render(request, 'products/product_detail.html', context)


def add_product(request):
    """ Add a product to the store """
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            messages.success(request, 'Successfully added product!')
            return redirect(reverse('add_product'))
        else:
            messages.error(request, 'Failed to add product. Please ensure the form is valid.')
    else:
        form = ProductForm()
        
    template = 'products/add_product.html'
    context = {
        'form': form,
    }

    return render(request, template, context)


def edit_product(request, product_id):
    """ Edit a product in the store """
    product = get_object_or_404(Product, pk=product_id)
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES, instance=product)
        if form.is_valid():
            form.save()
            messages.success(request, 'Successfully updated product!')
            return redirect(reverse('product_detail', args=[product.id]))
        else:
            messages.error(request, 'Failed to update product. Please ensure the form is valid.')
    else:
        form = ProductForm(instance=product)
        messages.info(request, f'You are editing {product.name}')

    template = 'products/edit_product.html'
    context = {
        'form': form,
        'product': product,
    }

    return render(request, template, context)

```

products/urls.py
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.all_products, name='products'),
    path('<int:product_id>/', views.product_detail, name='product_detail'),
    path('add/', views.add_product, name='add_product'),
    path('edit/<int:product_id>/', views.edit_product, name='edit_product'),
]

```
- git add . 
- git commit -m "Product Admin - Editing Products"
- git push


### Deleting Products

products/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.db.models import Q
from django.db.models.functions import Lower

from .models import Product, Category
from .forms import ProductForm


def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None
    sort = None
    direction = None

    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))
            if sortkey == 'category':
                sortkey = 'category__name'
            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)
            
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    current_sorting = f'{sort}_{direction}'

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
        'current_sorting': current_sorting,
    }

    return render(request, 'products/products.html', context)


def product_detail(request, product_id):
    """ A view to show individual product details """

    product = get_object_or_404(Product, pk=product_id)

    context = {
        'product': product,
    }

    return render(request, 'products/product_detail.html', context)


def add_product(request):
    """ Add a product to the store """
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            product = form.save()
            messages.success(request, 'Successfully added product!')
            return redirect(reverse('product_detail', args=[product.id]))
        else:
            messages.error(request, 'Failed to add product. Please ensure the form is valid.')
    else:
        form = ProductForm()
        
    template = 'products/add_product.html'
    context = {
        'form': form,
    }

    return render(request, template, context)


def edit_product(request, product_id):
    """ Edit a product in the store """
    product = get_object_or_404(Product, pk=product_id)
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES, instance=product)
        if form.is_valid():
            form.save()
            messages.success(request, 'Successfully updated product!')
            return redirect(reverse('product_detail', args=[product.id]))
        else:
            messages.error(request, 'Failed to update product. Please ensure the form is valid.')
    else:
        form = ProductForm(instance=product)
        messages.info(request, f'You are editing {product.name}')

    template = 'products/edit_product.html'
    context = {
        'form': form,
        'product': product,
    }

    return render(request, template, context)


def delete_product(request, product_id):
    """ Delete a product from the store """
    product = get_object_or_404(Product, pk=product_id)
    product.delete()
    messages.success(request, 'Product deleted!')
    return redirect(reverse('products'))

```

products/urls.py
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.all_products, name='products'),
    path('<int:product_id>/', views.product_detail, name='product_detail'),
    path('add/', views.add_product, name='add_product'),
    path('edit/<int:product_id>/', views.edit_product, name='edit_product'),
    path('delete/<int:product_id>/', views.delete_product, name='delete_product'),
]

```

products/templates/products/product_detail.html
```
{% extends "base.html" %}
{% load static %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container-fluid">
        <div class="row">
            <div class="col-12 col-md-6 col-lg-4 offset-lg-2">
                <div class="image-container my-5">
                    {% if product.image %}
                        <a href="{{ product.image.url }}" target="_blank">
                            <img class="card-img-top img-fluid" src="{{ product.image.url }}" alt="{{ product.name }}">
                        </a>
                        {% else %}
                        <a href="">
                            <img class="card-img-top img-fluid" src="{{ MEDIA_URL }}noimage.png" alt="{{ product.name }}">
                        </a>
                    {% endif %}
                </div>
            </div>
            <div class="col-12 col-md-6 col-lg-4">
                <div class="product-details-container mb-5 mt-md-5">
                    <p class="mb-0">{{ product.name }}</p>
                    <p class="lead mb-0 text-left font-weight-bold">${{ product.price }}</p>
                    {% if product.category %}
                    <p class="small mt-1 mb-0">
                        <a class="text-muted" href="{% url 'products' %}?category={{ product.category.name }}">
                            <i class="fas fa-tag mr-1"></i>{{ product.category.friendly_name }}
                        </a>
                    </p>
                    {% endif %}
                    {% if product.rating %}
                        <small class="text-muted"><i class="fas fa-star mr-1"></i>{{ product.rating }} / 5</small>
                    {% else %}
                        <small class="text-muted">No Rating</small>
                    {% endif %}
                    {% if request.user.is_superuser %}
                        <small class="ml-3">
                            <a href="{% url 'edit_product' product.id %}">Edit</a> | 
                            <a class="text-danger" href="{% url 'delete_product' product.id %}">Delete</a>
                        </small>
                    {% endif %}
                    <p class="mt-3">{{ product.description }}</p>
                    <form class="form" action="{% url 'add_to_bag' product.id %}" method="POST">
                        {% csrf_token %}
                        <div class="form-row">
                            {% with product.has_sizes as s %}
                            {% if s %}
                                <div class="col-12">
                                    <p><strong>Size:</strong></p>
                                    <select class="form-control rounded-0 w-50" name="product_size" id='id_product_size'>
                                        <option value="xs">XS</option>
                                        <option value="s">S</option>
                                        <option value="m" selected>M</option>
                                        <option value="l">L</option>
                                        <option value="xl">XL</option>
                                    </select>
                                </div>
                            {% endif %}
                            <div class="col-12">
                                <p class="mt-3"><strong>Quantity:</strong></p>
                                <div class="form-group w-50">
                                    <div class="input-group">
                                        <div class="input-group-prepend">
                                            <button class="decrement-qty btn btn-black rounded-0" 
                                                data-item_id="{{ product.id }}" id="decrement-qty_{{ product.id }}">
                                                <span class="icon">
                                                    <i class="fas fa-minus"></i>
                                                </span>
                                            </button>
                                        </div>
                                        <input class="form-control qty_input" type="number"
                                            name="quantity" value="1" min="1" max="99"
                                            data-item_id="{{ product.id }}"
                                            id="id_qty_{{ product.id }}">
                                        <div class="input-group-append">
                                            <button class="increment-qty btn btn-black rounded-0"
                                                data-item_id="{{ product.id }}" id="increment-qty_{{ product.id }}">
                                                <span class="icon">
                                                    <i class="fas fa-plus"></i>
                                                </span>
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="col{% if s %}-12 mt-2{% endif %}">
                                <a href="{% url 'products' %}" class="btn btn-outline-black rounded-0 mt-5">
                                    <span class="icon">
                                        <i class="fas fa-chevron-left"></i>
                                    </span>
                                    <span class="text-uppercase">Keep Shopping</span>
                                </a>
                                <input type="submit" class="btn btn-black rounded-0 text-uppercase mt-5" value="Add to Bag">
                            </div>
                            <input type="hidden" name="redirect_url" value="{{ request.path }}">
                            {% endwith %}
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
{% endblock %}

{% block postloadjs %}
{{ block.super }}
{% include 'products/includes/quantity_input_script.html' %}
{% endblock %}
```

products/templates/products/products.html
```
{% extends "base.html" %}
{% load static %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container-fluid">
        <div class="row">
            <div class="col text-center mt-3">
                <h2 class="logo-font">Products</h2>
                {% for c in current_categories %}
                    <a class="category-badge text-decoration-none" href="{% url 'products' %}?category={{ c.name }}">
                        <span class="p-2 mt-2 badge badge-white text-black rounded-0 border border-dark">{{ c.friendly_name }}</span>
                    </a>
                {% endfor %}
                <hr class="w-50 mb-1">
            </div>
        </div>
        <div class="row">
            <div class="product-container col-10 offset-1">
                <div class="row mt-1 mb-2">
                    <div class="col-12 col-md-6 my-auto order-md-last d-flex justify-content-center justify-content-md-end">
                        <div class="sort-select-wrapper w-50">
                            <select id="sort-selector" class="custom-select custom-select-sm rounded-0 border border-{% if current_sorting != 'None_None' %}info{% else %}black{% endif %}">
                                <option value="reset" {% if current_sorting == 'None_None' %}selected{% endif %}>Sort by...</option>
                                <option value="price_asc" {% if current_sorting == 'price_asc' %}selected{% endif %}>Price (low to high)</option>
                                <option value="price_desc" {% if current_sorting == 'price_desc' %}selected{% endif %}>Price (high to low)</option>
                                <option value="rating_asc" {% if current_sorting == 'rating_asc' %}selected{% endif %}>Rating (low to high)</option>
                                <option value="rating_desc" {% if current_sorting == 'rating_desc' %}selected{% endif %}>Rating (high to low)</option>
                                <option value="name_asc" {% if current_sorting == 'name_asc' %}selected{% endif %}>Name (A-Z)</option>
                                <option value="name_desc" {% if current_sorting == 'name_desc' %}selected{% endif %}>Name (Z-A)</option>
                                <option value="category_asc" {% if current_sorting == 'category_asc' %}selected{% endif %}>Category (A-Z)</option>
                                <option value="category_desc" {% if current_sorting == 'category_desc' %}selected{% endif %}>Category (Z-A)</option>
                            </select>
                        </div>
                    </div>
                    <div class="col-12 col-md-6 order-md-first">
                        <p class="text-muted mt-3 text-center text-md-left">
                            {% if search_term or current_categories or current_sorting != 'None_None' %}
                                <span class="small"><a href="{% url 'products' %}">Products Home</a> | </span>
                            {% endif %}
                            {{ products|length }} Products{% if search_term %} found for <strong>"{{ search_term }}"</strong>{% endif %}
                        </p>
                    </div>
                </div>
                <div class="row">
                    {% for product in products %}
                        <div class="col-sm-6 col-md-6 col-lg-4 col-xl-3">
                            <div class="card h-100 border-0">
                                {% if product.image %}
                                <a href="{% url 'product_detail' product.id %}">
                                    <img class="card-img-top img-fluid" src="{{ product.image.url }}" alt="{{ product.name }}">
                                </a>
                                {% else %}
                                <a href="{% url 'product_detail' product.id %}">
                                    <img class="card-img-top img-fluid" src="{{ MEDIA_URL }}noimage.png" alt="{{ product.name }}">
                                </a>
                                {% endif %}
                                <div class="card-body pb-0">
                                    <p class="mb-0">{{ product.name }}</p>
                                </div>
                                <div class="card-footer bg-white pt-0 border-0 text-left">
                                    <div class="row">
                                        <div class="col">
                                            <p class="lead mb-0 text-left font-weight-bold">${{ product.price }}</p>
                                            {% if product.category %}
                                            <p class="small mt-1 mb-0">
                                                <a class="text-muted" href="{% url 'products' %}?category={{ product.category.name }}">
                                                    <i class="fas fa-tag mr-1"></i>{{ product.category.friendly_name }}
                                                </a>
                                            </p>
                                            {% endif %}
                                            {% if product.rating %}
                                                <small class="text-muted"><i class="fas fa-star mr-1"></i>{{ product.rating }} / 5</small>
                                            {% else %}
                                                <small class="text-muted">No Rating</small>
                                            {% endif %}
                                            {% if request.user.is_superuser %}
                                                <small class="ml-3">
                                                    <a href="{% url 'edit_product' product.id %}">Edit</a> | 
                                                    <a class="text-danger" href="{% url 'delete_product' product.id %}">Delete</a>
                                                </small>
                                            {% endif %}
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        {% if forloop.counter|divisibleby:1 %}
                            <div class="col-12 d-sm-none mb-5">
                                <hr>
                            </div>
                        {% endif %}                        
                        {% if forloop.counter|divisibleby:2 %}
                            <div class="col-12 d-none d-sm-block d-md-block d-lg-none mb-5">
                                <hr>
                            </div>
                        {% endif %}
                        {% if forloop.counter|divisibleby:3 %}
                            <div class="col-12 d-none d-lg-block d-xl-none mb-5">
                                <hr>
                            </div>
                        {% endif %}
                        {% if forloop.counter|divisibleby:4 %}
                            <div class="col-12 d-none d-xl-block mb-5">
                                <hr>
                            </div>
                        {% endif %}
                    {% endfor %}
                </div>
            </div>
        </div>
    </div>
    <div class="btt-button shadow-sm rounded-0 border border-black">
        <a class="btt-link d-flex h-100">
            <i class="fas fa-arrow-up text-black mx-auto my-auto"></i>
        </a>	
    </div>
{% endblock %}

{% block postloadjs %}
    {{ block.super }}
    <script type="text/javascript">
		$('.btt-link').click(function(e) {
			window.scrollTo(0,0)
		})
	</script>
    
    <script type="text/javascript">
        $('#sort-selector').change(function() {
            var selector = $(this);
            var currentUrl = new URL(window.location);

            var selectedVal = selector.val();
            if(selectedVal != "reset"){
                var sort = selectedVal.split("_")[0];
                var direction = selectedVal.split("_")[1];

                currentUrl.searchParams.set("sort", sort);
                currentUrl.searchParams.set("direction", direction);

                window.location.replace(currentUrl);
            } else {
                currentUrl.searchParams.delete("sort");
                currentUrl.searchParams.delete("direction");

                window.location.replace(currentUrl);
            }
        })
    </script>
{% endblock %}
```
- git add . 
- git commit -m "Product Admin - Editing Products"
- git push


### Securing the Views

products/views.py
```
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.contrib.auth.decorators import login_required
from django.db.models import Q
from django.db.models.functions import Lower

from .models import Product, Category
from .forms import ProductForm

# Create your views here.

def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None
    sort = None
    direction = None

    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))
            if sortkey == 'category':
                sortkey = 'category__name'
            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)
            
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    current_sorting = f'{sort}_{direction}'

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
        'current_sorting': current_sorting,
    }

    return render(request, 'products/products.html', context)


def product_detail(request, product_id):
    """ A view to show individual product details """

    product = get_object_or_404(Product, pk=product_id)

    context = {
        'product': product,
    }

    return render(request, 'products/product_detail.html', context)


@login_required
def add_product(request):
    """ Add a product to the store """
    if not request.user.is_superuser:
        messages.error(request, 'Sorry, only store owners can do that.')
        return redirect(reverse('home'))

    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            product = form.save()
            messages.success(request, 'Successfully added product!')
            return redirect(reverse('product_detail', args=[product.id]))
        else:
            messages.error(request, 'Failed to add product. Please ensure the form is valid.')
    else:
        form = ProductForm()
        
    template = 'products/add_product.html'
    context = {
        'form': form,
    }

    return render(request, template, context)


@login_required
def edit_product(request, product_id):
    """ Edit a product in the store """
    if not request.user.is_superuser:
        messages.error(request, 'Sorry, only store owners can do that.')
        return redirect(reverse('home'))

    product = get_object_or_404(Product, pk=product_id)
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES, instance=product)
        if form.is_valid():
            form.save()
            messages.success(request, 'Successfully updated product!')
            return redirect(reverse('product_detail', args=[product.id]))
        else:
            messages.error(request, 'Failed to update product. Please ensure the form is valid.')
    else:
        form = ProductForm(instance=product)
        messages.info(request, f'You are editing {product.name}')

    template = 'products/edit_product.html'
    context = {
        'form': form,
        'product': product,
    }

    return render(request, template, context)


@login_required
def delete_product(request, product_id):
    """ Delete a product from the store """
    if not request.user.is_superuser:
        messages.error(request, 'Sorry, only store owners can do that.')
        return redirect(reverse('home'))

    product = get_object_or_404(Product, pk=product_id)
    product.delete()
    messages.success(request, 'Product deleted!')
    return redirect(reverse('products'))

```

profiles/views.py
```
from django.shortcuts import render, get_object_or_404
from django.contrib import messages
from django.contrib.auth.decorators import login_required
from .models import UserProfile
from .forms import UserProfileForm

from checkout.models import Order


@login_required
def profile(request):
    """ Display the user's profile. """
    profile = get_object_or_404(UserProfile, user=request.user)

    if request.method == 'POST':
        form = UserProfileForm(request.POST, instance=profile)
        if form.is_valid():
            form.save()
            messages.success(request, 'Profile updated successfully')
        else:
            messages.error(request, 'Update failed. Please ensure the form is valid.')
    else:
        form = UserProfileForm(instance=profile)
    orders = profile.orders.all()

    template = 'profiles/profile.html'
    context = {
        'form': form,
        'orders': orders,
        'on_profile_page': True
    }

    return render(request, template, context)


def order_history(request, order_number):
    order = get_object_or_404(Order, order_number=order_number)

    messages.info(request, (
        f'This is a past confirmation for order number {order_number}. '
        'A confirmation email was sent on the order date.'
    ))

    template = 'checkout/checkout_success.html'
    context = {
        'order': order,
        'from_profile': True,
    }

    return render(request, template, context)

```
- git add . 
- git commit -m "Product Admin - Securing the Views"
- git push


### Fixing The Image Field - Part 1

products/widgets.py 
```
from django.forms.widgets import ClearableFileInput
from django.utils.translation import gettext_lazy as _


class CustomClearableFileInput(ClearableFileInput):
    clear_checkbox_label = _('Remove')
    initial_text = _('Current Image')
    input_text = _('')
    template_name = 'products/custom_widget_templates/custom_clearable_file_input.html'

```

products/templates/products/custom_widget_templates/custom_clearable_file_input.html
```
{% if widget.is_initial %}
    <p>{{ widget.initial_text }}:</p>
    <a href="{{ widget.value.url }}">
        <img width="96" height="96" class="rounded shadow-sm" src="{{ widget.value.url }}">
    </a>
    {% if not widget.required %}
        <div class="custom-control custom-checkbox mt-2">
            <input class="custom-control-input" type="checkbox" name="{{ widget.checkbox_name }}" id="{{ widget.checkbox_id }}">
            <label class="custom-control-label text-danger" for="{{ widget.checkbox_id }}">{{ widget.clear_checkbox_label }}</label>
        </div>
    {% endif %}<br>
    {{ widget.input_text }}
{% endif %}
<span class="btn btn-black rounded-0 btn-file">
    Select Image <input id="new-image" type="{{ widget.type }}" name="{{ widget.name }}"{% include "django/forms/widgets/attrs.html" %}>
</span>
<strong><p class="text-danger" id="filename"></p></strong>
```

products/forms.py 
```
from django import forms
from .widgets import CustomClearableFileInput
from .models import Product, Category


class ProductForm(forms.ModelForm):

    class Meta:
        model = Product
        fields = '__all__'

    image = forms.ImageField(label='Image', required=False, widget=CustomClearableFileInput)

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        categories = Category.objects.all()
        friendly_names = [(c.id, c.get_friendly_name()) for c in categories]

        self.fields['category'].choices = friendly_names
        for field_name, field in self.fields.items():
            field.widget.attrs['class'] = 'border-black rounded-0'

```
- git add . 
- git commit -m "Product admin - Fixing the image field part 1"
- git push


### Fixing The Image Field - Part 2

static/css/base.css
```
html {
    height: 100%;
}

body {
    background: url('/media/homepage_background_cropped.jpg') no-repeat center center fixed;
    background-size: cover;
    height: calc(100vh - 164px);
    color: #555;
    font-family: 'Lato';
}

/* from Bulma */
.icon {
    align-items: center;
    display: inline-flex;
    justify-content: center;
    height: 1.5rem;
    width: 1.5rem;
}

.logo-font {
    text-transform: uppercase;
}

.main-logo-link {
    width: fit-content;
}

.shop-now-button {
    background: black;
    color: white;
    min-width: 260px;
}

.btn-black {
    background: black;
    color: white;
}

.btn-outline-black {
    background: white;
    color: black !important; /* use important to override link colors for <a> elements */
    border: 1px solid black;
}

.btn-outline-black:hover,
.btn-outline-black:active,
.btn-outline-black:focus {
    background: black;
    color: white !important;
}

.shop-now-button:hover,
.shop-now-button:active,
.shop-now-button:focus,
.btn-black:hover,
.btn-black:active,
.btn-black:focus {
    background: #222;
    color: white;
}

.text-black {
    color: #000 !important;
}

.border-black {
    border: 1px solid black !important;
}

.bg-black {
    background: #000 !important;
}

.overlay {
	height: 100%;
	width: 100%;
	top: 0;
	left: 0;
	position: fixed;
	background: white;
	z-index: -1;
}

a.category-badge > span.badge:hover {
    background: #212529 !important;
    color: #fff !important;
}

.btt-button {
    height: 42px;
    width: 42px;
    position: fixed;
    bottom: 10px;
    right: 10px;
}

.btt-link,
.update-link,
.remove-item {
    cursor: pointer;
}

input[name='q']::placeholder {
    color: #aab7c4;
}

/* ------------------------------- bootstrap toasts */

.message-container {
    position: fixed;
    top: 72px;
    right: 15px;
    z-index: 99999999999;
}

.custom-toast {
    overflow: visible;
}

.toast-capper {
    height: 2px;
}

/* from CSS-tricks.com: https://css-tricks.com/snippets/css/css-triangle/ */
.arrow-up {
    width: 0; 
    height: 0; 
    border-left: 4px solid transparent;
    border-right: 4px solid transparent; 
    border-bottom: 10px solid black;
    position: absolute;
    top: -10px;
    right: 36px;
}

/* Convenience classes - colors copied from Bootstrap */
.arrow-primary {
    border-bottom-color: #007bff !important;
}

.arrow-secondary {
    border-bottom-color: #6c757d !important;
}

.arrow-success {
    border-bottom-color: #28a745 !important;
}

.arrow-danger {
    border-bottom-color: #dc3545 !important;
}

.arrow-warning {
    border-bottom-color: #ffc107 !important;
}

.arrow-info {
    border-bottom-color: #17a2b8 !important;
}

.arrow-light {
    border-bottom-color: #f8f9fa !important;
}

.arrow-dark {
    border-bottom-color: #343a40 !important;
}

.bag-notification-wrapper {
    height: 100px;
    overflow-x: hidden;
    overflow-y: auto;
}

/* -------------------------------- Media Queries */

/* Slightly larger container on xl screens */
@media (min-width: 1200px) {
  .container {
    max-width: 80%;
  }
}

/* Allauth form formatting */

.allauth-form-inner-content p {
    margin-top: 1.5rem; /* mt-4 */
    color: #6c757d; /* text-secondary */
}

.allauth-form-inner-content input {
    border-color: #000;
    border-radius: 0;
}

.allauth-form-inner-content label:not([for='id_remember']) {
    display: none;
}

.allauth-form-inner-content input::placeholder {
    color: #aab7c4;
}

.allauth-form-inner-content button,
.allauth-form-inner-content input[type='submit'] {
	/* btn */
	display: inline-block;
    font-weight: 400;
    color: #fff;
    text-align: center;
    vertical-align: middle;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    background-color: #000;
    border: 1px solid #000;
    padding: .375rem .75rem;
    font-size: 1rem;
    line-height: 1.5;
    border-radius: 0;

    /* standard bootstrap btn transitions */
    transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
}

.allauth-form-inner-content button:hover,
.allauth-form-inner-content input[type='submit']:hover {	
	color: #fff;
    background-color: #222;
    border-color: #222;
}

.allauth-form-inner-content a {
	color: #17a2b8; /* text-info */
}

/* Product Form */

.btn-file {
    position: relative;
    overflow: hidden;
}

.btn-file input[type="file"] {
    position: absolute;
    top: 0;
    right: 0;
    min-width: 100%;
    min-height: 100%;
    opacity: 0;
    cursor: pointer;
}

.custom-checkbox .custom-control-label::before {
    border-radius: 0;
    border-color: #dc3545;
}

.custom-checkbox .custom-control-input:checked~.custom-control-label::before {
    background-color: #dc3545;
    border-color: #dc3545;
    border-radius: 0;
}


/* fixed top navbar only on medium and up */
@media (min-width: 992px) {
    .fixed-top-desktop-only {
        position: fixed;
        top: 0;
        right: 0;
        left: 0;
        z-index: 1030;
    }

    .header-container {
        padding-top: 164px;
    }
}

/* pad the top a bit when navbar is collapsed on mobile */
@media (max-width: 991px) {
    .header-container {
        padding-top: 116px;
    }

    body {
        height: calc(100vh - 116px);
    }
}
```

products/templates/products/edit_product.html
```
{% extends "base.html" %}
{% load static %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col-12 col-md-6">
                <hr>
                <h2 class="logo-font mb-4">Product Management</h2>
                <h5 class="text-muted">Edit a Product</h5>
                <hr>
            </div>
        </div>

        <div class="row">
            <div class="col-12 col-md-6">
                <form method="POST" action="{% url 'edit_product' product.id %}" class="form mb-2" enctype="multipart/form-data">
                    {% csrf_token %}
                    {% for field in form %}
                        {% if field.name != 'image' %}
                            {{ field | as_crispy_field }}
                        {% else %}
                            {{ field }}
                        {% endif %}
                    {% endfor %}
                    <div class="text-right">
                        <a class="btn btn-outline-black rounded-0" href="{% url 'products' %}">Cancel</a>
                        <button class="btn btn-black rounded-0" type="submit">Update Product</button>
                    </div>
                </form>
            </div>            
        </div>
    </div>
{% endblock %}

{% block postloadjs %}
    {{ block.super }}
    <script type="text/javascript">
        $('#new-image').change(function() {
            var file = $('#new-image')[0].files[0];
            $('#filename').text(`Image will be set to: ${file.name}`);
        });
    </script>
{% endblock %}
```

products/templates/products/add_product.html
```
{% extends "base.html" %}
{% load static %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col-12 col-md-6">
                <hr>
                <h2 class="logo-font mb-4">Product Management</h2>
                <h5 class="text-muted">Add a Product</h5>
                <hr>
            </div>
        </div>

        <div class="row">
            <div class="col-12 col-md-6">
                <form method="POST" action="{% url 'add_product' %}" class="form mb-2" enctype="multipart/form-data">
                    {% csrf_token %}
                    {% for field in form %}
                        {% if field.name != 'image' %}
                            {{ field | as_crispy_field }}
                        {% else %}
                            {{ field }}
                        {% endif %}
                    {% endfor %}
                    <div class="text-right">
                        <a class="btn btn-outline-black rounded-0" href="{% url 'products' %}">Cancel</a>
                        <button class="btn btn-black rounded-0" type="submit">Add Product</button>
                    </div>
                </form>
            </div>            
        </div>
    </div>
{% endblock %}

{% block postloadjs %}
    {{ block.super }}
    <script type="text/javascript">
        $('#new-image').change(function() {
            var file = $('#new-image')[0].files[0];
            $('#filename').text(`Image will be set to: ${file.name}`);
        });
    </script>
{% endblock %}
```
- git add . 
- git commit -m "Product Admin - Fixing the image field part 2"
- git push


### Creating a Heroku App

Heroku
- create new Heroku App
  - click: Create New App
  - App name: ci-django-boutique
  - Choose a region: Europe
  - click: Create App
- Resources
  - type in search field: Heroku Postgres
  - Plan name: Hobby Dev - Free
  - click: Submit Order Form

GitPod
- Terminal
  - pip3 install dj_database_url
  - pip3 install psycopg2-binary
  - pip3 freeze > requirements.txt

boutique/settings.py
```
import os
import dj_database_url


# DATABASES = {
#    'default': {
#        'ENGINE': 'django.db.backends.sqlite3',
#        'NAME': BASE_DIR / 'db.sqlite3',
#    }
# }


DATABASES = {
        'default': dj_database_url.parse()
}
```

Heroku
- Settings
  - Config Vars
  - Value from DATABASE_URL: postgres://...

boutique/settings.py
```
import os
import dj_database_url


# DATABASES = {
#    'default': {
#        'ENGINE': 'django.db.backends.sqlite3',
#        'NAME': BASE_DIR / 'db.sqlite3',
#    }
# }


DATABASES = {
        'default': dj_database_url.parse(postgres://...)
}
```

- unset PGHOSTADDR
- python3 manage.py showmigrations
- python3 manage.py migrate
- python3 manage.py loaddata categories
- python3 manage.py loaddata products
- python3 manage.py createsuperuser
  - Username: Admin
  - Email address: admin@admin.com
  - Password: superuser
  - Password (again): superuser

boutique/settings.py
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
- git add . 
- git commit -m "Deployment part 1"
- git push





- git add . 
- git commit -m "Product Admin - Fixing the image field part 2"
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