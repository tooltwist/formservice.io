---
title: Schema reference
type: schema
order: 1
---

## View Attributes

### fields
### modes
### table
### extensions


## Field Attributes
### primary
### desccription
### *reference
### label
### boolean
### pounds#
### dollars#
### readonly
### date
### column (might not work)
### virtual

1. Simple

2. Multi-field primary keys

### Modes

### UI Hints

### aliases

## Examples


``` js
  "thing": {
    fields: {
      id: 'primary',
      description: 'description',
      status: '*thing_status.status',
      category: '*thing_category(cat1).category,*thing_category(cat2).category',
      subcategory: '*thing_category(cat1).subcategory',
      subcategory2: '*thing_category(cat2).subcategory',
      order_id: '*order',
    }
  },

  "thing_status": {
    fields: {
      status: 'primary',
      description: 'description',
      is_good: null,
    }
  },


  "thing_category": {
    fields: {
      category: 'primary',
      subcategory: 'primary',
      description: 'description',
    }
  },

```
