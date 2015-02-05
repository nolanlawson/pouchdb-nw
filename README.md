pouchdb-nw
==========

PouchDB works great in NW.js (formerly known as Node-Webkit). Here's how to get started.

Sample app
----

* [PouchDB NW.js "Hello world" app](https://github.com/nolanlawson/node-webkit-pouchdb-demo) (using all three adapters!)

Installation
-----

There are two ways to use PouchDB in NW.js: as a browser script or as a Node.js module.

By far the easiest way is to use it as a browser script. NW.js is based on Chromium, so PouchDB will work out of the box, since both IndexedDB and WebSQL are supported by Chromium.

In Node.js, PouchDB uses LevelDB via [leveldown](https://www.npmjs.com/package/leveldown), which has to be separately compiled for NW.js. Many people have reported problems getting it to build, especially on Windows. Use at your own risk.

### PouchDB as a browser script

Just download [pouchdb.js](http://pouchdb.com/guides/setup-pouchdb.html) and include it in your `index.html`. That's it.

```html
<script src="path/to/pouchdb.js"></script>
```

Now `PouchDB` is available as a global variable. So you can create an IndexedDB-based PouchDB:

```js
var db = new PouchDB('mydb');
```

or a WebSQL-based PouchDB:

```js
var db = new PouchDB('mydb', {adapter: 'websql'});
```

Use whichever one you prefer. They both work the same, although in my experience WebSQL is slightly faster than IndexedDB in Chromium, for most use cases. (NW is based on Chromium.)

### PouchDB as a Node.js module

You will need to recompile leveldown using [nw-gyp](https://www.npmjs.com/package/nw-gyp). The [sample app](https://github.com/nolanlawson/node-webkit-pouchdb-demo) shows you exactly how to do this.

Basically you just need to run `nw-gyp` in the `node_modules/leveldown` directory, including the version of NW you are using:

```bash
nw-gyp configure --target=0.8.6 # or whatever version of NW you're using
nw-gyp build
```

In the sample app, this is accomplished with a `postinstall` script (see `package.json`) because that way it's done automatically when you `npm install`.

After that, to use the LevelDB-based PouchDB, just `require` it:

```js
var LevelPouchDB = require('pouchdb');
var db = new LevelPouchDB('mydb');
```

Because the LevelDB-based PouchDB is `require`d whereas the others are attached to `window.PouchDB`, you can actually use all three adapters simultaneously! Which is kinda neat.
