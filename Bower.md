Bower
=====
### 2014-6-4

Install
-------
#### install node.js
#### npm install -g bower
#### npm update -g bower

Bower Basic
------------
#### bower install jquery
#### bower uninstall jquery
#### bower install jquery#1.11.1
#### bower info jquery
#### bower update (update all package)
#### bower install underscore (if already installed, then update it)
#### bower list (all install package)
#### bower search xxx (http://bower.io/search)

Bower Configuration
-------------------
#### Bower.json
- angualr/bower.json

```JSON
{
  "name": "angular",
  "version": "1.2.16",
  "main": "./angular.js",
  "dependencies": {
  }
}
```
- jquery/bower.json
```JSON
{
  "name": "jquery",
  "version": "2.1.1",
  "main": "dist/jquery.js",
  "license": "MIT",
  "ignore": [
    "**/.*",
    "build",
    "speed",
    "test",
    "*.md",
    "AUTHORS.txt",
    "Gruntfile.js",
    "package.json"
  ],
  "devDependencies": {
    "sizzle": "1.10.19",
    "requirejs": "2.1.10",
    "qunit": "1.14.0",
    "sinon": "1.8.1"
  },
  "keywords": [
    "jquery",
    "javascript",
    "library"
  ]
}
```

- bootstrap/bower.json
```JSON
{
  "name": "bootstrap",
  "version": "3.1.1",
  "main": [
    "./dist/css/bootstrap.css",
    "./dist/js/bootstrap.js",
    "./dist/fonts/glyphicons-halflings-regular.eot",
    "./dist/fonts/glyphicons-halflings-regular.svg",
    "./dist/fonts/glyphicons-halflings-regular.ttf",
    "./dist/fonts/glyphicons-halflings-regular.woff"
  ],
  "ignore": [
    "**/.*",
    "_config.yml",
    "CNAME",
    "composer.json",
    "CONTRIBUTING.md",
    "docs",
    "js/tests"
  ],
  "dependencies": {
    "jquery": ">= 1.9.0"
  }
}
```

#### bower init
```JSON
{
  "name": "bower-basics",
  "version": "1.0",
  "description": "My Bower Project",
  "main": "Index.html",
  "keywords": [
    "bower"
  ],
  "authors": [
    "zhang jinglin"
  ],
  "license": "MIT",
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "test",
    "tests"
  ],
  "dependencies": {
    "angular": "~1.2.16",
    "bootstrap": "~3.1.1",
    "jasmine": "~2.0.0"
  }
}
```
#### .bowerrc
```JSON
{
  "directory": "js/lib"
}
```
- all package will install to js/lib

#### bower install (by bower.json, install package)
#### bower install bootstrap --save (add item to bower.json "dependencies")
#### bower install jasmine --save-dev (add item to bower.json "devDependencies")
#### bower uninstall bootstrap --save
#### bower uninstall jasmine --save-dev

Advanced Topic
---------------
#### bower cache list
#### bower cache clean
#### bower install jquery -o (offline mode)
#### bower install http://code.jquery.com/jquery-1.11.1.js
#### bower install --production (only install "dependencies" package)
#### bower install lo_=lodash 
#### bower help
#### bower help install
#### bower info jquery (top: the boer.json about of jquery)
#### bower info jquery#1.11.0
#### bower lookup angular (git://github.com/angular/**bower-angular**.git)
#### bower prune (delete package not in "dependencies" or "devDependencies")

#### an example
- create .bowerrc
```JSON
{
  "directory": "js/lib"
}
```
- create bower.json
```JSON
{
  "name": "My Project"
}
```
- bower install jquery lodash bootstrap angular angular-resource angular-route --save (it create js/lib directory)

- create /test/lib
```JSON
{
  "directory": "lib"
}
```

- create /test/.bowerrc /test/bower.json
```JSON
{
  "name": "My Project Test"
}
```

- cd test
- bower install jasmine --save (it will install package in test/lib)