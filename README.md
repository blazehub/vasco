# node-vasco

Service Discoverer node module

## Slides from the talk at DeveloperWeek 2018

[Vasco talk](https://github.com/asyncanup/vasco/blob/33b878b3ae43f2c75197ea3e93afa0876ce990de/vasco-talk.pdf)

Includes an introduction to Service Discovery, Vasco Architecture, and Code Walk-through.

## Install

```bash
npm install vasco
```

## Usage

Inside `package.json` of a new service module:

```js
    "name": "user-api",                 // name of service
    "version": "v1.0.1",                // version of service
    "vasco": {                          // vasco configuration
        "dependencies": {               // dependencies of service
            "accounts-api": "v3.0.0",   // name and version of dependency
            "auth": "v1.0.0"
        },
        "mocks": {
            "auth": "localhost:3000"    // dependency mocked with given url
        }
    }
```

Now, inside `app.js` (or another entrypoint):

```js
var vasco = require('vasco');
vasco.register(selfUrl, require('./package'), (err, deps) => {
  if (err) {
    // some error happened in registering self or getting dependency urls
    throw err;
  }
  // deps is a hash of dependency name:version mapped against their urls
  console.log(deps);
});
```

where `selfUrl` is the URL (typically `<ip>:<port>`) that you want this
service to be registered against.

Then, start the service command prompt:

```bash
VASCO_URL=127.0.0.1:6379 node app.js
```

If not supplied, `VASCO_URL` gets the default value `127.0.0.1:6379`,
ideal for development.
