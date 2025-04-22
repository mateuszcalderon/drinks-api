<p align="center">
  <img src="https://github.com/devicons/devicon/blob/master/icons/djangorest/djangorest-original.svg" height="60" width="60" style="margin-right: 20px;">
  <img src="https://github.com/devicons/devicon/blob/master/icons/python/python-original.svg" height="60" width="60">
</p>

<div align="center">
  <h1>RESTful Drinks API</h1>
</div>

The "RESTful Drinks API" is designed to efficiently manage a comprehensive collection of beverages. Developed with Django REST Framework, this API supports full CRUD (Create, Read, Update, Delete) operations, enabling seamless interaction with drink data, which includes attributes such as name, description and price. This robust API framework ensures ease of integration with other applications while maintaining the flexibility to operate as a standalone service.

## Resources:
This project was developed with guidance from [Caleb Curry](https://www.youtube.com/watch?v=i5JykvxUk_A), whose tutorial provided invaluable insights.

## Code Walkthrough:
**Note:** This Code Walkthrough includes only the key files from my project. It does not cover all files or implementation details. For a complete view, please refer to the full project on my GitHub profile.

#### urls.py:
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
    - ` path('drinks/', views.drink_list) `: Routes requests to the ` drink_list ` view function in the ` views ` module of the ` drinks_app ` application.
    - ` path('drinks/<int:id>/', views.drink_detail) `: Routes requests to the view function named ` drink_detail ` in the ` views ` module, capturing an integer parameter named `id`.

#### settings.py:
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

  - This section lists all applications that are activated in my Django project, including:
    - Built-in Django apps like ` admin `, ` auth `, and ` sessions `.
    - My custom app, ` drinks_app `, which contains the views and models for drink details.
    - ` rest_framework `, which is the Django REST Framework used to build APIs.
  - The ` settings.py ` file also contains other critical configurations, including ` DATABASES `, ` MIDDLEWARE `, and ` TEMPLATES `. These settings are vital for the overall functionality and security of the project, but they are not directly related to specific features of all Django application.

#### admin.py:
```python
  from django.contrib import admin
  from .models import Drink
  admin.site.register(Drink)
```

  - The ` admin.py ` file registers the Drink model to the Django admin site, allowing to manage drinks data via the Django admin interface.

#### models.py:
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

#### views.py:
```python
  from django.http import JsonResponse
  from .models import Drink
  from .serializers import DrinkSerializer
  from rest_framework.decorators import api_view
  from rest_framework.response import Response
  from rest_framework import status
```

  - ` JsonResponse `: Used for returning JSON responses.
  - ` Drink ` model and ` DrinkSerializer ` are imported from other ` drinks_app ` files.
  - Includes REST Framework decorators, response, and status modules.

```python
  @api_view(['GET', 'POST'])
  def drink_list(request, format=None):
    if request.method == 'GET':
      drinks = Drink.objects.all()
      serializer = DrinkSerializer(drinks, many=True)
      return Response(serializer.data)
    if request.method == 'POST':
      serializer = DrinkSerializer(data=request.data)
      if serializer.is_valid():
        serializer.save()
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```

  - This view handles two HTTP methods: ` GET ` and ` POST `.
    - **GET:** Fetches all drinks from the database, serializes them, and returns the data.
    - **POST:** Accepts data to create a new drink, validates it with the serializer, and saves the new drink if valid.

```python
  @api_view(['GET', 'PUT', 'DELETE'])
  def drink_detail(request, id, format=None):
    try:
      drink = Drink.objects.get(pk=id)
    except Drink.DoesNotExist:
      return Response(status=status.HTTP_404_NOT_FOUND)
    if request.method == 'GET':
      serializer = DrinkSerializer(drink)
      return Response(serializer.data)
    elif request.method == 'PUT':
      serializer = DrinkSerializer(drink, data=request.data)
      if serializer.is_valid():
        serializer.save()
        return Response(serializer.data)
      return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
    elif request.method == 'DELETE':
      drink.delete()
      return Response(status=status.HTTP_204_NO_CONTENT)
```

  - This view handles three HTTP methods: ` GET `, ` PUT `, and ` DELETE `.
    - **GET:** Fetches a specific drink by its ` id `, serializes it, and returns the data.
    - **PUT:** Updates the details of a specific drink. It validates the input data, saves it if valid, and returns the updated data.
    - **DELETE:** Deletes the specified drink and returns a ` 204 No Content status `.

## Development Environment:
This project was built using the following tools and their respective versions:
  - **Programming Language:** Python 3.13.1
  - **Editor:** Visual Studio Code 1.97.2
  - **Package Manager:** pip 24.3.1
  - **Framework:**
    - Django: 5.1.6
    - Django REST Framework: 3.15.2

## Acknowledgment:
A huge shoutout to Caleb Curry for his amazing [Django REST Framework - Build an API from Scratch](https://www.youtube.com/watch?v=i5JykvxUk_A) video. His clear guidance simplified complex concepts and made understanding this project and its concepts much easier.

## Contact:
Feel free to reach out to me with any questions, suggestions, or feedback!<br/>
  * [GitHub](https://github.com/mateuszcalderon)
  * [Instagram](https://www.instagram.com/mateuszcalderon/)
  * [LinkedIn](https://www.linkedin.com/in/mateuszcalderonreis/)
