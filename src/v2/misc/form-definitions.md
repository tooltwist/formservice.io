---
title: Designing Forms
type: misc
order: 2
---


## Dumb Forms
Dumb forms are created by dragging widgets onto your form, and giving a field name to each of the input fields. When you press the submit button, these fields will be sent to the backend as a regular HTML form.

Whatever you send to the server will be save as-is.

Dumb forms allow you to quickly whip up a web page that collects basic information (such as a "contact" form), but must be used with care because the backend does not validate what is being saved. If your form changes over time your data may end up with different combinations of fields, containing unvalidated data.

## Schemas
A _schema_ is a definition of the data that will be used in your forms, and allows the server to validate the data being saved, and allows the form designer to intelligently place known fields on the form.

A schema can also define the relationship between different types of data, so you can easily include dropdown lists, searching, and autocomplete fields of your forms.

If you have an existing relational database, a schema can also be used to map onto the columns and tables of that database.

The schema will define each of the _views_ and _fields_ of your application. Views are similar to database tables, but we don't call them tables because some views are used to display information that is never actually stored. However, in most cases views and fields do actually map onto [and get stored in] tables and columns.

To keep things simple, we stick with the convention of calling data views and fields when it defines the data displayed on forms, and tables and columns when we actually store that data in a database.

>    As a security concern, we try to avoid passing information about tables and columns to the front end.

For each view several things need to be defined:
#### The Primary Key
The view must have a unique primary key that can used to reference a specific record. This primary key may be one or more fields.

Where no unique primary key is apparent, you can use an auto-increment field to provide a key.

#### Description Fields
When we display a list of records on the screen, we typically want to display some, but not all, or the fields of the table. For example if we are displaying a list of products, we can display the product code (the primary key), but will probably also want to display the product description, which is more user-relevant information.

In our schema we can define a field as being a _description fields_. Multiple fields can be included in the description, however having too many fields may result in

#### Linking between tables
The fields in a view may refer to another view. For example, a field that allows the user to select 'country' would typically reference a _country_ table.

#### Searchable Fields

## Form-first Design


## Schema-first Design
