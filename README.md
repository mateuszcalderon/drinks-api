<p align="center">
  <img src="https://github.com/devicons/devicon/blob/master/icons/djangorest/djangorest-original.svg" height="60" width="60" style="margin-right: 20px;">
  <img src="https://github.com/devicons/devicon/blob/master/icons/python/python-original.svg" height="60" width="60">
</p>

## Code Walkthrough:
#### drinks_project:
#### urls.py
```python
  from django.contrib import admin
  from django.urls import path
  from drinks_app import views
  from rest_framework.urlpatterns import format_suffix_patterns

  urlpatterns = [
    path('admin/', admin.site.urls),
    path('drinks/', views.drink_list),
    path('drinks/<int:id>/', views.drink_detail),
  ]

  urlpatterns = format_suffix_patterns(urlpatterns)
```

  - The ` urls.py ` file is responsible for routing incoming HTTP requests to the appropriate view functions.
    - ` path('admin/', admin.site.urls) `: Routes requests to the Django admin interface.
    - ` path('drinks/', views.drink_list) `: Routes requests to a view function named drink_list in the views module of the drinks_app application.
    - ` path('drinks/<int:id>/', views.drink_detail) `: Routes requests to the view function named ` drink_detail ` in the ` views ` module, capturing an integer parameter named `id`.

#### settings.py
```python
  INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'drinks_app',
    'rest_framework',
  ]
```

  - This section lists all applications that are activated in your Django project, including:
    - Built-in Django apps like ` admin `, ` auth `, and ` sessions `.
    - My custom app, ` drinks_app `, which contains the views and models for drink details.
    - ` rest_framework `, which is the Django REST Framework used to build APIs.
    - The ` settings.py ` file also contains other critical configurations, including ` DATABASES `, ` MIDDLEWARE `, and ` TEMPLATES `. These settings are vital for the overall functionality and security of the project, but they are not directly related to the specific features of your application.

#### drinks_app:
#### admin.py
```python
  from django.contrib import admin
  from .models import Drink

  admin.site.register(Drink)
```

  - The ` admin.py ` file registers the Drink model to the Django admin site, allowing you to manage your drinks data via the Django admin interface.

#### models.py
```python
  from django.db import models

  class Drink(models.Model):
    name = models.CharField(max_length=200)
    description = models.CharField(max_length=200)
    price = models.DecimalField(max_digits=5, decimal_places=2)

    def __str__(self):
      return self.name + ', ' + self.description + ', $' + str(self.price)
```

  - This file defines the ` Drink ` model, which includes the following fields:
    - ` name `, ` description `, and ` price ` are variables representing the respective attributes of the Drink model.
    - ` CharField ` and ` DecimalField ` are used to define the type and constraints of these fields in the database.
