---
title: Introduction
type: guide
order: 1
---

Formservice provides instant browser and server-side support for entering data
and saving it in a database.

## Client Side
[Vue-contentservice](https://www.npmjs.com/package/@tooltwist/vue-contentservice) is an npm package that adds content management to a website.

[Vue-formservice](https://www.npmjs.com/package/@tooltwist/vue-contentservice) adds forms functionality to vue-contentservice.

With this combination forms can be quickly designed using drag and drop functionality, and save data to either a Formservice or other backend server.

<a href="/v2/client">More information about Formservice in the browser...</a>


## Server Side

Formservice [backend](https://www.npmjs.com/package/@tooltwist/formservice) is an npm package to provide simplified access to a MySQL database.

A schema file defines _Views_ that map to database tables, or can be stored as generic JSON.

The schema also defines relationships between tables, and hints for the UI about how fields are displayed.

An API allows programmatic and also [Restify](http://restify.com/) interfaces to add, update, select and delete view records.

When selecting data to display on a form, the reply can also include information from related tables (e.g. descriptions for status codes), lists of records (e.g. order items for an order), or values available for a field (e.g. values to populate a dropdown box).

**Please note** that the Formservice backend will greatly simplify many database tasks, but it is not intended to be a replacement for traditional database access. Use it where it provides benefit - it may satisfy your entire needs - but make sure you don't use it beyond it's utility.

<a href="/v2/server">More information about Formservice on the server...</a>


## Independant Components

The browser and server-side parts of Formservice work seamlessly together, but they can also be used independently. A Formservice designed form can pass data to a non-Formservice backend.

Similarly, the Formservice backend package can be a useful addition to any application, even if it does not have a front end.

A [Tooltwist](http://tooltwist.com) account includes hosting for both client-side form layouts, and also a ready-to-use Formservice backend.
