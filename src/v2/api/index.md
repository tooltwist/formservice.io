---
title: API
type: api
---

## selectMappedView (GET)

Select records of a specified type

``` js
let result = await getMappedView(context, viewname, options)
```

The returned value contains a list of record from the specified view. Without a [where](#where) clause all records in the view will be selected.

```js
let result = await getMappedView(context, 'thing', { })
// {
//   "data": [
//       {
//           "id": 4,
//           "description": "trashcan",
//           "status": "uncool",
//           "category": "household",
//           "subcategory": "kitchen",
//       },
//       ...
//   ]
// }
```

Related information can also be selected using the options described below.

#### REST equivalent

```bash
curl http://127.0.0.1:4000/api/APIKEY/1.0/thing
```

Options can be passed as JSON in the body of the request, or as URL query
parameters as described below.

### where
Similar to the _where_ clause of an SQL statement, the records to select can be
specified...

``` js
let result = await getMappedView(context, 'thing_category', {
  where: { first_name: 'Fred', last_name: 'Snurgs' }
})
```

Or more explicitly

``` js
let result = await getMappedView(context, 'thing_category', {
  where: {
    first_name: { value: 'Fred', op: 'eq' },
    last_name: {value: 'Snurgs', op: 'eq' }
  }
})
```

Or in shortcut form

``` js
let result = await getMappedView(context, 'thing_category', {
  first_name: 'Fred',
  last_name: 'Snurgs'
})
```

#### REST equivalent

```bash
curl http://127.0.0.1:4000/api/APIKEY/1.0/thing/4?first_name=Fred&last_name=Snurgs
```

For primary key access, a specific record can be accessed by providing the correct number of primary key values to the URL.


```bash
curl http://127.0.0.1:4000/api/APIKEY/1.0/thing/4

curl http://127.0.0.1:4000/api/APIKEY/1.0/thing_status/cool

curl http://127.0.0.1:4000/api/APIKEY/1.0/thing_category/household/kitchen
```



### refs

In the schema we can define that the field(s) in one view refer to the primary key of another view. With the *refs* option we can load either just the description, or all the fields of the views referenced by the selected records.

In the following example, the status field in the selected _thing_ record refers to the <i>thing_status</i> record with primary key 'cool'. Since 'desc' is specified in the _refs_ option, the description field in the <i>thing_status</i> record is returned.

The _thing_ record also refers to a <i>thing_category</i> record via a multi-part primary key (category,subcategory). For this view the _refs_ option specifies 'all', so all the fields in the <i>thing_category</i> view are returned.

``` js
let result = await getMappedView(context, 'thing_category', {
  "refs": "status=desc,categorization=all"
})
// {
//   "data": [
//       {
//           "id": 4,
//           "description": "trashcan",
//           "status": "uncool",
//           "category": "household",
//           "subcategory": "kitchen",
//           "_references": {
//               "status": "Mock those in possession of such an object",
//               "categorization": {
//                   "category": "household",
//                   "subcategory": "kitchen",
//                   "description": "Things in the kitchen"
//               }
//           }
//       }
//   ]
// }
```

The mode is typically 'desc' or 'all', but if left out will default to 'desc'.

To select details from all referenced records, _refs_ can be specified as simply 'desc' or 'all', and this mode will be applied to all references.

``` js
let result = await getMappedView(context, 'thing_category', {
  refs: 'desc'
})
// {
//     "data": [
//         {
//             "id": 4,
//             "description": "trashcan",
//             "status": "uncool",
//             "category": "household",
//             "subcategory": "kitchen",
//             "_references": {
//                 "status": "Mock those in possession of such an object",
//                 "categorization": "Things in the kitchen"
//             }
//         }
//     ]
// }
```
Avoid using this shortcut excessively or when not needed, as there is an overhead to selecting from related datbase tables.

#### REST equivalent

```bash
curl http://127.0.0.1:4000/api/APIKEY/1.0/thing/4?refs=status=desc,categorization=all

curl http://127.0.0.1:4000/api/APIKEY/1.0/thing/4?refs=desc
```




### options
The `option` option is similar to the _refs_ option, except that instead of returning details of a specific record that is referenced, it returns _all_ the records of the referenced type.

This is primarily used when the selected record is about to be displayed and edited on a user interface, and we want to know the allowable values for a dropdown or select box.

In the following example we want to know the possible status codes for a _thing_ being edited.

Note that the options are included separately to the data records, as the same values will apply to all records.



``` js
let result = await getMappedView(context, 'thing_category', {
	"options": "status"
})
// {
//     "data": [
//         {
//             "id": 4,
//             "description": "trashcan",
//             "status": "uncool",
//             "category": "household",
//             "subcategory": "kitchen"
//         }
//     ],
//     "options": {
//         "status": [
//             {
//                 "key": "nocomment",
//                 "desc": "Have no opinion on this subject"
//             },
//             {
//                 "key": "uncool",
//                 "desc": "Mock those in possession of such an object"
//             },
//             {
//                 "key": "cool",
//                 "desc": "Object is worthy"
//             },
//             {
//                 "key": "huh?",
//                 "desc": "Who cares?"
//             }
//         ]
//     }
// }
```


#### REST equivalent

```bash
curl http://127.0.0.1:4000/api/APIKEY/1.0/thing/4?options=status
```



### usedBy
This option will include in the return data, a list of records that "use" (i.e. refer to) the record being selected.

#### Single field primary key
If the view being selected has a single primary key field, then _referenceName_ will be the name of a field that refers to this view. For example, in this schema, _thing.status_ refers to a <i>thing_status</i> record, which has a single field primary key.

```js
"thing": {
  fields: {
    ...
    status: '*thing_status.status',
  }
},
"thing_status": {
  fields: {
    status: 'primary',
    ...
  }
},
```

If we are selecting <i>thing_status</i> we can also select the _thing_ records that reference it as follows.

``` js
let result = await getMappedView(context, 'thing', {
  where: { status: 'cool' },
  usedBy: "thing.status=desc"
})
// {
//     "data": [
//         {
//             "status": "cool",
//             "description": "Object is worthy",
//             "is_good": 1,
//             "_usedBy": {
//                 "thing.status": [
//                     {
//                         "id": 2,
//                         "description": "spoon"
//                     },
//                     {
//                         "id": 5,
//                         "description": "fidgetspinner"
//                     },
//                     ...
//                 ]
//             }
//         }
//     ]
// }
```


#### Multi-field primary key
If this view has a multi-part primary key, then we use the _reference alias_ to define the records to include.

In this example's schema, _thing_ refers to a <i>thing_category</i> record via two fields referencing a two part primary key. The reference alias 'categorization' is used to clarify this relationship.

```js
"thing": {
  fields: {
    ...
    category: '*thing_category(categorization).category',
    subcategory: '*thing_category(categorization).subcategory',
  }
},

"thing_category": {
  fields: {
    category: 'primary',
    subcategory: 'primary',
    ...
  }
},
```


``` js
let result = await getMappedView(context, 'thing_category', {
  where: {
    category: "household",
    subcategory: "kitchen"
  },
  usedBy: "thing.categorization=desc"
})
// {
//     "data": [
//         {
//             "category": "household",
//             "subcategory": "kitchen",
//             "description": "Things in the kitchen",
//             "_usedBy": {
//                 "thing.categorization": [
//                     {
//                         "id": 1,
//                         "description": "table"
//                     },
//                     {
//                         "id": 2,
//                         "description": "spoon"
//                     },
//                     {
//                         "id": 4,
//                         "description": "trashcan"
//                     }
//                 ]
//             }
//         }
//     ]
// }
```

#### Only one reference
If there is only one reference from the referring table to the table being selected, then the field name of reference alias can be excluded. For example, there is only one field in the _thing_ view that refers to the <i>thing_category</i> view, so the following can be used.

``` js
let result = await getMappedView(context, 'thing_category', {
  where: {
    category: "household",
    subcategory: "kitchen"
  },
  usedBy: "thing"        <--- No field name or alias
})
```
Be careful using this form - if you ever add a second reference between these two tables, then it is not possible to determine which reference is required, and existing code will throw an exception.


#### REST equivalent

```bash
http://127.0.0.1:3000/api/ABC123/v1.0/thing_status/cool?usedBy=thing.status
```





## insertMappedView (POST)


## updateMappedView (PUT)


## deleteMappedView (DEL)
