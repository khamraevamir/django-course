## POST запрос создания новой записи в таблице

1. В директории приложения в папке **templates/app** создайте **course-create.html**
```html
{% extends 'base.html' %}

{% block content %}

<h1>Создание нового курса</h1>
<hr>

<form action="{% url 'create-course' %}" method="POST">
    {% csrf_token %}
    <p>
        <label>Категория</label>
        <select name="category_id">
            {% for category in categories %}
            <option value="{{category.id}}">{{category.title}}</option>
            {% endfor %}
        </select>
    </p>
    <p>
        <label for="">Название курса</label>
        <input name="title" type="text" placeholder="Например: Python...">
    </p>
    <p>
        <label for="">Описание курса</label>
        <textarea name="content" rows="5" placeholder="Lorem ipsum..."></textarea>
    </p>
    <p>
        <label for="">Цена</label>
        <input name="price" type="number" value="0">
    </p>

    <button>Создать новый курс</button>
</form>

{% endblock %}

```

2. В директории приложения в файле **views.py** создайте функцию для создания нового курса через POST request

```python
def course_create(request):
    # Сработает только тогда, когда мы нажимаем на кнопку находящаяся в теге </form> который имеет method='POST'
    if request.method == 'POST':
        category_id = request.POST.get('category_id')
        title = request.POST.get('title')
        content = request.POST.get('content')
        price = request.POST.get('price')

        category = Category.objects.get(id=category_id)

        course = Course()
        course.title = title
        course.content = content
        course.price = price
        course.category = category
        course.save()

        return redirect('index')
  # Сработает когда мы просто переходим по ссылке через тег </a>
    else:
        categories = Category.objects.all()
        context = {
            'categories': categories
        }
        return render(request, 'app/course-create.html', context=context)
```

3. В директории приложения в файле **urls.py** добавьте url для ранее созданной функции
```python
from . import views
from django.urls import path

urlpatterns = [
    path('course-create/', views.course_create, name='course-create')
]
```

## POST запрос изменения записи в таблице

1. В директории приложения в папке **templates/app** создайте **course-edit.html**

```html
{% extends 'base.html' %}


{% block content %}

<h1>Изменение курса</h1>
<hr>

<form action="{% url 'course-edit' course_id=course.id %}" method="POST">
    {% csrf_token %}
    <p>
        <label>Категория</label>
        <select name="category_id">
            {% for category in categories %}
            <option value="{{category.id}}" {% if category == course.category %}selected{% endif %}>
                {{category.title}}
            </option>
            {% endfor %}
        </select>
    </p>
    <p>
        <label for="">Название курса</label>
        <input name="title" type="text" value="{{course.title}}"
               placeholder="Например: Python...">
    </p>
    <p>
        <label for="">Описание курса</label>
        <textarea name="content" rows="5"
                  placeholder="Lorem ipsum...">{{course.content}}</textarea>
    </p>
    <p>
        <label for="">Цена</label>
        <input name="price" type="number" value="{{course.price}}">
    </p>

    <button>Изменить курс</button>
</form>

{% endblock %}
```

2. В директории приложения в файле **views.py** создайте функцию для изменения курса через POST request
```python
def edit_course(request, course_id):
    if request.method == 'POST':
        category_id = request.POST.get('category_id')
        title = request.POST.get('title')
        content = request.POST.get('content')
        price = request.POST.get('price')

        category = Category.objects.get(id=category_id)

        course = Course.objects.get(id=course_id)
        course.title = title
        course.content = content
        course.price = price
        course.category = category
        course.save()

        return redirect('index')
    else:
        course = Course.objects.get(id=course_id)
        categories = Category.objects.all()

        context = {
            "course": course,
            'categories': categories
        }
        return render(request, 'app/course-edit.html', context=context)


```

3. В директории приложения в файле **urls.py** добавьте url для ранее созданной функции
```python
from . import views
from django.urls import path

urlpatterns = [
    path('course-create/', views.course_create, name='course-create')
    path('course-edit/<int:course_id>/', views.course_edit, name='course-edit'),
]
```
