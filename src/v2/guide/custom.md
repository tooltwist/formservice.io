---
title: Custom Form Elements
type: guide
order: 4
---





## Authentication



## Custom Form widgets








### Creating an NPM Package
ZZZZ


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
