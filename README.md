# Homey App Bundler

A shell script for Homey app developers to dramatically decrease installation times (the time it takes for `homey app run` to transfer and install the app on your Homey) by bundling all its resources into a single file.

## Prerequisites

Depends on [`@vercel/ncc`](https://github.com/vercel/ncc), which should be installed globally:
```
$ npm i -g @vercel/ncc
```

## Installation

Download the `homey-app-bundle` script and copy it to somewhere in your `$PATH`. Make sure it's executable:
```
$ chmod 755 /path/to/homey-app-bundle
```

## How to use

Instead of using `homey app run`, use `homey-app-bundle`

## What it does

The script creates a temporary directory to where it copies all app files, _except `node_modules` and `.homeybuild`_. It then runs `ncc` to bundle your `app.js` file including all its dependencies into a single file. Lastly, it will run `homey app run` from inside the temporary directory. Once you stop the app, the temporary directory will be removed automatically.

The script should not change anything in your local app directory.

## Disclaimer

Only tested on a _very_ limited subset of apps, so it will likely not work with all apps (especially SDKv2 apps are problematic at the moment). YMMV.

Known issues:
* apps that load requirements (using `require()`) in `api.js` will fail unless those requirements are also used in/from the main `app.js`
* the use of modules like `require-all` will very likely cause the bundler not to work
