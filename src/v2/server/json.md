---
title: JSON Views
type: server
order: 3
---

So far, all the views we have defined have been mapped to database tables.

Formservice also has the ability to save data in JSON format. This allows arbitrary data to be saved, without needing to be pre-defined, or for additional database tables to be created.

While this provides great flexibility it does have some limitations:

1. In some circumstances queries may run slow.
1. It does not get the full protections provided by a properly modelled database.

> Use JSON format with care. Where applicable it provides great flexibility and can save a LOT of time, but do not be tempted to use it beyond it's capabilities.

To define a view as JSON mode, add `storeAsJSON: true` to the view definition.

## The Key

JSON tables will always have a single primary key, named _key_.

The key can be provided when you insert a new record (i.e. application generated), or it can be automatically generated as follows...

``` js

  "order": {
    storeAsJSON: true,
    key: 'order-{{RANDOM}}',
    ...
  },
```

In the database many types of JSON are stored together. A prefix like 'order-' makes it easier to recognize data while debugging.


## Defining fields

Given that we are storing arbitrary JSON, it may not be possible to know all the data fields at the time the view is being defined. However where possible you should define fields that:

* will be used as selection criteria.
* that reference other views.
* the description field.

These fields will be indexed, with a maximum of ten fields.

```js
  "order": {
    storeAsJSON: true,
    key: 'order-{{RANDOM}}',
    fields: {
      comments: "description",
      order_date: null,
      customer_id: null,
      status: '*order_status.status'
    },
  },
```

## Paths

It is also possible to index nested data within the JSON.

For example, if the order above contains an object named `delivery_address`, which contains a field `street_name`, we can define it as a field. This street_name will then be indexed, allowing fast selections by that field.


``` js
  "order": {
    storeAsJSON: true,
    key: "order-{{RANDOM}}",
    fields: {
      ...
      "delivery_address.street_name": null
    },
  },
```

These "paths" may also contain array references, and follow the same conventions as the [select](/v2/api/#where) API.

## Re-indexing

If the order or number of fields is ever changed, the data in the view will need to be re-indexed.

This is done by running ***TO-BE-CONTINUED***.
