wordpos-web
=======

[![NPM version](https://img.shields.io/npm/v/wordpos-web.svg)](https://www.npmjs.com/package/wordpos-web)
[![Build Status](https://img.shields.io/travis/moos/wordpos-web/master.svg)](https://travis-ci.org/moos/wordpos-web)

[wordpos](moos/wordpos) is a set of *fast* part-of-speech (POS) utilities for Node.js **and** browser using fast lookup in the WordNet database.

**wordpos-web** formats the WordNet DB files to allow running wordpos in the browser.


## Installation

     npm install wordpos-web


## wordpos Docs
See [wordpos/README](https://github.com/moos/wordpos).

## Running inside the browsers

v2.0 of [wordpos](https://github.com/moos/wordpos) introduces the capability of running in the browser.  The dictionary files are optimized for fast access (lookup by lemma), but they must be fetched, parsed and loaded into browser memory.  The files are loaded on-demand (unless the option `preload: true` is given).

The dict files can be served locally or from CDN (see [samples/cdn](samples/cdn/) for code, or [see it in action](https://moos.github.io/wordpos/cdn)).  Include the following scripts in your `index.html`:
```html
<script src="wordpos/dist/wordpos.min.js"></script>
<script>
  let wordpos = new WordPOS({
    // preload: true,
    dictPath: '/wordpos/dict',
    profile: true
  });

  wordpos.getAdverbs('this is lately a likely tricky business this is')
    .then(res => {
      console.log(res); // ["lately", "likely"]
    });
</script>
```
Above assumes wordpos is installed to the directory `./wordpos`.  `./wordpos/dict` holds the index and data WordNet files generated for the web in a postinstall script.

See [samples/self-hosted](samples/self-hosted/).  To run samples, `npm i -g http-server`, then:

```bash
$ npm run build
$ npm run start
Starting up http-server, serving ./
Available on:
  http://localhost:8080
```
and open your browser to that url.


## Changes
- 1.0.0 - Initial web version


License
-------

https://github.com/moos/wordpos-web
Copyright (c) 2012-2020 mooster@42at.com
(The MIT License)
