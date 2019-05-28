# Editing related objects in the admin

Taken from the [Django docs](https://docs.djangoproject.com/en/1.11/ref/contrib/admin/#django.contrib.admin.TabularInline).


Suppose you have two related models
```
from django.db import models

class Author(models.Model):
   name = models.CharField(max_length=100)

class Book(models.Model):
   author = models.ForeignKey(Author, on_delete=models.CASCADE)
   title = models.CharField(max_length=100)
```

You can edit the `Book` from the `AuthorAdmin` page.

```
from django.contrib import admin

class BookInline(admin.TabularInline):
    model = Book

class AuthorAdmin(admin.ModelAdmin):
    inlines = [
        BookInline,
    ]
```

Combined with `ModelAdmin.save_formset`, you can perform conditional logic based on the related object you are saving.

```
class AuthorAdmin(admin.ModelAdmin):
    def save_formset(self, request, form, formset, change):
        # save_formset called for each formset

        instances = formset.save(commit=False)
        for instance in instances:
            if isinstance(instance, RelatedObjectA):
                instance.user = request.user
            instance.save()
        formset.save_m2m()
```

Inlines come in two flavors: `StackedInline` and `TabularInline`. You can optionally specify a `formset` for the inline which allows you to customize the usage of the related models. Possibilities: limit the queryset, specify whether you can add/delete, edit only certain fields etc.