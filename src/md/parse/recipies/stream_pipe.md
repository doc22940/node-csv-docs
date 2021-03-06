---
title: Stream pipe
description: CSV Parse - stream, callback and sync APIs
keywords: ['csv', 'parse', 'parser', 'recipe', 'stream', 'sync', 'pipe', 'read', 'write']
sort: 5
---

# Using pipe to connect multiple streams

Part of the [Stream API](https://nodejs.org/api/stream.html), the [`pipe` function](https://nodejs.org/api/stream.html#stream_readable_pipe_destination_options) is a precious tool used to wire multiple streams. The function is meant to connect a `stream.Readable` source to a `stream.Writable` destination.

The [pipe example](https://github.com/adaltas/node-csv-parse/blob/master/samples/recipe.pipe.js) reads a file, parses its content, transforms it and print the result to the standard output.

This example is available with the command `node samples/recipe.pipe.js`.

```js
const parse = require('csv-parse')
const generate = require('csv-generate')
const transform = require('stream-transform')

const generator = generate({
  length: 20
})
const parser = parse({
  delimiter: ':'
})
const transformer = transform(function(record, callback){
  setTimeout(function(){
    callback(null, record.join(' ')+'\n')
  }, 500)
}, {
  parallel: 5
})
generator.pipe(parser).pipe(transformer).pipe(process.stdout)
```
