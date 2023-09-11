## Django authentification - login | logout | sign up

### Login  - вход в аккаунт

1. В директории приложения в папке templates/app создайте **login.html**
```html
{% extends 'base.html' %}


{% block content %}

<h1>Аудентификация</h1>
<hr>

<form action="{% url 'login' %}" method="POST">
  {% csrf_token %}
  <p>
    <label for="">Логин</label>
    <input name="login" type="text">
  </p>
  <p>
    <label for="">Пароль</label>
    <input name="password" type="password">
  </p>
  <button>Войти</button>

  {% if error %}
  <h5>{{error}}</h5>
  {% endif %}

</form>

{% endblock %}
```

2. В директории приложения в файле views.py создайте функцию для входа в аккаунт
```python
from django.contrib.auth import authenticate, login as auth_login

def login(request):
    if request.method == 'POST':
        username = request.POST.get('login')
        password = request.POST.get('password')
        # Метод authenticate возвращает объект user если есть в базе данных иначе возвращает None 
        user = authenticate(request, username=username, password=password)
        # Если user существует
        if user is not None:
            auth_login(request, user)
            # Redirect to a success page.
            return redirect('index')  # Assuming you have a 'home' URL pattern
        # Иначе
        else:
            # Return an 'invalid login' error message.
            context = {'error': 'Invalid username or password'}
            return render(request, 'app/login.html', context)
    else:
        if request.user.is_authenticated:
            return redirect('index')
        return render(request, 'app/login.html')
```

3. В директории приложения в файле urls.py добавьте url для ранее созданной функции
```python
from . import views
from django.urls import path

urlpatterns = [
    path('login/', views.login, name='login'),
]
```

### Logout - выход из аккаунта

1. В директории приложения в файле views.py создайте функцию для вызода из аккаунта
```python
from django.contrib.auth import authenticate, login as auth_login, logout as auth_logout

def logout(request):
    auth_logout(request)
    return redirect('login')
```

2. В директории приложения в файле urls.py добавьте url для ранее созданной функции
```python
from . import views
from django.urls import path

urlpatterns = [
    path('login/', views.login, name='login'),
    path('logout/', views.logout, name='logout')
]
```

### Register - Создание нового аккаунта

1. В директории приложения в папке templates/app создайте **sign-up.html**
```html
{% extends 'base.html' %}


{% block content %}

<h1>Регистрицая</h1>
<hr>

<form action="{% url 'sign-up' %}" method="POST">
  {% csrf_token %}
  <p>
    <label for="">Имя</label>
    <input name="first_name" type="text">
  </p>
   <p>
    <label for="">Фамилия</label>
    <input name="last_name" type="text">
  <p>
  <p>
    <label for="">E-mail</label>
    <input name="email" type="email">
  </p>
    <p>
    <label for="">Придумайте логин</label>
    <input name="login" type="text">
  </p>
    <label for="">Придумайте пароль</label>
    <input name="password" type="password">
  </p>
  </p>
    <label for="">Введите пароль еще раз</label>
    <input name="password_2" type="password">
  </p>
  <button>Зарегестрироваться</button>

  {% if error %}
  <h5>{{error}}</h5>
  {% endif %}

</form>

{% endblock %}
```

2. В директории приложения в файле views.py создайте функцию для создания аккаунта
```python
def sign_up(request):
    if request.method == 'POST':
        first_name = request.POST.get('first_name')
        last_name = request.POST.get('last_name')
        email = request.POST.get('email')
        username = request.POST.get('login')
        password = request.POST.get('password')
        password_2 = request.POST.get('password_2')

        if password == password_2:
            if User.objects.filter(username=username).exists():
                context = {'error': 'Логин уже существует'}
                return render(request, 'app/register.html', context)
            elif User.objects.filter(email=email).exists():
                context = {'error': 'Email уже существует'}
                return render(request, 'app/register.html', context)
            else:
                user = User()
                user.username=username
                user.password=password
                user.email=email
                user.first_name=first_name
                user.last_name=last_name
                user.save()
                auth_login(request, user)
                return redirect('index')  
        else:
            context = {'error': 'Пароли не совпадают'}
            return render(request, 'app/register.html', context)
    else:
        return render(request, 'app/register.html')
```

3. В директории приложения в файле urls.py добавьте url для ранее созданной функции
```python
from . import views
from django.urls import path

urlpatterns = [
    path('login/', views.login, name='login'),
    path('logout/', views.logout, name='logout'),
    path('sign-up/', views.sign_up, name='sign-up')
]
```

