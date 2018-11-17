---
title: Contentservice.io
type: guide
order: 2
---


## Adding to a project

### Creating a new project (in seconds)

Could not be much simpler...

```bash
vue create my-project
cd my-project
vue add contentservice
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

### Add to a VueJS project

Assuming you have a working VueJS project (similar to one created by Vue CLI):

- Update `package.json` and add

```vue
"dependencies": {
  ...
  "vue-contentservice": "^0.1.76"
  "@fortawesome/fontawesome-free": "^5.4.2",
  "bulma": "^0.7.2",
  ...
}
```

- Import these packages into your `main.js`:

```vue
import ContentService from "vue-contentservice"

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

### Managing your own content

The code so far has used the APIKEY of the Content Service demonstration account. This content can be edited by anyone, and gets reset periodically.

To display edit your own content, create an account on the [Tooltwist](http://a3-dev.authservice.io/) website and get your own APIKEY.

The content that is displayed on a page is specified by the 'anchor' attribute, that acts as a key to identify your specific content. You can use almost any value for the anchor, but we suggest you create a simple naming convention to keep things simple, for example

`application.page.layout`



## Managed Content

### Full Page Content Management

The examples so far have all allowed full page content management, allowing 'widgets' to be added to the page.

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

#### The Left pane

When a page is being edited, an extra pane appears on the right, containing a toolbox of widgets and allowing you to enter properties for widgets already on the page. This right pane can be resized by dragging it's border.

In some cases you may wish to have a similar pane on the left side of the page. This can be done simply by a slot to your template.

```html
<template>
  <div id="app">
      <content-layout-editor :editable="editable" :anchor="anchor">

        <template slot="left-pane">
          This is a pane on the left!
        </template>

      </content-layout-editor>
  </div>
</template>

<script>
export default {
  name: 'ContentWithLeftPane',
  data: function () {
    return {
      editable: true,
      anchor: 'contentservice.example.layout',
    }
  }
}
</script>
```

#### Styling the page

The generated code includes classes that you may define:

Class |	Notes
--- | ---
.c-triple-pane | The entire content area, including the side panes.
.c-editing-layout | Set when in content management mode.
.c-not-editing-layout | Normal mode, when not laying out the page.
.c-has-left-pane | Set when the left pane is being used.
.c-left-pane | Left pane area
.c-middle-pane | Use with care.
.c-right-pane | Use with care.

### Remote Content Management

In some cases you may wish to display managed content on your site, but want that content management to occur on the site itself. In this case you can use Remote Content Management.

With Remote Content Management (RCM) you can log in to your [Tooltwist](http://a3-dev.authservice.io/) account and modify the content from there. Your changes will "magically" appear on your website.

Content management from the site itself is easy, so why would you want to do this?

a) It may be because you don't want the site to have the 'weight' of the logic required to edit pages (i.e. faster load times, or lower memory usage)

b) You do not want to add login functionality to your site, or

c) You prefer the tighter security of the Tooltwist admin console, and want to make it as hard as possible for hackers.

#### Adding RCM to your own site

If your application already has an administration section, or if you don't want your users to log into a Tooltwist admin account (because they might modify the wrong thing!) then it makes sense to provide Content Management functionality in your admin website.

Content Service provides components to make this easy.

### Only Adding Specific Components to a Page

Widgets for ContentService are written such that they can operation in three modes:

- Embedded within a page layout, where the definition of the content is provides from the layout definition, which comes from the ContentService server.
- Standalone, where the component gets it's content comes the ContentService server.
- Standalone, where the program provides the content.



## Using your own widgets



## Creating an NPM Package

#### Reusable Widget Library

1. [ContentService.io](https://contentservice.io/) adds CMS functionality to VueJS and Nuxt projects.
2. ContentService can be extended with custom *widgets*, allowing you to create reusable, application-specific widgets.
3. This project provides a template for creating **npm** packages of reusable custom widgets.

#### What You Get
When you use this template, you will get an npm package that can be used in VueJS and Nuxt projects:

- Example widget added to the toolbox on pages that use contentservice.io.
- Example Vuex store.
- Example code calling a remote service.
- Example code to read config files.
- Example Nuxt plugin.
- Build scripts.
- A local test harness.
- Testing framework.

### Getting Started

#### Quick Start

Clone this project, and push it to your own repo, then play at will:
```bash
git clone https://github.com/tooltwist/vue-whatever.git myProject
cd myProject
rm -rf .git; git init; git add .; git commit -m 'Cloned from vue-whatever'
git remote add origin <your repos URL>
git push --set-upstream origin master
```

Open the project in your edit and...

1. Globally substitute 'whatever' with your project name with the first character *in lower case*.
2. Globally substitute 'Whatever' with your project name with the first character *in upper case*.
3. git mv src/lib/Whatever.js src/lib/**Projectname**.js
4. git mv src/store/WhateverStore.js src/store/**Projectname**Store.js
5. git mv src/components/widgets/ContentWhatever.vue src/components/widgets/Content**Projectname**.vue
6. git mv src/components/widgets/ContentWhateverProps.vue src/components/widgets/Content**Projectname**Props.vue
7. Update the description in `package.json`
8. Modify `README.md` to explain what this widget library does.

You can test your widget using:

```bash
yarn install
yarn serve
```

To create new widgets, you can copy and modify the example widget files in src/components/widgets:

- ContentWhatever.vue = the component displayed on the page
- ContentWhateverProps.vue = a component to edit the properties when in editing mode.
- Register your new widget using `$content.registerWidget` inside `src/components/index.js`.

Commit to github, then publish at will:

```bash
yarn build-bundle
yarn patch-release
```

### Using this template

Use this template to create an npm package that can be added to VueJS or Nuxt projects that utilise [Contentservice.io](https://contentservice.io/).

#### Step 1 - Create your own empty repo in Github
For the sake of this documentation, we'll assume your new project will be called `vue-xxxx`.

#### Step 2 - Clone this template and save it as your own repository
```bash
cd /Development/Project   (or somewhere else)
git clone https://github.com/tooltwist/vue-whatever.git vue-xxxx
cd vue-xxxx
```
#### Step 3 - Clone this template and save it as your own repository
```bash
rm -r .git
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/tooltwist/vue-xxxx.git
git push -u origin master
```
#### Step 4 - Define your widgets


### Developing locally

Sometimes the best, or only, place to develop and debug a package is as it is being within another application.

The *"make change, publish to npm, pull to project"* process is too slow, and risks having a broken or debug version of the package being deployed by other users of the package.

A better solution is to clone the project directly into the node_modules directory of your project and make changes there.

#### Step 1 - Install your project
```bash
cd <projectDir>/node_modules  
rm -r vue-xxxx
git clone https://github.com/xxxx/vue-xxxx.git
cd vue-xxxx
npm install
npm run build-bundle
```

#### Step 2 - Option A
After each code change, rebuild the bundle.
```bash
npm run build-bundle
```

#### Step 2 - Option B
For many projects, this option will allow live reload of the application each time you change the code, however it will not republish the artifacts in the `dist` directory.
```bash
rm -r node_modules
```

Modify package.json, and change the main entry point from:
```vue
"main": "./dist/vue-xxxx.common.js",
```
to...
```vue
"main": "./src/components/index.js",
```

This option however will NOT update the CSS file `vue-xxxx/dist/vue-xxxx.css`. To rebuild this file you will need to run `npm install` and `npm run build-bundle` again.

#### Beware
If you run `npm-install` in your project directory, it will overwrite any changes you've made to your package.

Do not check in `package.json` if you've made the changes mentioned in Option 2B.

If you are using option 2B and see the error `Module build failed: TypeError: this.setDynamic is not a function` then remove the node_modules directory *in your vue-xxx package* directory (not the main project) and restart your server.


### Creating widgets

Widgets required two Vue files:

1. The widget as it is displayed on a page
2. A properties entry area, where the user can enter details while editing the page.
