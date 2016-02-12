webtoolz : Simplifying the web toolchain
========================================

The JavaScript ecosystem is complex and continually evolving. This is an attempt to simplify it. Goals of this project:

* Provide an out-of-the-box toolchain for JS/CSS projects
* Highly opinionated sensible defaults
* Command line tool (plays well with other tools, including shell scripts, make, npm run, etc)
* No config files
* 'Batteries included' toolchain: ES2015 (and more) transpiling, enforce coding standards, ES import module packaging, shrinking, testing, CSS pre-processing
* Designed for browser apps - you don't need to know NodeJS

webtoolz takes two arguments:
1. An entry point .js for your app in a source tree consisting of ES2015 code, modules, CSS, etc.
2. An output .js file that can be included in your final web page with a single `<script src=...>` element. Transpiled,
   resolved, validated, crunched, everything.

webtoolz was born out of frustration with how complicated it is to get JavaScript tooling setup for real world projects.

Usage
-----

Install globally:

```bash
$ npm install -g webtoolz
```

Alternatively, install just for your current project:

```bash
$ npm install --save-dev webtoolz
# Note: You will need to call $(npm bin)/webtoolz to run it
```

Run:

```bash
# Compile once
$ webtoolz webapp src/myapp.js out/myapp.js

# Compile continuously whenever a file changes
$ webtoolz webapp --watch src/myapp.js out/myapps.js
```

Usage:

```
webtoolz [webapp|weblib] [... options] ENTRYPOINT OUTPUT

webapp              Build for use in a browser. Result can be included in <script src="...">.

weblib                 Build into self-contained library, that can be imported into other
                    projects as a regular ES6 module.

ENTRYPOINT          Entry point to your app (.js). Any dependencies (including CSS)
                    will be discovered by walking the imports.

OUTPUT              Output JS file. For a webapp, this will also include auto-injected CSS. Use '-'
                    for stdout.



Common options:

-h --help           This.

-w --watch          Keep running and automatically rebuild when any dependency changes.

-t --test FILE      Source file or directory to run unit tests. If directory, will be
                    recursively searched for .js files. Glob patterns will be expanded.
                    This option can be specified multiple times to search multiple dirs.

--[semi|no]standard Fail if code does not meet coding standards. See http://standardjs.com
                    'standard' enforces no semicolons, 'semistandard' enforces semicolons,
                    'nostandard' bypasses checks. Default is --standard.



Advanced options:

-v --verbose        Show what's going on under the hood.

--[no]minify        Whether to minify results or not. Default is to write both minified
                    and unminified with source maps.

--[no]pretty        Whether to use human friendly pretty output (colors, progress bars, ascii art,
                    etc). Defaults to pretty is used from a TTY (e.g. terminal) or nopretty if
                    no terminal (e.g. unattended script, build server...).

-e --exclude PATT   Patterns to exclude in DIR (may contain globs).

-i --include PATT   Override --exclude.

--babelpreset PRES  Babel preset to use for transpiling JS. See https://babeljs.io/docs/plugins/.
                    Default is 'es2015', unless .jsx files are present, in which case
                    it's 'react'.

--babelplugin PLUG  Additional Babel plugins to use. See https://babeljs.io/docs/plugins/.
                    Default is none (relies on babelpreset). This option can be specified
                    multiple times to include multiple plugins.


```

Under the hood
--------------

* enforce JS coding standards with [StandardJS](http://standardjs.com/)
* transpile JS with [Babel](https://babeljs.io)
* bundle ES6 modules and shrink JS with [Rollup](http://rollupjs.org)
* run JS unit tests with [Mocha](https://mochajs.org/)
* minify JS with [Uglify](http://lisperator.net/uglifyjs/)
* transpile CSS with [postcss](https://github.com/postcss/postcss)
* minify CSS with [cssnano](http://cssnano.co/)

