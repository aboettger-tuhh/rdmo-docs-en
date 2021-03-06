# Views

Views can be configured under *Views* in the management menu in the navigation bar.

![](../_static/img/screens/views.png)
> *Screenshot of the views management interface*

On the left-hand side is the main display of all the views available in this installation of RDMO. Views show their key, title and description. On the right side of each views panel, icons indicate ways to interact the element. The following options are available:

* **Update** (![](../_static/img/icons/update.png)) a view to change its properties.
* **Edit the template** (![](../_static/img/icons/template.png)) of a view.
* **Delete** (![](../_static/img/icons/delete.png)) a view. **This action cannot be undone!**

The sidebar on the right shows additional interface items:

* **Filter** filters the view according to a user given string. Only views containing this string in their path will be shown.
* **Options** offers additional operations:

  * Create a new view

* **Export** exports the conditions to one of the displayed formats. While the textual formats are mainly for presentation purposes, the XML export can be used to transfer the views to a different installation of RDMO.

Views have different properties to control their behavior. As descibed in [the introduction](index.html), all elements have an URI prefix, a key, and an internal comment only to be seen by other managers of the RDMO installation. In addition, you can edit the parameters below:

## Parameters
### View

|**Tab English**||
|-|-|
|Title|The English title for the view. The title will be shown in the projects overview.|
|Help|The English help text for the view. The help text will be shown in the projects overview|
|**Tab German**|*contains the same elements as the English one but obviously for German language content*|
|**Tab Groups**|
|Groups|Displays the groups for this view. If at least one group is selected, only users of these<br> groups will see this view for a project.|
|**Tab Sites**|
|Sites| *(Only in a multi site installation)* Displays the sites for this view. Only users of these<br> groups will see this view for a project.|

## View Templates

![](../_static/img/screens/template.png)
> *Screenshot of the template modal*

Each view has a template, which determines how the answers given by the user are mapped to a textual document. The template is composed using the [Django template](https://docs.djangoproject.com/en/stable/ref/templates/language/) syntax, which is a combination of regular HTML, variables, which get replaced with values when the template is evaluated (`{{ a_variable }}`), and tags, which control the logic of the template (`{% a_tag %}`).

In the first line of the view template you will find the load command for the available view tags. It makes the logic of the template available and should always be there.

```django
{% load view_tags %}
```

Immediately afterwards there will probably be variable declarations which load sets into place holders and make them available throughout the whole template. These variables can for example be used in for loops as you will see later.

```django
{% get_set 'project/partner' as partners %}
{% get_set 'project/dataset' as datasets %}
```

Consider an attribute `project/research_question/title` and a user, who answered the question connected to this attribute with "To boldly go where no man has gone before.". The attribute would be available in the template as `project/research_question/title`.

```django
The main research question of the project is: {% render_value 'project/research_question/title' %}
```

would, when evaluated in the context by a user in his/her project, render:

```django
The main research question of the project is: To boldly go where no man has gone before.
```

Lists of multiple values can also be rendered.

```django
<p>
    {% render_value_inline_list 'project/research_question/keywords' %}
</p>
```

As equivalent for the snippet above you can also use the following which gives you more control over the list layout.
```django
<ul>
{% get_values 'project/research_question/keywords' set_index=0 as text %}
    {% for value in text %}
        <li>{{ value.value }}</li>
    {% endfor %}
</ul>
```

For set entities, you can use the initially declared variables. Your code would look like this.

```django
{% for dataset in datasets %}
    <p>
        {% render_set_value dataset 'project/dataset/id' %}
    </p>
{% endfor %}
```

Values can be used if they meet certain conditions. If you want to display something based on a certain value being `true` you can for example do this. Note that there is an `.is_false` function as well which can be used just as the mentioned counterpart.

```django
{% get_value 'project/dataset/sharing/yesno' as val %}
{% if val.is_true %}
    This will be only rendered if personal_data resolves to be true.
{% endif %}
```

Or checking a value within a dataset.

```django
{% for dataset in datasets %}
    {% get_set_value dataset 'project/dataset/id' as val %}
    {% if val.is_true %}
        {% render_set_value dataset 'project/dataset/id' %}
    {% endif %}
{% endfor %}
```

Please consult the documentation of the Django template syntax for all the available tags and filters: https://docs.djangoproject.com/en/stable/ref/templates/language.
