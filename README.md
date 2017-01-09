# Platform-Widget-Bundle

The Widget bundle provides the developer a way to define widgets in the user interface. 

## Table of Contents

- [Widget Definition](#widget-definition)
- [Widget Creation](#widget-creation)
- [Real World Example](#widget-creation)
- [Todo](#to-do)

## Widget Definition

A widget consists of a title and content meant to be displayed in a template in a specific position. Essentially, it allows the developer to customise templates without directly modifying the template files.

## Widget Creation

Widgets can be created in any bundle. They are defined the same way as any other services in Symfony using the Service Container. The developer will need to tag the service with the `ds.widget` tag in order for the system to pick it up as a widget. Finally, the developer will need to define a position in a template file, where the widget should be displayed.

**The widget class** `src/Gov/Bundle/BlogBundle/Widget/LatestPostsWidget.php`:

```php
<?php

namespace Gov\Bundle\BlogBundle\Widget;

use Ds\Bundle\WidgetBundle\Widget\Widget;

class LatestPostsWidget extends Widget
{
    public function getTitle()
    {
        return 'Latest Posts';
    }

    public function getContent(array $data = [])
    {
        return '<ul><li><a href="">Post 1</a></li><li><a href="">Post 2</a></li></ul>';
    }
}
```

**The widget service** `src/Acme/Bundle/TestBundle/Resources/config/services.yml`:

```yml
services:
    gov.blog.widget.latest_posts:
        parent: ds.widget.widget.abstract
        class: Gov\Bundle\BlogBundle\Widget\LatestPostsWidget
        tags:
            - { name: ds.widget, position: aside }
```

**The template position**:

```twig
<html>
    <body>
        <aside>
            {% for widget in ds_widgets({ position: 'aside' }) %}
                <h3>{{ widget.title }}</h3>
                {{ widget.content|raw }}
            {% endfor %}
        </aside>
    </body>
</html>
```

## Todo

**Introduce custom twig tag for widget positions.**
  
Example 1: 
{% position <position_name> with { variable: value } %}

Example 2: 
{% position <position_name> %}
    {{ widget.title }}
    {{ widget.content|raw }}
{% endposition %}

**Enable widgets to be defined as template or callbacks, instead of classes.**

