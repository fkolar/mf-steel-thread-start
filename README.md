### How to enable Module Federation in your project

Since Module Federation (MF) in early stage and requires webpack 5 + some patched angular libraries 
we need to do a few steps to make it work.


### 1. Create angular project

```
 ng new content-item-app
```


### 2. New folder structure

Having existing angular project `step1/content-item-app` for which you want to add MF support, you 
need to do following:

 - Introduce another folder level on top of your project with sub-folder packages/ so it looks like this:
 
 ```html
     /mf-app-root/                
                 packages/


```
 - Move your existing project to `packages` folder
 
 
```html
   /mf-app-root/                
               packages/
                    content-item-app/
                      



```   

 - Copy `ng9-module-federation-builder` into your project from this repository https://github.com/valorkin/sap-ng-mf/tree/master/ng9-module-federation-builder
 
  ```html
      /mf-app-root/                
                  packages/
                  ng9-module-federation-builder/
 
 ```
 
 
 - Add `package.json` and `lerna.json ` into your new project root  ` mf-app-root/` 
 
 **package.json**
 ```
{
  "name": "mf-root",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "private": true,
  "scripts": {
    "build": "lerna run build",
    "build:prod": "lerna run build -- --prod"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/valorkin/sap-ng-mf.git"
  },
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/valorkin/sap-ng-mf/issues"
  },
  "homepage": "https://github.com/valorkin/sap-ng-mf#readme",
  "devDependencies": {
    "lerna": "^3.22.1"
  }
}
```

 **lerna.json**

```
{
  "packages": [
    "packages/*"
  ],
  "version": "0.0.0"
}

```

- Install dependencies under `/mf-app-root`
 

```
#install lerna
npm ci

#npm install in all micro apps that are under /packages/
lerna bootstrap

npm i --prefix=ng9-module-federation-builder/
```


### 3. Update Microapps angular under `/mf-app-root/packages/content-item-app`

 Create new file `bootstrap.ts`s under:
 
```
/mf-app-root/packages/content-item-app/src/bootstrap.ts

```
with following content (this is copied content from main.ts):



```
import { AppModule } from './app/app.module';
import { environment } from './environments/environment';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { enableProdMode } from '@angular/core';

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));


```
                    
- Add following import into existing main.ts                    
 `step3/mf-app-root/packages/content-item-app/src/main.ts`
 
 ```
import('./bootstrap');

```
 
 -- Turn off Ivy in your Angular10 project's `step3/mf-app-root/packages/content-item-app/tsconfig.base.json`
 
 ```
 "angularCompilerOptions": {
    "enableIvy": false
  }
```
 
 
 
 ### 5. Build Micro-apps 
 
 Before we can build our project you need to now configure module federation to expose remote endpoints and create some 
 component that can be exposed as remote point. 
 
 - Create new component `DownloadComponent` in our angular project and add it to default `AppModule`
 - Expose this components as remote end-point inside:
  `step3/mf-app-root/ng9-module-federation-builder/webpack.config.js`
  
  you should have something like this:
  
```
const mfa = {
  ngConfigPath: '../packages/content-item-apps/angular.json',
  name: 'content-item-app',
  libName: 'mfa',
  port: 3002,
  publicPath: 'http://localhost:3002/',
  entryModule: '../packages/content-item-apps/src/app/app.module#AppModule',
  shared: ['@angular/core', '@angular/common', '@angular/router'],
  exposes: {
    './Download': '../packages/content-item-apps/src/app/download.component.ts's
  }
};
```

- Build remote endpoints

```
cd  packages/content-item-apps
ng builds

cd ../../ng9-module-federation-builder
npm run build
npm run serve:app


```


`Currently our Webpack configuration does not process scss files.` 

 

   
   


