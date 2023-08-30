1. В директории приложения app в файле **models.py** создайте модель **Course**

```python
from django.db import models


class Course(models.Model):
    # optional arg for CharField: unique=True
    title = models.CharField('title', max_length=256) # короткий текст
    content = models.TextField('content') # большой текст
    price = models.IntegerField('price', default=0) # Числа
    image = models.ImageField('image', upload_to='course-image') # Картинки
    is_published = models.BooleanField('is published', default=False) # Буленова значение
    created_at = models.DateTimeField('created at') # Дата со временем

    class Meta:
        # Поле в джанго админке - Добавить Курс +
        verbose_name = 'Курс'
        # Название модели в джанго админке
        verbose_name_plural = 'Курсы'

    # Строковое представление объекта для читабельности 
    def __str__(self):
        return self.title
```


2. Если в модели присутствует тип данных ImageField, то для работы с картинкой нужно установить библиотеку **Pillow**


3. После создания модели нужно прописать команду
```
# Создане модели для работы с бд
python manage.py makemigrations

# Миграция модели(создние таблицы в бд по шаблону модели)
python manage.py migrate
```

4. После успешной миграции, модель нужно зарегистрировать в **admin.py** в директории приложения где была создана модель
```python
from django.contrib import admin
from .models import Course

admin.site.register(Course)
```

5. Создайте супер пользователя для входа в админ панельку
```
python manage.py createsuperuser
# admin
# admin@gmai.com
# admin
# admin
# y
```

6. Для передача данных из бд в страницу нужно прописать в файле **views.py** следующее:
```python
from django.shortcuts import render
from .models import Course
from datetime import datetime

# Измененный код
def index(request):
    courses = Course.objects.all() # Все записи таблицы Course
    # Упаковка для передачи данных в html
    context = {
        "courses": courses,
        "date": datetime.now()
    }
    return render(request, 'app/index.html', context=context)


def about(request):
    return render(request, 'app/about.html')

```

7. Для принятия данных в страницы пропишите следующее:
```html
{% extends 'base.html' %}


{% block content %}

<h1>Courses</h1>
<hr>
<small>{{date}}</small>


<div class="row mt-5">

    {% for course in courses %}
    <div class="col-sm-6">
        <div class="card">
            <!--            <img src="{{course.image.url}}" class="card-img-top " alt="...">-->
            <div class="card-body">
                <h5 class="card-title">{{course.title}}</h5>
                <p class="card-text">
                    {% if course.content|length > 100 %}
                        {{ course.content|slice:":100" }}...
                    {% else %}
                        {{ course.content }}
                    {% endif %}
                </p>
                <a href="#" class="btn btn-primary">Посмотреть</a>
            </div>
        </div>
    </div>
    {% endfor %}

</div>


{% endblock %}
```

