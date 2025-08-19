# Jinja2 Template Engine Reference Card

## Syntax Overview

### Delimiters

- `{{ }}` - Expression (outputs value)
- `{% %}` - Statement (control flow)
- `{# #}` - Comment (not rendered)
- `{%- -%}` - Strip whitespace before/after

### Escaping

- `{{ '{{' }}` - Literal output
- `{% raw %}...{% endraw %}` - Raw block (no processing)

## Variables and Expressions

### Basic Variable Output

```jinja2
{{ variable }}
{{ user.name }}
{{ items[0] }}
{{ dict['key'] }}
```

### Arithmetic Operations

```jinja2
{{ 5 + 3 }}        # Addition: 8
{{ 10 - 4 }}       # Subtraction: 6
{{ 3 * 4 }}        # Multiplication: 12
{{ 15 / 3 }}       # Division: 5.0
{{ 17 // 3 }}      # Floor division: 5
{{ 17 % 3 }}       # Modulo: 2
{{ 2 ** 3 }}       # Power: 8
```

### Comparison Operations

```jinja2
{{ 5 == 5 }}       # Equal: True
{{ 5 != 3 }}       # Not equal: True
{{ 5 > 3 }}        # Greater than: True
{{ 3 < 5 }}        # Less than: True
{{ 5 >= 5 }}       # Greater or equal: True
{{ 3 <= 5 }}       # Less or equal: True
```

### Logical Operations

```jinja2
{{ True and False }}    # AND: False
{{ True or False }}     # OR: True
{{ not True }}          # NOT: False
```

## Control Flow Statements

### If Statements

```jinja2
{% if condition %}
    Content if true
{% elif other_condition %}
    Content if other condition
{% else %}
    Content if false
{% endif %}

<!-- Inline if -->
{{ 'Yes' if condition else 'No' }}
```

### For Loops

```jinja2
{% for item in items %}
    {{ loop.index }}: {{ item }}
{% else %}
    No items found
{% endfor %}

<!-- With unpacking -->
{% for key, value in dict.items() %}
    {{ key }}: {{ value }}
{% endfor %}

<!-- With filtering -->
{% for user in users if user.active %}
    {{ user.name }}
{% endfor %}
```

### Loop Variables

|Variable        |Description                        |
|----------------|-----------------------------------|
|`loop.index`    |Current iteration (1-indexed)      |
|`loop.index0`   |Current iteration (0-indexed)      |
|`loop.revindex` |Reverse index (1-indexed)          |
|`loop.revindex0`|Reverse index (0-indexed)          |
|`loop.first`    |True if first iteration            |
|`loop.last`     |True if last iteration             |
|`loop.length`   |Total number of items              |
|`loop.cycle()`  |Cycle through values               |
|`loop.depth`    |Current recursion depth            |
|`loop.depth0`   |Current recursion depth (0-indexed)|

### Set Variables

```jinja2
{% set variable = value %}
{% set x, y = 1, 2 %}
{% set navigation %}
    <ul>...</ul>
{% endset %}
```

### With Statement (Context)

```jinja2
{% with %}
    {% set foo = 42 %}
    {{ foo }}
{% endwith %}

{% with foo = 42 %}
    {{ foo }}
{% endwith %}
```

## Built-in Filters

### String Filters

|Filter      |Description            |Example                           |
|------------|-----------------------|----------------------------------|
|`upper`     |Convert to uppercase   |`{{ name|upper }}`                |
|`lower`     |Convert to lowercase   |`{{ name|lower }}`                |
|`title`     |Title case             |`{{ name|title }}`                |
|`capitalize`|Capitalize first letter|`{{ name|capitalize }}`           |
|`strip`     |Remove whitespace      |`{{ text|strip }}`                |
|`length`    |Get length             |`{{ text|length }}`               |
|`reverse`   |Reverse string/list    |`{{ text|reverse }}`              |
|`replace`   |Replace substring      |`{{ text|replace('old', 'new') }}`|
|`truncate`  |Truncate text          |`{{ text|truncate(20) }}`         |
|`wordwrap`  |Wrap words             |`{{ text|wordwrap(40) }}`         |
|`indent`    |Indent lines           |`{{ text|indent(4) }}`            |
|`center`    |Center text            |`{{ text|center(50) }}`           |
|`urlize`    |Convert URLs to links  |`{{ text|urlize }}`               |
|`striptags` |Remove HTML tags       |`{{ html|striptags }}`            |

### Number Filters

|Filter  |Description       |Example                 |
|--------|------------------|------------------------|
|`abs`   |Absolute value    |`{{ -5|abs }}`          |
|`round` |Round number      |`{{ 3.14159|round(2) }}`|
|`int`   |Convert to integer|`{{ "42"|int }}`        |
|`float` |Convert to float  |`{{ "3.14"|float }}`    |
|`string`|Convert to string |`{{ 42|string }}`       |

### List/Dict Filters

|Filter   |Description        |Example                         |
|---------|-------------------|--------------------------------|
|`first`  |First item         |`{{ list|first }}`              |
|`last`   |Last item          |`{{ list|last }}`               |
|`random` |Random item        |`{{ list|random }}`             |
|`min`    |Minimum value      |`{{ list|min }}`                |
|`max`    |Maximum value      |`{{ list|max }}`                |
|`sum`    |Sum values         |`{{ list|sum }}`                |
|`sort`   |Sort items         |`{{ list|sort }}`               |
|`reverse`|Reverse order      |`{{ list|reverse }}`            |
|`unique` |Remove duplicates  |`{{ list|unique }}`             |
|`list`   |Convert to list    |`{{ dict|list }}`               |
|`join`   |Join with separator|`{{ list|join(', ') }}`         |
|`slice`  |Slice into groups  |`{{ list|slice(3) }}`           |
|`batch`  |Group into batches |`{{ list|batch(3) }}`           |
|`groupby`|Group by attribute |`{{ list|groupby('category') }}`|

### Date/Time Filters

|Filter    |Description    |Example                          |
|----------|---------------|---------------------------------|
|`strftime`|Format datetime|`{{ date|strftime('%Y-%m-%d') }}`|

### Other Filters

|Filter          |Description      |Example                    |
|----------------|-----------------|---------------------------|
|`default`       |Default value    |`{{ var|default('N/A') }}` |
|`escape`        |HTML escape      |`{{ html|escape }}`        |
|`safe`          |Mark as safe HTML|`{{ html|safe }}`          |
|`length`        |Get length       |`{{ item|length }}`        |
|`pprint`        |Pretty print     |`{{ data|pprint }}`        |
|`xmlattr`       |XML attributes   |`{{ attrs|xmlattr }}`      |
|`tojson`        |Convert to JSON  |`{{ data|tojson }}`        |
|`filesizeformat`|Format file size |`{{ size|filesizeformat }}`|

### Custom Filter Syntax

```jinja2
{{ value|filter }}
{{ value|filter(arg1, arg2) }}
{{ value|filter1|filter2 }}
```

## Built-in Tests

### Basic Tests

```jinja2
{% if variable is defined %}
{% if variable is undefined %}
{% if variable is none %}
{% if variable is number %}
{% if variable is string %}
{% if variable is sequence %}
{% if variable is mapping %}
{% if variable is callable %}
```

### Type Tests

|Test      |Description|Example                |
|----------|-----------|-----------------------|
|`boolean` |Is boolean |`{{ var is boolean }}` |
|`integer` |Is integer |`{{ var is integer }}` |
|`float`   |Is float   |`{{ var is float }}`   |
|`number`  |Is number  |`{{ var is number }}`  |
|`string`  |Is string  |`{{ var is string }}`  |
|`sequence`|Is sequence|`{{ var is sequence }}`|
|`mapping` |Is mapping |`{{ var is mapping }}` |
|`iterable`|Is iterable|`{{ var is iterable }}`|

### Value Tests

|Test         |Description   |Example                      |
|-------------|--------------|-----------------------------|
|`even`       |Is even number|`{{ num is even }}`          |
|`odd`        |Is odd number |`{{ num is odd }}`           |
|`divisibleby`|Divisible by N|`{{ num is divisibleby(3) }}`|
|`equalto`    |Equal to value|`{{ var is equalto(5) }}`    |
|`greaterthan`|Greater than  |`{{ var is greaterthan(5) }}`|
|`lessthan`   |Less than     |`{{ var is lessthan(5) }}`   |
|`lower`      |Is lowercase  |`{{ text is lower }}`        |
|`upper`      |Is uppercase  |`{{ text is upper }}`        |

## Global Functions

### Range Function

```jinja2
{% for i in range(5) %}         # 0, 1, 2, 3, 4
{% for i in range(1, 5) %}      # 1, 2, 3, 4
{% for i in range(0, 10, 2) %}  # 0, 2, 4, 6, 8
```

### Lipsum (Lorem Ipsum)

```jinja2
{{ lipsum(5) }}          # 5 paragraphs
{{ lipsum(5, html=True) }} # With HTML tags
```

### Dict Function

```jinja2
{% set d = dict(key1='value1', key2='value2') %}
{% set d = dict([('key1', 'value1'), ('key2', 'value2')]) %}
```

### Cycler Function

```jinja2
{% set row_class = cycler('odd', 'even') %}
{% for item in items %}
    <tr class="{{ row_class.next() }}">
        <td>{{ item }}</td>
    </tr>
{% endfor %}
```

### Joiner Function

```jinja2
{% set comma = joiner(", ") %}
{% for item in items %}
    {{ comma() }}{{ item }}
{% endfor %}
```

## Template Inheritance

### Base Template

```jinja2
<!-- base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}{% endblock %}</title>
    {% block head %}{% endblock %}
</head>
<body>
    {% block content %}{% endblock %}
</body>
</html>
```

### Child Template

```jinja2
<!-- child.html -->
{% extends "base.html" %}

{% block title %}Page Title{% endblock %}

{% block head %}
    {{ super() }}  <!-- Include parent content -->
    <link rel="stylesheet" href="style.css">
{% endblock %}

{% block content %}
    <h1>Content here</h1>
{% endblock %}
```

## Include and Macros

### Include

```jinja2
{% include 'header.html' %}
{% include 'template.html' ignore missing %}
{% include ['template1.html', 'template2.html'] %}
```

### Macros

```jinja2
<!-- Define macro -->
{% macro input(name, value='', type='text', size=20) %}
    <input type="{{ type }}" name="{{ name }}" 
           value="{{ value|e }}" size="{{ size }}">
{% endmacro %}

<!-- Use macro -->
{{ input('username') }}
{{ input('password', type='password') }}

<!-- Macro with content -->
{% macro render_dialog(title) %}
    <div class="dialog">
        <h1>{{ title }}</h1>
        <div class="contents">
            {{ caller() }}
        </div>
    </div>
{% endmacro %}

{% call render_dialog('Hello World') %}
    This is the dialog content.
{% endcall %}
```

### Import Macros

```jinja2
{% from 'forms.html' import input %}
{% from 'forms.html' import input as input_field %}
{% import 'forms.html' as forms %}
{{ forms.input('username') }}
```

## Advanced Features

### Assignments

```jinja2
{% set navigation = [
    ('index.html', 'Index'),
    ('about.html', 'About')
] %}

{% set class, id = 'foo', 'bar' %}
```

### Namespace Objects

```jinja2
{% set ns = namespace() %}
{% set ns.found = false %}
{% for item in items %}
    {% if item.check_something() %}
        {% set ns.found = true %}
    {% endif %}
{% endfor %}
```

### Whitespace Control

```jinja2
{%- if True %}
    yay
{%- endif %}

<!-- Remove all whitespace -->
{%- for item in seq -%}
    {{ item }}
{%- endfor -%}
```

### Line Statements

```jinja2
# for item in seq
    <li>{{ item }}</li>
# endfor

# set x = 42
```

### Escaping

```jinja2
{{ '{{' }}              # Literal {{
{% raw %}
    {% for item in seq %}
        <li>{{ item }}</li>
    {% endfor %}
{% endraw %}
```

## Common Patterns

### Conditional CSS Classes

```jinja2
<div class="item {{ 'active' if item.active else '' }}">
<div class="{{ 'error' if errors else 'success' }}">
```

### Default Values

```jinja2
{{ user.name or 'Anonymous' }}
{{ config.DEBUG|default(False) }}
```

### Safe HTML Output

```jinja2
{{ content|safe }}
{{ content|striptags|safe }}
```

### Loop with Separators

```jinja2
{% for item in items %}
    {{ item }}{% if not loop.last %}, {% endif %}
{% endfor %}

<!-- Or use join filter -->
{{ items|join(', ') }}
```

### Conditional Blocks

```jinja2
{% if users %}
    <ul>
    {% for user in users %}
        <li>{{ user.name }}</li>
    {% endfor %}
    </ul>
{% else %}
    <p>No users found.</p>
{% endif %}
```

### Template Comments

```jinja2
{# This is a comment #}
{# 
   Multi-line
   comment
#}
```

## Error Handling

### Ignore Missing Variables

```jinja2
{{ undefined_var|default('fallback') }}
```

### Try/Catch Pattern

```jinja2
{% set result = [] %}
{% for item in items %}
    {% if item.valid %}
        {% set _ = result.append(item) %}
    {% endif %}
{% endfor %}
```

## Best Practices

### Performance Tips

- Use `{% set %}` to avoid repeated calculations
- Prefer filters over complex logic in templates
- Use `{% include %}` for reusable components
- Cache expensive operations outside templates

### Security

- Always escape user input: `{{ user_input|e }}`
- Use `|safe` only for trusted content
- Be careful with `include` and dynamic template names

### Maintainability

- Keep logic in Python, presentation in templates
- Use meaningful variable names
- Comment complex template logic
- Organize templates in logical directory structure