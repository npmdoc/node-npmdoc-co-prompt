# api documentation for  co-prompt (v1.0.0)  [![npm package](https://img.shields.io/npm/v/npmdoc-co-prompt.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-co-prompt) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-co-prompt.svg)](https://travis-ci.org/npmdoc/node-npmdoc-co-prompt)
#### simple terminal user input 'co'

[![NPM](https://nodei.co/npm/co-prompt.png?downloads=true)](https://www.npmjs.com/package/co-prompt)

[![apidoc](https://npmdoc.github.io/node-npmdoc-co-prompt/build/screenCapture.buildNpmdoc.browser.%2Fhome%2Ftravis%2Fbuild%2Fnpmdoc%2Fnode-npmdoc-co-prompt%2Ftmp%2Fbuild%2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-co-prompt/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-co-prompt/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-co-prompt/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "dependencies": {
        "keypress": "~0.2.1"
    },
    "description": "simple terminal user input 'co'",
    "devDependencies": {
        "co": "1.4.1",
        "mocha": "~1.10.0",
        "should": "~1.2.2"
    },
    "directories": {},
    "dist": {
        "shasum": "fb370e9edac48576b27a732fe5d7f21d9fc6e6f6",
        "tarball": "https://registry.npmjs.org/co-prompt/-/co-prompt-1.0.0.tgz"
    },
    "keywords": [
        "terminal",
        "console",
        "prompt",
        "confirm",
        "password",
        "mask",
        "co"
    ],
    "license": "MIT",
    "maintainers": [
        {
            "name": "tjholowaychuk",
            "email": "tj@vision-media.ca"
        }
    ],
    "name": "co-prompt",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "version": "1.0.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module co-prompt](#apidoc.module.co-prompt)
1.  [function <span class="apidocSignatureSpan">co-prompt.</span>confirm (msg)](#apidoc.element.co-prompt.confirm)
1.  [function <span class="apidocSignatureSpan">co-prompt.</span>multiline (msg)](#apidoc.element.co-prompt.multiline)
1.  [function <span class="apidocSignatureSpan">co-prompt.</span>password (msg, mask)](#apidoc.element.co-prompt.password)



# <a name="apidoc.module.co-prompt"></a>[module co-prompt](#apidoc.module.co-prompt)

#### <a name="apidoc.element.co-prompt.confirm"></a>[function <span class="apidocSignatureSpan">co-prompt.</span>confirm (msg)](#apidoc.element.co-prompt.confirm)
- description and source-code
```javascript
confirm = function (msg){
  return function(done){
    prompt(msg)(function(err, val){
      if (err) return done(err);
      done(null, bool(val));
    });
  }
}
```
- example usage
```shell
...
Prompt for password input with 'msg' and optional 'mask'
defaulting to "*".

### prompt.multiline(msg)

Prompt for multi-line input with 'msg'.

### prompt.confirm(msg)

Prompt for confirmation with 'msg'.

## License

MIT
...
```

#### <a name="apidoc.element.co-prompt.multiline"></a>[function <span class="apidocSignatureSpan">co-prompt.</span>multiline (msg)](#apidoc.element.co-prompt.multiline)
- description and source-code
```javascript
multiline = function (msg){
  return function(done){
    var buf = [];
    console.log(msg);
    process.stdin.setEncoding('utf8');
    process.stdin.on('data', function(val){
      if ('\n' == val || '\r\n' == val) {
        process.stdin.removeAllListeners('data');
        done(null, buf.join('\n'));
      } else {
        buf.push(val.trimRight());
      }
    }).resume();
  }
}
```
- example usage
```shell
...
Prompt for user input with 'msg'.

### prompt.password(msg, [mask])

Prompt for password input with 'msg' and optional 'mask'
defaulting to "*".

### prompt.multiline(msg)

Prompt for multi-line input with 'msg'.

### prompt.confirm(msg)

Prompt for confirmation with 'msg'.
...
```

#### <a name="apidoc.element.co-prompt.password"></a>[function <span class="apidocSignatureSpan">co-prompt.</span>password (msg, mask)](#apidoc.element.co-prompt.password)
- description and source-code
```javascript
password = function (msg, mask){
  mask = null == mask ? '*' : mask;
  return function(done){
    var buf = '';

    keypress(process.stdin);
    process.stdin.setRawMode(true);
    process.stdout.write(msg);

    process.stdin.on('keypress', function(c, key){
      if (key && 'return' == key.name) {
        console.log();
        process.stdin.pause();
        process.stdin.removeAllListeners('keypress');
        process.stdin.setRawMode(false);
        if (!buf.trim().length) return exports.password(msg, mask)(done);
        done(null, buf);
        return;
      }

      if (key && key.ctrl && 'c' == key.name) {
        console.log('%s', buf);
        process.exit();
      }

      process.stdout.write(mask);
      buf += c;
    }).resume();
  }
}
```
- example usage
```shell
...

## API

### prompt(msg)

Prompt for user input with 'msg'.

### prompt.password(msg, [mask])

Prompt for password input with 'msg' and optional 'mask'
defaulting to "*".

### prompt.multiline(msg)

Prompt for multi-line input with 'msg'.
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
