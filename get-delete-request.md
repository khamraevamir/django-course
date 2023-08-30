## GET запрос для получения одного объекта (не список)

1. В директории приложения в файле **views.py** создайте функцию для получения всей информации об курсе
```python
def course_detail(request, course_id):
    # Извлекаем курс из списка курсов по id
    course = Course.objects.get(id=course_id)
    context = {
        "course": course,
    }
    return render(request, 'app/course-detail.html', context=context)
```

2. В директории приложени в файле urls.py пропишите следующее:
```python
from . import views
from django.urls import path

urlpatterns = [
    path('', views.index, name='index'),
    path('about/', views.about, name='about'),
    # url с аргументом course_id с типом данных int(integer)
    path('course-detail/<int:course_id>/', views.course_detail, name='course-detail')
]
```

3. В директории приложения **app/templates** создайте **course-detail.html** и пропишите следующее

```html
{% extends 'base.html' %}


{% block content %}

<h1>{{course.title}}</h1>
<small>Добавлено: {{course.created_at}}</small>
<hr>
<p>
  {{course.content}}
</p>
<h3>Price: {{course.price}}</h3>

{% endblock %}
```

## DELETE запрос для удаления объекта

1. В директории приложения в файле **views.py** создайте функцию для удаления курса
```python
# other packages ....
from django.shortcuts import render, redirect

def delete_course(request, course_id):
    course = Course.objects.get(id=course_id)
    course.delete()
    return redirect('index')
```

2. В директории приложени в файле urls.py пропишите следующее:
```python
from . import views
from django.urls import path

urlpatterns = [
    path('', views.index, name='index'),
    path('about/', views.about, name='about'),
    # url с аргументом course_id с типом данных int(integer)
    path('course-detail/<int:course_id>/', views.course_detail, name='course-detail'),
    path('delete-course/<int:course_id>/', views.delete_course, name='delete-course')
]
```



3. В директории приложения **app/templates** обновите **course-detail.html** и прописав следующее

```html
{% extends 'base.html' %}


{% block content %}

<h1>{{course.title}}</h1>
<small>Добавлено: {{course.created_at}}</small>
<hr>
<p>
  {{course.content}}
</p>
<h3>Price: {{course.price}}</h3>

// Кнопка для удалени курса
<div class="d-flex">
  <a href="{% url 'delete-course' course_id=course.id %}" class="btn btn-danger ml-auto d-block">Удалить курс</a>
</div>
{% endblock %}
```
