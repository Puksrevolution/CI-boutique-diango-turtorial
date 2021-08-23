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