---
title: Formservice.io
type: guide
order: 2
---


## Architecture

### Basics
Lets start with the basics... you want to have a form where you enter information, and that information will get stored somewhere.


<center>![Basics](/images/formservice/basics-1.png)</center>


Assuming you want a modern web-based application, the form will typically be used on either a mobile device or web browser and the database will be "in the cloud" or, to the use non-marketing-speak, it will be "on a server somewhere".


> This architectural split is often called the _client_ and the _server_, but this sometimes causes confusion because the word "server" sometimes refers to a physical or virtual machine (e.g. a Dell server or AWS), to the web-server program (e.g. Apache, Tomcat, or NodeJS Express), or in other cases to the application code providing a service.
>
> To avoid confusion we'll often refer to the _front end_ and the _back end_. In each case we are talking about the _software_ in, or used by, your application.

As a software developer, you could write the form code by hand, and you could write the database code by hand. You are here looking at Formservice because you would presumably like an easier and more flexible way of doing things.

### Generic Code

The role of Formservice is to provide you with options to use pre-existing software for the above architecture, so you don't need to write your own. For the front end we provide a library that can be included into your application. For the backend you have quite a few options, ranging from using our cloud based service (don't write code, and don't install anything) through to a custom integration of Formservice functionality and your own code. More details about this later...

In order to make Formservice do what _your_ application requires, it needs to work with three types of information:

1. Definitions of your User Interface
2. Definitions of your application's data
3. Storing and accessing your Application's Data



### Views and Layouts
`Views` are the various user interfaces of your forms. Usually there will be multiple user interfaces that work on the same data. For example, when you displayed a _list_ of products, the user interface (UI) is different to when you display a _single_ product. If you provide a Formservice front end with _layout_ definitions that describe the user interface of each view, Formservice can create the user interface for you.

The easiest way to do this is with the Formservice design tool. If the layout definitions are stored in your backend, any UI changes can take effect immediately without modifying or re-deploying your website.


<center>![Designer](/images/formservice/designer-1.png)</center>



> At the current time, there is a front end library for websites named vue-formservice available via npm. Mobile applications can access the Formservice backend by calling the API directly. If you would like to create a new front end library, feel free to base it the open source vue-formservice, or contact us at ToolTwist for assistance.

### Data Schema

When Formservice understands more about the actual application data you wish to store, it can do a great deal more for you.

By defining the _schema_ of your application's data, you allow Formservice to...

* The Designer becomes easier to use, allowing relevant fields to be added to forms, with minimal definition.
* The Formservice front end libraries will create UIs with appropriate validations, and automatically provide dropdowns and lookups on fields that refer to data in other database tables.
* The Formservice backend is able to store the data for you, with indexing, and searchable fields, or alternately can store the data in an existing application database.


<center>![Schemas](/images/formservice/schemas-1.png)</center>


### Application Data

The Formservice backend can store your data for you, or it can intelligently store it in an existing application database.

[More details below] ZZZ

### Where data is Stored

Usually on the backend.

<center>![Data triad](/images/formservice/data-triad-1.png)</center>




## Development Order

### A quick word of warning
> Formservice makes some things so simple that it is easy to fall into bad habits. Formservice is NOT a replacement for traditional data modelling skills and a well thought out design.
>
> While Formservice has the ability to trivialise simple form creating and data storage, it also has the ability to work with existing databases and custom application code. Please do not try to use the simplifying features in places where they are not appropriate.
>
> Good data modelling and consideration to how and where data is stored is essential for the long term stability and performance of your application.


Each project starts with different requirements and available information, and developers have different skills or preferred approaches to problems. An enterprise level application may start by considering the corporate data requirements, whereas a lean startup may begin by prototyping user interfaces. In other projects you might start with existing data that you need to de-engineer.


While the best result in Formservice comes when you have data, schema, and views,  Formservice allows you to work "with what you have".

### User Interface driven Form Design


<center>![UI driven Form Design](/images/formservice/ui-first-1.png)</center>



### Data-Modelling driven Form Design

<center>![Data Modelling driven Form Design](/images/formservice/modelling-first-1.png)</center>



### Data driven Form Design

<center>![Data driven Form Design](/images/formservice/data-first-1.png)</center>





## Setup
Formservice is designed to be embedded into applications.

To provide flexibility to accommodate as many application architectures as possible, Formservice is provided as multiple _functional_ components, with variations for the deployment of each.


 <center>![Architecture](/images/formservice/architecture-1.png)</center>


Notice that these are _functional_ components, and in some cases there may be alternates in the software used to provide that functionality. Some of these components are Open Source, and if you wish you are free to create your own version of one of these components,

The glue that holds the parts together is the use of a common API protocol.


### Default (simplest) configuration
The default configuration is to have your application let the ToolTwist servers do all the work. Configure your application to use https://formservice.io then log in to your Tooltwist account, where you can define the data you want to store and design your forms.

<center>![Simplest Design](/images/formservice/magic-1.png)</center>


As far as your application code is concerned, all the form designing and the storage and retrieval of application data is handled by magic.

This simple configuration is suitable for many simple application requirements, such as a basic 'Contact Us' form on a website (not to be confused with a full CRM ticketing system!) where setting up a full database seems like overkill.



## Client Side Programming

### Webpage
<center>![](/images/formservice/client-vue-formservice-1.png)</center>

[vue-formservice](https://www.npmjs.com/package/vue-formservice)


### Mobile App
<center>![](/images/formservice/client-mobile-1.png)</center>




## Server Backend
### Use the Tooltwist servers
_Quick / Inexpensive / Performant_
As simple as specifying formservice.io in your configuration.

<center>![](/images/formservice/backend-formservice-io-1.png)</center>

### Self-hosting Formservice

<center>![](/images/formservice/backend-self-hosted-1.png)</center>


### Embedded in Your Server

<center>![](/images/formservice/backend-embedded-1.png)</center>


### Custom Server
If you wish, you implement the Formservice API in your own server, and have complete control over how data is stored.

<center>![](/images/formservice/backend-custom-1.png)</center>


## Data Storage

### Keyed access to JSON data

### NOSql Database

### Relational database

### Other
There's no requirement for you to use Formservice to store your data.

One option is to use Formservice to design forms and to use forms in your application, but let the application decide what to do with the data entered using those forms.

<center>![](/images/formservice/forms-only-1.png)</center>




## Designing forms/views


## Adding a Form Designer to your own site

If your application uses custom form widgets then the generic Tooltwist hosted form designer will not know about those widgets. In this case, the simplest solution is to embed the forms designer into an administrator-only page on your site.

ZZZ
