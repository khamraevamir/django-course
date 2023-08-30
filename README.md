1. Создание проекта
```
django-admin startproject project_name
```

2. Запуск проекта
```
# cd folder_name - переход в папку из директории
cd project_name 
python manage.py runserver
```

```
myproject/
│
├── manage.py
│
├── myproject/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py

```

3. Создание приложения
```
python manage.py startapp app_name
```

```
myproject/
│
├── manage.py
│
├── myproject/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── app/
│   ├── migrations/
│   │   └── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── views.py
│   └── urls.py


```

4. В главной папке в файле **settings.py** обновляем код

```python
# settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_name' # Название проложения
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'], # Директория нахождения htlm страниц
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

LANGUAGE_CODE = 'ru' # Язык джанго

TIME_ZONE = 'Asia/Tashkent' # Часовой пояс
```


5. В главной директории проекта в файле **urls.py** обновляем код
```python
# urls.py

from django.contrib import admin
from django.urls import path, include

# Шкаф
urlpatterns = [
    # Полка admin
    path('admin/', admin.site.urls),
    # Полка api/app
    path('api/app/', include('app.urls'))
]
```

6. В директории приложения в файле **views.py** обновляем код
```python
# app_name/views.py

from django.shortcuts import render

# Функции для вызова API 
# Каждая фукция упаковывает данные с бд (если есть) в html страницу

def index(request):
    return render(request, 'app/index.html')

def about(request):
    return render(request, 'app/about.html')
```

7. В директории приложения создать файл **urls.py**
```python
# app_name/urls.py

from . import views
from django.urls import path

# Полка от шкафа
urlpatterns = [
    # книга main
    path('main/', views.index, name='index'),
    # книга about
    path('about/', views.index, name='about')
]
```

В директории проекта создать папку под названием **templates/base.html**

```html
# Шаблонизатор

<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
</head>
<body>
    <div class="bg-light">
        <nav class="navbar navbar-expand-lg navbar-light container ">
      <a class="navbar-brand" href="{% url 'index' %}">Skillbox</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="{% url 'about' %}">About <span class="sr-only">(current)</span></a>
          </li>

<!--          <li class="nav-item dropdown">-->
<!--            <a class="nav-link dropdown-toggle" href="#" role="button" data-toggle="dropdown" aria-expanded="false">-->
<!--              Dropdown-->
<!--            </a>-->
<!--            <div class="dropdown-menu">-->
<!--              <a class="dropdown-item" href="#">Action</a>-->
<!--              <a class="dropdown-item" href="#">Another action</a>-->
<!--              <div class="dropdown-divider"></div>-->
<!--              <a class="dropdown-item" href="#">Something else here</a>-->
<!--            </div>-->
<!--          </li>-->

        </ul>
        <form class="form-inline my-2 my-lg-0">
          <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
          <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
        </form>
      </div>
    </nav>
    </div>

   <div class="container py-3">
        {% block content %}


        {% endblock %}
   </div>

    <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>
</body>
</html>
```

В директории **templates** создайте директорию **app/** + **html страницы**


```html
// index.html

# extends - импортирует шаблон
{% extends 'base.html' %}


# Независимая часть кода которая указана в base.html
{% block content %}

<h1>Main page</h1>
<hr>
<a href="{% url 'about' %}">Перейти на страницу о нас</a>

{% endblock %}

```


```html
// about.html

# extends - импортирует шаблон
{% extends 'base.html' %}

# Независимая часть кода которая указана в base.html
{% block content %}

<h1>About page</h1>
<hr>
<a href="{% url 'index' %}">Перейти на главную страницу</a>

{% endblock %}

```

```
myproject/
│
├── manage.py # файл для запуска проекта
│
├── myproject/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py # Настройки проекта
│   ├── urls.py # Маршрутизатор
│   └── wsgi.py
│
├── app/
│   ├── migrations/
│   │   └── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── views.py # API функции
│   └── urls.py # Маршрутизатор
│
├── templates/ # Местоположение всех html страниц
│   ├── app/
│   │   ├── index.html
│   │   └── about.html
    └── base.html # Шаблонизатор
│
├── static/
│   ├── css/
│   ├── js/
│   └── images/
│
└── media/

```
