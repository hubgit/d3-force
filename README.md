# d3-force

The most important things to know are:

1. Every object must have an `id` (used to identify the object) and a `name` (for displaying the object).
2. Each link must have a `source` and a `target` object.
3. Links can be added to the graph by passing an array of links to `addLinks`, or by overriding the `expand` function and returning a Promise.
4. Nodes are added to the graph automatically.

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
  // every node must have an id and a name
  graph.click({
      id: '6WoTvA9qinpHtSRJuldYh6',
      name: 'The Fall',
      href: 'https://api.spotify.com/v1/artists/6WoTvA9qinpHtSRJuldYh6',
      popularity: 52
  });
});
```
