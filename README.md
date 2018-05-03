[![Build Status](https://travis-ci.org/fabiocaccamo/django-treenode.svg?branch=master)](https://travis-ci.org/fabiocaccamo/django-treenode)
[![coverage](https://codecov.io/gh/fabiocaccamo/django-treenode/branch/master/graph/badge.svg)](https://codecov.io/gh/fabiocaccamo/django-treenode)
[![Code Health](https://landscape.io/github/fabiocaccamo/django-treenode/master/landscape.svg?style=flat)](https://landscape.io/github/fabiocaccamo/django-treenode/master)
[![Requirements Status](https://requires.io/github/fabiocaccamo/django-treenode/requirements.svg?branch=master)](https://requires.io/github/fabiocaccamo/django-treenode/requirements/?branch=master)
[![PyPI version](https://badge.fury.io/py/django-treenode.svg)](https://badge.fury.io/py/django-treenode)
[![Py versions](https://img.shields.io/pypi/pyversions/django-treenode.svg)](https://img.shields.io/pypi/pyversions/django-treenode.svg)
[![License](https://img.shields.io/pypi/l/django-treenode.svg)](https://img.shields.io/pypi/l/django-treenode.svg)

# django-treenode
Probably the best abstract model / admin for your **tree** based stuff.

## Features
- **Fast** - get `children`, `descendants`, `depth`, `index`, `level`, `parents`, `root`, `siblings`, `tree`, ... *(max 1 query)*
- **Synced** - in-memory model instances are automatically updated *(0 queries)*
- **Compatibility** - you can easily add treenode to existing projects
- **Easy configuration** - just extend the abstract model / model-admin
- **Admin integration** - great tree visualization with optional accordion
- **No dependencies**

## Requirements
- Python 2.7, 3.4, 3.5, 3.6
- Django 1.8, 1.9, 1.10, 1.11, 2.0

## Installation
- Run `pip install django-treenode`
- Add `treenode` to `settings.INSTALLED_APPS`
- Make your model inherit from `treenode.models.TreeNodeModel` *(described below)*
- Make your model-admin inherit from `treenode.admin.TreeNodeModelAdmin` *(described below)*
- Run `python manage.py makemigrations` and `python manage.py migrate`

## Configuration
#### `models.py`
Make your model class inherit from `treenode.models.TreeNodeModel`:

```python
from django.db import models

from treenode.models import TreeNodeModel


class Category(TreeNodeModel):

    # the field used to display the model instance
    # default value 'pk'
    treenode_display_field = 'name'

    name = models.CharField(max_length=50)

    class Meta(TreeNodeModel.Meta):
        verbose_name = 'Category'
        verbose_name_plural = 'Categories'
```

The `TreeNodeModel` abstract class adds many fields and public methods to your models.

All fields are prefixed with `tn_` to prevent direct access and avoid conflicts with possible existing fields.

If you want to access public methods as properties just make your model class inherit from both `TreeNodeModel` and `TreeNodeProperties`:

```python
# ...

from treenode.models import TreeNodeModel, TreeNodeProperties


class Category(TreeNodeModel, TreeNodeProperties):

    # ...
```

---

#### `admin.py`
Make your model-admin class inherit from `treenode.admin.TreeNodeModelAdmin`.

```python
from django.contrib import admin

from treenode.admin import TreeNodeModelAdmin
from treenode.forms import TreenodeForm

from .models import Category


class CategoryAdmin(TreeNodeModelAdmin):

    # if True an accordion will be used in the changelist view
    # default value False
    treenode_accordion = True

    # use TreenodeForm to automatically exclude invalid parent choices
    form = TreenodeForm

admin.site.register(Category, CategoryAdmin)
```

## Usage

#### Methods/Properties:
*Note that properties are available only if your model implements* `treenode.models.TreeNodeProperties` *(for more info check the configuration section)*

Get a **list containing all children** *(1 query)*:
```python
obj.get_children()
# or
obj.children
```

Get the **children count** *(0 queries)*:
```python
obj.get_children_count()
# or
obj.children_count
```

Get the **children queryset** *(0 queries)*:
```python
obj.get_children_queryset()
```

Get the **node depth** (how many levels of descendants) *(0 queries)*:
```python
obj.get_depth()
# or
obj.depth
```

Get a **list containing all descendants** *(1 query)*:
```python
obj.get_descendants()
# or
obj.descendants
```

Get the **descendants count** *(0 queries)*:
```python
obj.get_descendants_count()
# or
obj.descendants_count
```

Get the **descendants queryset** *(0 queries)*:
```python
obj.get_descendants_queryset()
```

Get a **n-dimensional** `dict` representing the **model tree** *(1 query)*:
```python
obj.get_descendants_tree()
# or
obj.descendants_tree
```

Get a **multiline** `string` representing the **model tree** *(1 query)*:
```python
obj.get_descendants_tree_display()
# or
obj.descendants_tree_display
```

Get the **node index** (index in node.parent.children list) *(0 queries)*:
```python
obj.get_index()
# or
obj.index
```

Get the **node level** (starting from 1) *(0 queries)*:
```python
obj.get_level()
# or
obj.level
```

Get the **order value** used for ordering *(0 queries)*:
```python
obj.get_order()
# or
obj.order
```

Get the **parent node** *(1 query)*:
```python
obj.get_parent()
# or
obj.parent
```

Set the **parent node** *(1 query)*:
```python
obj.set_parent(parent_obj)
```

Get a **list with all parents** (ordered from root to parent) *(1 query)*:
```python
obj.get_parents()
# or
obj.parents
```

Get the **parents count** *(0 queries)*:
```python
obj.get_parents_count()
# or
obj.parents_count
```

Get the **parents queryset** *(0 queries)*:
```python
obj.get_parents_queryset()
```

Get the **node priority** *(0 queries)*:
```python
obj.get_priority()
# or
obj.priority
```

Set the **node priority** *(1 query)*:
```python
obj.set_priority(100)
```

Get the **root node** for the current node *(1 query)*:
```python
obj.get_root()
# or
obj.root
```

Get **all root nodes** *(1 query)*:
```python
cls.get_roots()
# or
cls.roots
```

Get a **list with all the siblings** *(1 query)*:
```python
obj.get_siblings()
# or
obj.siblings
```

Get the **siblings count** *(0 queries)*:
```python
obj.get_siblings_count()
# or
obj.siblings_count
```

Get the **siblings queryset** *(0 queries)*:
```python
obj.get_siblings_queryset()
```

Get a **n-dimensional** `dict` representing the **model tree** *(1 query)*:
```python
cls.get_tree()
# or
cls.tree
```

Get a **multiline** `string` representing the **model tree** *(1 query)*:
```python
cls.get_tree_display()
# or
cls.tree_display
```

Return `True` if the current node is the **first child** *(0 queries)*:
```python
obj.is_first_child()
```

Return `True` if the current node is the **last child** *(0 queries)*:
```python
obj.is_last_child()
```

---

#### Update tree manually:
Useful after **bulk updates**:
```python
cls.update_tree()
```

## License
Released under [MIT License](LICENSE.txt).
