---
title: Schema
type: server
order: 2
---

Your schema file defines _views_ and their relationships with each other.

A simple view might be defined:

``` js
  "thing": {
    fields: {
      id: 'primary',
      description: 'description',
    }
  },
```

This definition above defines a view named "thing" with two fields.

The **primary** attribute indicates the unique key of the view. A view can have multiple primary fields, but it cannot have none.

The **description** attribute indicates a field in the view that a human will understand. For example a product ID of _BFL103ADW_ is meaningless, but a description _Beko 10kg Front Load Washing Machine_ is quite understandable. We'll discuss description fields more later.


Views by default map onto a database table and columns matching the view and field names, but these can be overridden. Note that it is perfectly valid to have multiple views mapped to the same database table.


## References

A field in a view may refer to a different view (In database terms, a _foreign key_):

``` js
  "thing": {
    fields: {
      ...
      status: '*thing_status',
    }
  },

  "thing_status": {
    fields: {
      status: 'primary',
      description: 'description'
    }
  },
```

In the example above, a _thing_ has a status field that points to a <i>thing_status</i> record. We can use this relationship for various purposes:

- to get the description of the status, or other information, from thing_status (see [refs](/v2/api#refs)).
- to find all possible values for the status field (see [options](/v2/api/#options)).
- to find all the things with a particular status (see [usedBy](/v2/api/#usedBy)).


## Multiple-field primary keys

If the view being referenced has a multi-field primary key, we need a more explicit notation for defining the relationship, to specify the exact field mapping.

``` js
  "thing": {
    fields: {
      ...
      category: '*thing_category.category',
      subcategory: '*thing_category.subcategory',
    }
  },

  "thing_category": {
    fields: {
      category: 'primary',
      subcategory: 'primary',
      description: 'description',
    }
  }
```

In cases where there are multiple references to the same target view, we can use a _reference alias_ to clarify the mappings. In this example a thing can have two subcategories, within the same category. we give these he aliases _cat1_ and _cat2_.

``` js
"thing": {
  fields: {
    ...
    category: '*thing_category(cat1).category,*thing_category(cat2).category',
    subcategory: '*thing_category(cat1).subcategory',
    subcategory2: '*thing_category(cat2).subcategory'
  }
}
```


## Data Types

## User Interface hints
The schema is also available to an application front-end, and can help the UI (User Interface) code decide how to display the information.

- **readonly**

## Modes
A User Interface will commonly show different information within a view at different times. For example, in a summary list only some fields will be displayed. Or, when editing, the primary key cannot be changed.

To handle this we use _modes_.

``` js
"thing": {
  ...
  modes: {
    list: 'id,description',
    add: true,
    edit: true
  }
}
```

In list mode we specify the fields we wish to see.

For add and edit modes, we simply specify that these exist (How this is interpreted is up to the front end code).
