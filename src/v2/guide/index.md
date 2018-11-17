---
title: Formservice.io
type: guide
order: 2
---


## Architecture

Formservice is designed to be embedded into applications.

To provide flexibility to accommodate as many application architectures as possible, Formservice is provided as multiple _functional_ components, with variations for the deployment of each.


 <center>![Architecture](/images/formservice/architecture-1.png)</center>


Notice that these are _functional_ components, and in some cases there may be alternates in the software used to provide that functionality. Some of these components are Open Source, and if you wish you are free to create your own version of one of these components,

The glue that holds the parts together is the use of a common API protocol.


### Default (simplest) configuration
The default configuration is to have your application let the ToolTwist servers do all the work. Configure your application to use https://formservice.io then log in to your Tooltwist account, where you can define the data you want to store and design your forms.

<center>![](/images/formservice/default-config-1.png)</center>

### Data and Meta-data
#### Schema
#### Views of that schema (Forms)
#### UI-first designing





## Client Side

### Webpage
<center>![](/images/formservice/client-vue-formservice-1.png)</center>

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
