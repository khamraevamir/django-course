from django.db import models


class Category(models.Model):
    title = models.CharField('title', max_length=256)

    class Meta:
        verbose_name = 'Категория'
        verbose_name_plural = 'Категории'

    def __str__(self):
        return self.title


class Course(models.Model):
    # ForeignKey - связка с другой таблицей
    # 1. on_delete.models.CASCADE - Если родителя удалить, то все объекты связанные с этим родителем удялятся
    # 2. on_delete.models.SET_NULL - Если родителя удалить, то все объекты связанные с этим родителем сохранятся
    # 3. Если выйдет ошибка связанная non-nullable field, добавьте null-True
    course = models.ForeignKey(Category, verbose_name='Категория', on_delete=models.CASCADE, null=True)
    title = models.CharField('title', max_length=256)
    content = models.TextField('content')
    price = models.IntegerField('price', default=0)
    image = models.ImageField('image', upload_to='course-image', blank=True)
    is_published = models.BooleanField('is published', default=False)
    created_at = models.DateTimeField('created at')

    class Meta:
        verbose_name = 'Курс'
        verbose_name_plural = 'Курсы'

    def __str__(self):
        return self.title
