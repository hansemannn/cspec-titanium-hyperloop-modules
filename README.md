# CSPEC 3 - Hyperloop modules in Titanium Mobile

This [CSPEC](https://github.com/appcelerator/cspec) is for standardizing around Titanium MObile modules built with Hyperloop.

## Audience

The primary audience for this proposal are advanced developers using Hyperloop to create cross-platform Titanium Mobile modules.

## Goals

The goals are to come up with a standardizing way to build native modules for Titanium Mobile using Hyperloop. That covers a unified project- and file-structure as well as guides to migrate existing modules to the Hyperloop syntax.

## Description

The problem is we currently have some platform specific ways to build modules which ends in different native module projects to maintain. Using Hyperloop would mean to have a platform specific structure to develop all platforms together, as well as a unified configuration file to specify author, license, version, guid etc.

## Proposal

The proposal would be to standardize the way to build Hyperloop modules so that the enduser will not notice any differences to the existing (native) modules. Therefore we should come up with a template similiar to the existing module template that developers can use to build Hyperloop modules.

### Project structure

The project structure needs to handle different development platforms as common module configuration:

```
/
LICENSE
README
appc.js
example/
  app.js
docs/
  index.md
src/
  ios/
    platform/
    assets/
    thirdparty/
  android/
    platform/
    assets/
    thirdparty/
  windows/
    platform/
    assets/
    thirdparty/
```

### File structure

The file structure should be unified across platforms. Public methods should be exposed using CommonJS `exports` statements.

### Configuration file

The configuration file should be similar to the `appc.js` specified [here](https://github.com/appcelerator/cspec-appc-appc.js). Common properties would also be:
- module name
- module author
- module license
- module version (per platform)
- module guid
- supported platforms
- copyright
- description
- minimum Ti.SDK

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
    author: 'Appcelerator',
    copyright: 'Copyright (c) 2016 by Appcelerator',
    license: 'Apache'
    
    // These should not be edited
    name: 'TiGoogleMaps',
    moduleid: 'ti.googlemaps',
    guid: '11111111-1111-1111-1111-111111111111',
    minsdk: 6.0.0,

    platforms: {
        ios: {
            version: 1.0.2,
            thirdparty: {
                'MyFramework': {
                     // these can be an array or string
                     source: ['src'],
                     header: 'src',
                     resource: 'src'
                }
            }
        },
        android: {
            version: 1.0.1,
            thirdparty: {}
        },
        windows: {
            version: 1.0.0,
            thirdparty: {}
        }
    }

```

## TODO

The following items need to be resolved as part of this CSPEC:

- Create new module template incorporating the proposed project- and file-structure
- Expose internal utility methods and macros (iOS) to be used with Hyperloop
- Implement new endpoints to submit a Hyerloop module
- Make Hyperloop modules run in Titanium Mobile projects like any other (existing) module

## Timeline

The goal of this proposal is to implement in Q1 2016 and Titanium and Alloy if possible in 6.0 (Q1 2016).

## Status

12-29-2015 - Initial Draft

## Legal Stuff

This proposal is non-binding and may not be implemented, may be implemented partially, differently or not at all. Any intellectual property developed as part of this proposal is owned and Copyright &copy; 2015 by Appcelerator, Inc. All Rights Reserved.

For more information about what a CSPEC is, please visit the [Community Specifications & Proposals Repo](https://github.com/appcelerator/cspec).
