# CSPEC 3 - Hyperloop modules in Titanium Mobile

This [CSPEC](https://github.com/appcelerator/cspec) proposes a standard for Titanium Mobile modules built with Hyperloop.

## Audience

The primary audience for this proposal are advanced developers using Hyperloop to create cross-platform Titanium Mobile modules.

## Goals

The goals are to come up with a standard way to build and distribute native modules for Titanium Mobile using Hyperloop. It covers a unified project- and file-structure as well as guides to migrate existing modules to the Hyperloop syntax.

## Description

The problem is we currently have some platform specific ways to build modules which ends in different native module projects to maintain. Apart from [gitTio](http://gitt.io) we also have no standard way to distribute these modules.

Using Hyperloop would mean to have a single module and configuration with platform specific folders. It would use NPM for dependency management and distribution.

## Proposal

The proposal would be to standardize the way to build Hyperloop modules so that the enduser will not notice any differences to the existing (native) modules. Therefore we should come up with a template similiar to the existing module template that developers can use to build Hyperloop modules.

### Project structure

The project structure needs to handle different development platforms as common module configuration:

```
/
LICENSE
README
appc.js
package.json
docs/
  index.md
example/
  app.js
src/
  ios/
    platform/
    assets/
    vendor/
  android/
    platform/
    assets/
    vendor/
  windows/
    platform/
    assets/
    vendor/
```

### File structure

The file structure should be unified across platforms. Public methods should be exposed using CommonJS `exports` statements.

### Package.json

For distribution via NPM the module requires a [package.json](https://docs.npmjs.com/files/package.json) file. It would hold information like:

- name (with a required prefix, like `ti-` or `hyperloop-`)
- author
- license
- version (shared by all platforms)
- copyright
- description

### Appc.js

The configuration file should be similar to the `appc.js` specified [here](https://github.com/appcelerator/cspec-appc-appc.js). It would contain information NPM does not need, but our build scripts do, like:

- supported platforms
- minimum Ti.SDK
- native framework dependencies

### Built-in utilities/helper

Known utilities like macros (e.g. `NUMINT(a)` and `ENSURE_TYPE(a,b)`) should be accessible during Hyperloop module development.

### 3rd Party library support 

External libraries are already supported during Hyperloop development. They should be linked in the module's `appc.js` to allow platform specific libraries.

### Publishing

Hyperloop modules should have the ability to be published to the Marketplace (and/or to Gitt.io) automatically. This would require new endpoints to submit a module after development.

### Example

The following is an example of a Hyperloop-enabled appc.js:

```javascript
/**
 * Hyperloop configuration
 */
module.exports = {

    // Basic configuration
    type: 'module',
    group: 'titanium',
    
    minsdk: 6.0.0,

    platforms: {
        ios: {
            vendor: {
                'MyFramework': {
                     // these can be an array or string
                     source: ['src'],
                     header: 'src',
                     resource: 'src'
                }
            }
        },
        android: {
            vendor: {}
        },
        windows: {
            vendor: {}
        }
    }

```

And this is what the `package.json` could look like:

```
{
    "name": "ti-mymodule",
    "description": "Maps Module for Titanium",
    "author": "Appcelerator",
    "copyright": "Copyright (c) 2016 by Appcelerator",
    "license": "Apache-2.0",
    "engines": {
        "titanium": ">=6.0.0"
    }
}
```

## TODO

The following items need to be resolved as part of this CSPEC:

- Create new module template incorporating the proposed project- and file-structure
- Expose internal utility methods and macros (iOS) to be used with Hyperloop
- Make Hyperloop modules run in Titanium Mobile projects like any other (existing) module

## Timeline

The goal of this proposal is to implement in Q1 2016 and Titanium and Alloy if possible in 6.0 (Q1 2016).

## Status

- 01-10-2016 - Added NPM distribution
- 12-29-2015 - Initial Draft

## Legal Stuff

This proposal is non-binding and may not be implemented, may be implemented partially, differently or not at all. Any intellectual property developed as part of this proposal is owned and Copyright &copy; 2016 by Appcelerator, Inc. All Rights Reserved.

For more information about what a CSPEC is, please visit the [Community Specifications & Proposals Repo](https://github.com/appcelerator/cspec).
