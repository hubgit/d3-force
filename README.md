# d3-force

[Demo](http://git.macropus.org/d3-force/components/d3-force/demo.html)

## Notes

* The layout uses DOM nodes rather than SVG. This makes the node templates easier, but means that only the nodes are drawn, and not the links between them.
* The nodes are positioned using CSS 3D transforms, so are composited using the GPU. This means that rendering the nodes takes a minimal amount of time - nearly all of the speed decrease as the graph gets larger is due to the graph layout calculation.
* By over-riding the `graph.node` function, Polymer elements can be used as nodes: see [this demo](http://git.macropus.org/d3-force/components/d3-force/demo-polymer.html).

## Nodes

1. Every object must have an `id` (used to identify the object) and a `name` (for displaying the object).
2. Each link must have a `source` and a `target` object.
3. Links can be added to the graph by passing an array of links to `addLinks`, or by overriding the `expand` function and returning a Promise.
4. Nodes are added to the graph automatically.

## Getting started

```bash
bower install --save hubgit/d3-force
```

```html
<link rel="import" href="bower_components/d3-force/d3-force.html">

<d3-force></d3-force>

<script>
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
</script>
```
