# ordered-merge-stream

A node stream that merges the output from several streams in the order they were added.

### Example Usage

```JavaScript
  var through = require('through');
  var orderedMergeStream = require('ordered-merge-stream');

  var lets = through();
  var go = through();
  var to = through();
  var space = through();

  var streams = [lets,
                 go,
                 to,
                 space];

  lets.pause();
  go.pause();
  to.pause();
  space.pause();

  var mergedStream = orderedMergeStream(streams);

  var cache = [];

  mergedStream.on('data', function(data){
    cache.push(data);
  });

  space.write('space');
  space.write('go');
  space.write('to');
  space.write('lets');

  mergedStream.on('end', function() {
    console.log(data) // Will output: ["lets", "go", "to", "space"]
  }


```