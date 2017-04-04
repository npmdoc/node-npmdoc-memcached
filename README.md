# api documentation for  [memcached (v2.2.2)](https://github.com/3rd-Eden/node-memcached#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-memcached.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-memcached) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-memcached.svg)](https://travis-ci.org/npmdoc/node-npmdoc-memcached)
#### A fully featured Memcached API client, supporting both single and clustered Memcached servers through consistent hashing and failover/failure. Memcached is rewrite of nMemcached, which will be deprecated in the near future.

[![NPM](https://nodei.co/npm/memcached.png?downloads=true)](https://www.npmjs.com/package/memcached)

[![apidoc](https://npmdoc.github.io/node-npmdoc-memcached/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-memcached_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-memcached/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-memcached/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-memcached/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Arnout Kazemier"
    },
    "bugs": {
        "url": "https://github.com/3rd-Eden/node-memcached/issues"
    },
    "dependencies": {
        "hashring": "3.2.x",
        "jackpot": ">=0.0.6"
    },
    "description": "A fully featured Memcached API client, supporting both single and clustered Memcached servers through consistent hashing and failover/failure. Memcached is rewrite of nMemcached, which will be deprecated in the near future.",
    "devDependencies": {
        "mocha": "*",
        "pre-commit": "*",
        "should": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "68f86ccfd84bcf93cc25ed46d6d7fc0c7521c9d5",
        "tarball": "https://registry.npmjs.org/memcached/-/memcached-2.2.2.tgz"
    },
    "gitHead": "83a6752842665cf14b7cccf37de85b02fc07f5d5",
    "homepage": "https://github.com/3rd-Eden/node-memcached#readme",
    "keywords": [
        "InnoDB memcached API",
        "cache",
        "client",
        "cluster",
        "failover",
        "hashing",
        "membase",
        "memcache",
        "memcached",
        "nMemcached",
        "nosql"
    ],
    "license": "MIT",
    "main": "index",
    "maintainers": [
        {
            "name": "ronkorving",
            "email": "ron@ronkorving.nl"
        },
        {
            "name": "v1",
            "email": "info@3rd-Eden.com"
        },
        {
            "name": "3rdeden",
            "email": "npm@3rd-Eden.com"
        }
    ],
    "name": "memcached",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+ssh://git@github.com/3rd-Eden/node-memcached.git"
    },
    "scripts": {
        "test": "mocha $(find test -name '*.test.js')"
    },
    "version": "2.2.2"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module memcached](#apidoc.module.memcached)
1.  object <span class="apidocSignatureSpan">memcached.</span>config
1.  object <span class="apidocSignatureSpan">memcached.</span>connection
1.  object <span class="apidocSignatureSpan">memcached.</span>utils

#### [module memcached.connection](#apidoc.module.memcached.connection)
1.  [function <span class="apidocSignatureSpan">memcached.connection.</span>Available (host, callback)](#apidoc.element.memcached.connection.Available)
1.  [function <span class="apidocSignatureSpan">memcached.connection.</span>IssueLog (args)](#apidoc.element.memcached.connection.IssueLog)

#### [module memcached.utils](#apidoc.module.memcached.utils)
1.  [function <span class="apidocSignatureSpan">memcached.utils.</span>Iterator (collection, callback)](#apidoc.element.memcached.utils.Iterator)
1.  [function <span class="apidocSignatureSpan">memcached.utils.</span>curry (context, fn)](#apidoc.element.memcached.utils.curry)
1.  [function <span class="apidocSignatureSpan">memcached.utils.</span>escapeValue (value)](#apidoc.element.memcached.utils.escapeValue)
1.  [function <span class="apidocSignatureSpan">memcached.utils.</span>fuse (target, handlers)](#apidoc.element.memcached.utils.fuse)
1.  [function <span class="apidocSignatureSpan">memcached.utils.</span>merge (target, obj)](#apidoc.element.memcached.utils.merge)
1.  [function <span class="apidocSignatureSpan">memcached.utils.</span>reallocString (value)](#apidoc.element.memcached.utils.reallocString)
1.  [function <span class="apidocSignatureSpan">memcached.utils.</span>unescapeValue (value)](#apidoc.element.memcached.utils.unescapeValue)
1.  [function <span class="apidocSignatureSpan">memcached.utils.</span>validateArg (args, config)](#apidoc.element.memcached.utils.validateArg)



# <a name="apidoc.module.memcached"></a>[module memcached](#apidoc.module.memcached)



# <a name="apidoc.module.memcached.connection"></a>[module memcached.connection](#apidoc.module.memcached.connection)

#### <a name="apidoc.element.memcached.connection.Available"></a>[function <span class="apidocSignatureSpan">memcached.connection.</span>Available (host, callback)](#apidoc.element.memcached.connection.Available)
- description and source-code
```javascript
function ping(host, callback) {
  var isWin = process.platform.indexOf('win') === 0; // win32 or win64
  var arg = isWin ? '-n' : '-c';
  var pong = spawn('ping', [arg, '3', host]); // only ping 3 times

  pong.stdout.on('data', function stdoutdata (data) {
    callback(false, data.toString().split('\n')[0].substr(14));
    pong.kill();
  });

  pong.stderr.on('data', function stderrdata (data) {
    callback(new Error(data.toString().split('\n')[0].substr(14)), false);
    pong.kill();
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.memcached.connection.IssueLog"></a>[function <span class="apidocSignatureSpan">memcached.connection.</span>IssueLog (args)](#apidoc.element.memcached.connection.IssueLog)
- description and source-code
```javascript
function IssueLog(args) {
  this.config = args;
  this.messages = [];
  this.failed = false;
  this.locked = false;
  this.isScheduledToReconnect = false;

  this.totalFailures = 0;
  this.retry = 0;
  this.totalReconnectsAttempted = 0;
  this.totalReconnectsSuccess = 0;

  Utils.merge(this, args);
  EventEmitter.call(this);
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.memcached.utils"></a>[module memcached.utils](#apidoc.module.memcached.utils)

#### <a name="apidoc.element.memcached.utils.Iterator"></a>[function <span class="apidocSignatureSpan">memcached.utils.</span>Iterator (collection, callback)](#apidoc.element.memcached.utils.Iterator)
- description and source-code
```javascript
function iterator(collection, callback) {
  var arr = Array.isArray(collection)
    , keys = !arr ? Object.keys(collection) : false
    , index = 0
    , maximum = arr ? collection.length : keys.length
    , self = this;

  // returns next item
  this.next = function next () {
    var obj = arr ? collection[index] : { key: keys[index], value: collection[keys[index]] };
    callback(obj, index++, collection, self);
  };

  // check if we have more items
  this.hasNext = function hasNext () {
    return index < maximum;
  };
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.memcached.utils.curry"></a>[function <span class="apidocSignatureSpan">memcached.utils.</span>curry (context, fn)](#apidoc.element.memcached.utils.curry)
- description and source-code
```javascript
function curry(context, fn) {
  var copy = Array.prototype.slice
    , args = copy.call(arguments, 2);

  return function bowlofcurry () {
    return fn.apply(context || this, args.concat(copy.call(arguments)));
  };
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.memcached.utils.escapeValue"></a>[function <span class="apidocSignatureSpan">memcached.utils.</span>escapeValue (value)](#apidoc.element.memcached.utils.escapeValue)
- description and source-code
```javascript
escapeValue = function (value) {
  return value.replace(/(\r|\n)/g, '\\$1');
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.memcached.utils.fuse"></a>[function <span class="apidocSignatureSpan">memcached.utils.</span>fuse (target, handlers)](#apidoc.element.memcached.utils.fuse)
- description and source-code
```javascript
function fuse(target, handlers) {
  for (var i in handlers)
    if (handlers.hasOwnProperty(i)){
      target.on(i, handlers[i]);
    }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.memcached.utils.merge"></a>[function <span class="apidocSignatureSpan">memcached.utils.</span>merge (target, obj)](#apidoc.element.memcached.utils.merge)
- description and source-code
```javascript
function merge(target, obj) {
  for (var i in obj) {
    target[i] = obj[i];
  }

  return target;
}
```
- example usage
```shell
...
  this.isScheduledToReconnect = false;

  this.totalFailures = 0;
  this.retry = 0;
  this.totalReconnectsAttempted = 0;
  this.totalReconnectsSuccess = 0;

  Utils.merge(this, args);
  EventEmitter.call(this);
}

util.inherits(IssueLog, EventEmitter);
var issues = IssueLog.prototype;

issues.log = function log (message) {
...
```

#### <a name="apidoc.element.memcached.utils.reallocString"></a>[function <span class="apidocSignatureSpan">memcached.utils.</span>reallocString (value)](#apidoc.element.memcached.utils.reallocString)
- description and source-code
```javascript
reallocString = function (value) {
  // Reallocate string to fix slow string operations in node 0.10
  // see https://code.google.com/p/v8/issues/detail?id=2869 for details
  return (' ' + value).substr(1);
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.memcached.utils.unescapeValue"></a>[function <span class="apidocSignatureSpan">memcached.utils.</span>unescapeValue (value)](#apidoc.element.memcached.utils.unescapeValue)
- description and source-code
```javascript
unescapeValue = function (value) {
  return value.replace(/\\(\r|\n)/g, '$1');
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.memcached.utils.validateArg"></a>[function <span class="apidocSignatureSpan">memcached.utils.</span>validateArg (args, config)](#apidoc.element.memcached.utils.validateArg)
- description and source-code
```javascript
function validateArg(args, config) {
  var err;

  args.validate.forEach(function (tokens) {
    var key = tokens[0]
      , value = args[key];

    switch(tokens[1]){
      case Number:
        if (toString.call(value) !== '[object Number]') {
          err = 'Argument "' + key + '" is not a valid Number.';
        }

        break;

      case Boolean:
        if (toString.call(value) !== '[object Boolean]') {
          err = 'Argument "' + key + '" is not a valid Boolean.';
        }

        break;

      case Array:
        if (toString.call(value) !== '[object Array]') {
          err = 'Argument "' + key + '" is not a valid Array.';
        }
        if (!err && key === 'key') {
          for (var vKey=0; vKey<value.length; vKey++) {
            var vValue = value[vKey];
            var result = validateKeySize(config, vKey, vValue);
            if (result.err) {
              err = result.err;
            } else {
              args.command = args.command.replace(vValue, result['value']);
            }
          }
        }
        break;

      case Object:
        if (toString.call(value) !== '[object Object]') {
          err = 'Argument "' + key + '" is not a valid Object.';
        }

        break;

      case Function:
        if (toString.call(value) !== '[object Function]') {
          err = 'Argument "' + key + '" is not a valid Function.';
        }

        break;

      case String:
        if (toString.call(value) !== '[object String]') {
          err = 'Argument "' + key + '" is not a valid String.';
        }

        if (!err && key === 'key') {
          var result = validateKeySize(config, key, value);
          if (result.err) {
            err = result.err;
          } else {
            args.command = reallocString(args.command).replace(value, result['value']);
          }
        }
        break;

      default:
        if (toString.call(value) === '[object global]' && !tokens[2]) {
          err = 'Argument "' + key + '" is not defined.';
        }
    }
  });

  if (err){
    if (args.callback) args.callback(new Error(err));
    return false;
  }

  return true;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
