# Cache foreign keys values in the changelist admin page
http://blog.ionelmc.ro/2012/01/19/tweaks-for-making-django-admin-faster/

```python
class MyAdmin(admin.ModelAdmin):
    
    list_editable = 'myfield',
    
    def formfield_for_dbfield(self, db_field, **kwargs):
        request = kwargs['request']
        formfield = super(MyAdmin, self).formfield_for_dbfield(db_field, **kwargs)

        if db_field.name == 'myfield':
            choices = getattr(request, '_myfield_choices_cache', None)
            if choices is None:
                request._myfield_choices_cache = choices = list(formfield.choices)
            formfield.choices = choices

        return formfield
  ```
