### How to enable Module Federation in your project

Since Module Federation (MF) in early stage and requires webpack 5 + some patched angular libraries 
we need to do a few steps to make it work.


### 1. Create angular project

```
 ng new content-item-app
```


### 2. Create new Folder structure

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
 
 - Add `package.json` and `lerna.json ` into your new project root   /mf-app-root/ 
 
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

- Install dependencies


### 2. Add initial configuration files

 - Add lerna.json to `/mf-app-root`
                    
 
   
   


