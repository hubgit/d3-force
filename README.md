# d3-force

See the [component page](http://hubgit.github.io/d3-force) for more information.

## Getting Started


```bash
bower install --save hubgit/d3-force
```

```html
<link rel="import" href="../bower_components/d3-force.html">
```

```html
<d3-force></d3-force>
```

```js
window.addEventListener('polymer-ready', function(e) {
  var graph = document.querySelector('d3-force');

  // when a node is clicked, add more links
  graph.expand = function(source) {
      return new Promise(function(respond, reject) {
          var request = new XMLHttpRequest();
          request.open('GET', source.href + '/related-artists');
          request.responseType = 'json';
          request.onload = function() {
              respond(this.response.artists.map(function(artist) {
                  return ({
                      source: source,
                      target: artist
                  });
              }));
          };
          request.send();
      });
  }

  // start with a seed node
  var seed = {
      id: '6WoTvA9qinpHtSRJuldYh6',
      name: 'The Fall',
      popularity: 52,
      selected: true,
  }

  // expand the seed node
  graph.expand(seed);
});
```
