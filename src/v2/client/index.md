---
title: Client Installation
type: client
order: 1
---


## Quick Start

**WIP***

```bash
vue create my-project
cd my-project
vue add contentservice
vue add formservice
```

You can then view the site in development mode:

```bash
yarn serve
```

Open your browser at http://localhost:8080.

To edit the content of the page press `control-option-escape` on a Mac or `control-alt-escape` on Windows.

> Note that the content shown is example data. Change it as you wish, but be aware that other people will see anything you enter, and also that the content gets reset regularly without warning so your changes may disappear.

To customise the site, or edit your own content, see the [ContentService](http://contentservice.io/) documentation.

For more information about this and other prefabricated application components, see the [ToolTwist](http://tooltwist.com/) website.

## Adding to a project

### Add to a VueJS project

Assuming you have a working VueJS project (similar to one created by Vue CLI):

- Update `package.json` and add

```vue
"dependencies": {
  ...
  "@tooltwist/vue-contentservice": "^0.1.76"
  "@tooltwist/vue-formservice": "^0.1.76"
  "@fortawesome/fontawesome-free": "^5.4.2",
  "bulma": "^0.7.2",
  ...
}
```

- Import these packages into your `main.js`:

```vue
import ContentService from "vue-contentservice"
import FormService from "vue-formservice"

require('font-awesome/css/font-awesome.min.css')
require('vue-contentservice/dist/vue-contentservice.css')
require('bulma/css/bulma.min.css')

Vue.use(ContentService, {
  //protocol: 'https',
  host: 'uat.crowdhound.io',
  //port: 443,
  version: '2.0',
  apikey: 'API10O0X1NS8FWUTO3FXKN15ZOR09',
  //froalaActivationKey: FroalaKey
  defaultIconPack: 'fa'
});
}
```

- Add pages to you app that have content management

```html
<template>
  <div id="app">
      <content-layout-editor :editable="editable" :anchor="anchor">
      </content-layout-editor>
  </div>
</template>

<script>
export default {
  name: 'ContentServiceExample',
  data: function () {
    return {
      editable: true,
      anchor: 'contentservice.example.layout',
    }
  }
}
</script>
```

### Adding to a webpage
Adding ContentService to a page can be done in three steps.

- Add VueJS to the page.

- Add [Content Service](http://contentservice.io/), [Bulma](https://bulma.io/), and [Font Awesome](https://fontawesome.com/) to the page.
```html
<!-- Bulma -->  
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.7.2/css/bulma.min.css">  
<!-- Font-awesome -->  
<script defer src="https://use.fontawesome.com/releases/v5.0.7/js/all.js"></script>  
<!-- Content Service -->  
<link rel="stylesheet" href="https://unpkg.com/vue-contentservice@latest/dist/vue-contentservice.css">  

<script src="https://unpkg.com/vue-contentservice@latest/dist/vue-contentservice.umd.js"></script>  
```

- Initialize Content Service
```vue
Vue.use(window.ContentService, {  
  //protocol: 'https',  
  host: 'uat.crowdhound.io',  
  //port: 443,  
  version: '2.0',  
  apikey: 'API10O0X1NS8FWUTO3FXKN15ZOR09',  
  //froalaActivationKey: FroalaKey  
});
```

- Place the content editing component on your page
```html
<div id="app">  
  <content-layout-editor :editable="editable" :anchor="anchor">   
  </content-layout-editor>  
</div>  
```

In this default mode, pressing `Control-Option-Escapeon` a Mac or `Control-Alt-Escape` on Windows, while on the page will enter layout editing mode.

See [this jsfiddle](https://jsfiddle.net/philcal/c1vjswgo/37/) for an example or if you wish to to experiment.

### Adding to a Nuxt project

Assuming you have a working [Nuxt](https://github.com/tooltwist/contentservice.io/wiki/nuxtjs.org) project:

- Update `package.json` to add
```vue
"dependencies": {
  ...
  "vue-contentservice": "^0.1.76"
  "@fortawesome/fontawesome-free": "^5.4.2",
  "bulma": "^0.7.2",
  ...
}
```

- Create a plugin in `plugins/vue-contentservice`:
```vue
import Vue from 'vue'
import Contentservice from 'vue-contentservice'

Vue.use(ContentService, {
  //protocol: 'https',
  host: 'uat.crowdhound.io',
  //port: 443,
  version: '2.0',
  apikey: 'API10O0X1NS8FWUTO3FXKN15ZOR09',
  //froalaActivationKey: FroalaKey
  defaultIconPack: 'fa'
});
```

- You can now add pages to your app that have content management at `pages/<pagename>.vue`:

```html
<template>
  <div id="app">
      <content-layout-editor :editable="editable" :anchor="anchor">
      </content-layout-editor>
  </div>
</template>

<script>
export default {
  name: 'ContentServiceExample',
  data: function () {
    return {
      leftPane: false,
      editable: true,
      anchor: 'contentservice.example.layout',
    }
  }
}
</script>
```

#### A more configurable option

To simplify deploying in different stages of development (test, UAT, staging, production) we recommend keeping the configuration external to the application. One way to do this is the place the config in a separate directory that can be overwritten by your build scripts as they deploy to different environments.

This approach also allows you to combine the configs for multiple plugins (for example, [LoginService](http://loginservice.io/), [ContentService](http://contentservice.io/) and [Crowdhound](http://crowdhound.io/)) into one config file.

```vue
import Vue from 'vue'

import Contentservice from 'vue-contentservice'

// Load the configuration.
// This file will be overwritten during Docker builds.
import Config from '../protected-config/websiteConfig'
import FroalaKey from '../protected-config/froalaKey'


let options = null
if (Config.contentservice) {
  options = {
    protocol: Config.contentservice.protocol,
    host: Config.contentservice.host,
    port: Config.contentservice.port,
    version: Config.contentservice.version,
    apikey: Config.contentservice.apikey,

    // Use font-awesome version 5
    defaultIconPack: 'fas',

    // If you choose to use Froala
    froalaActivationKey: FroalaKey
  }
} else {
  console.error('Missing contentservice configuration in protected-config/websiteConfig.js')
  console.error('Contentservice will be disabled.')
}

Vue.use(Contentservice, options)
```

A combined configuration can then go in protected-config/websiteConfig.js, something like this:

```
/*
 *  This file gets overwritten during production deployments,
 *  using a config file copied from /secure-config/website/overlay.
 */
module.exports = {
  website : {
    protocol: 'http',
    host: 'localhost',
    port: 3000,
  },
  api: {
    protocol: 'http',
    host: 'localhost',
    port: 8000,
    version: 'v1',
  },
  loginservice: {
    // Local testing
    host: 'localhost',
    port: 9090,
    apikey: 'API10O0X1NS8FWUTO3FXKN15ZOR09',
    version: 'v2',
    registerPath: '/mbc#new-user',
    forgotPath: '/change-password'
  },
  contentservice: {
    host: 'uat.crowdhound.io',
    version: '2.0',
    apikey: 'API10O0X1NS8FWUTO3FXKN15ZOR09',
    //froalaActivationKey: FroalaKey
  },
}
```

## Configuration

The example code so far has used the APIKEY of the Form Service demonstration account. This content can be edited by anyone, and gets reset periodically.

To edit and use your own forms, create an account on the [Tooltwist](http://a3-dev.authservice.io/) website and get your own APIKEY.

The content that is displayed on a page is specified by the 'anchor' attribute, that acts as a key to identify your specific content. You can use almost any value for the anchor, but we suggest you create a simple naming convention to keep things simple, for example

`application.page.layout`
