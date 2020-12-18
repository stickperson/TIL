# Adding non-model fields when using ModelAdmin

You may want to include extra (non-model) fields in the admin interface. When using `ModelAdmin`, you can specify the `form` to use. As always, you can use custom validation with a `clean_<field>` method on the form. 

```
class MyForm(forms.ModelForm):
    non_model_field_a = forms.CharField(required=False)

    def clean_non_model_field_a(self):
        # conditional logic based potentially on other fields
        pass


class MyAdmin(admin.ModelAdmin):
    form = MyForm

    def get_fieldsets(self, request, obj=None):
        # conditional rendering
        if obj:
            pass
        else:
            pass

    def save_model(self, request, obj, form, change):
        special_condition = True
        if special_condition:
            non_model_field_a = form.cleaned_data['non_model_field_a']
        ...
```