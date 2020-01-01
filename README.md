wordpos-web
=======

[![NPM version](https://img.shields.io/npm/v/wordpos-web.svg)](https://www.npmjs.com/package/wordpos-web)

**CDNs:** [![](https://data.jsdelivr.com/v1/package/npm/wordpos-web/badge)](https://www.jsdelivr.com/package/npm/wordpos-web)


[wordpos](https://github.com/moos/wordpos) is a set of *fast* part-of-speech (POS) utilities for Node.js **and** browser using fast lookup in the WordNet database.

**wordpos-web** formats the WordNet DB files to allow running *wordpos* in the browser.

📣 [Demo](https://moos.github.io/wordpos-web/docs)


## Installation

     npm install wordpos-web


## wordpos API Docs
See [wordpos/README](https://github.com/moos/wordpos).

## Running inside the browsers

v2.0 of [wordpos](https://github.com/moos/wordpos) introduces the capability of running in the browser.  The dictionary files are optimized for fast access (lookup by lemma), but they must be fetched, parsed and loaded into browser memory.  The files are loaded on-demand (unless the option `preload: true` is given).

The dict files can be served locally or from CDN (see [samples/cdn](docs/cdn/)).  Include the following scripts in your `index.html`:
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

See [samples/self-hosted](docs/self-hosted/).  

### Using `preload` option
When running inside the browse, you can request that the necessary index & data files be preloaded, rather than loaded on demand.

Use this when you're **sure the files will be used** and want to minimize the on-demand wait time.

```js
let wordpos = new WordPOS({
  preload: 'a',       // Preload adjectives index file.
  includeData: true,  // Also preload the adjectives data file.
  dictPath: '/wordpos/dict'
});

wordpos.ready().then(()=> {
  // files are loaded
});
```
See [wordpos#options](https://github.com/moos/wordpos#options) for other `preload` options.

## A note on file sizes
The original WordNet DB is around 35 MB.  The size of the "index" and "data" files formatted for the web are as follows (before any compression):


| POS | data | index |
| --- | --- | --- |
| adverbs | 525 KB | 170 KB |
| verbs | 2.7 MB | 534 KB |
| adjectives | 3.1 MB | 851 KB |
| nouns | 15 MB<sup>🚩</sup> | 4.8 MB |
| All (raw) | 21.3 MB | 6.4 MB |
| All (gzip) | ~6 MB | 1.7 MB |


<sup>🚩</sup> Be aware this is a very large file and may take some time to fetch.

Resources fetched over CDN will be compressed.  For example, the compressed noun files are 4.1 MB (data) and 1.3 MB (index).  If you are self-hosting, ensure gzip compression is on for your server.

Some wordpos API need just the "index" file, while others need both.  Here's the breakdown:

| API | data | index |
| --- | --- | --- |
| `get?()` | ✖️ | ✔️ |
| `is?()` | ✖️ | ✔️ |
| `lookup?()` | ✔️ | ✔️ |
| `seek()` | ✔️ | ✔️ |
| `rand?()` | ✖️ | ✔️ |

The following API will access **all POS** files:
- `getPOS()` -- all index files
- `lookup()` -- all index files & data files for matched word
- `rand()` -- all index files


## Dev
To run samples, `npm i -g http-server`, then:

```bash
$ npm run build
$ npm run start
Starting up http-server, serving ./
Available on:
  http://localhost:8080
```
and open your browser to that url.


## Changes
- 1.0.2 - Build samples/ to docs/.
- 1.0.1 - Fix self-hosted sample dict/ path
- 1.0.0 - Initial web version


License
-------

https://github.com/moos/wordpos-web
Copyright (c) 2012-2020 mooster@42at.com
(The MIT License)
