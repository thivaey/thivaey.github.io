---
layout: post
title: "D3 Graph Visualization in GitHub Pages"
author: "R. Tyler McLaughlin"
date: "April 29th, 2018"
categories: blog
---
<script src="//code.jquery.com/jquery.js"></script>
<style>

.node {
  stroke: #fff;
  stroke-width: 1.5px;
}

.link {
  stroke: #999;
  stroke-opacity: .6;
}

</style>

This is a starter guide for visualizing your own graph/network data in **D3** and loading it in **GitHub Pages**.

The Javascript library [D3](https://d3js.org/) is known for making extremely nice looking, interactive data visualization apps that run in your web browser.  Some impressive examples include Mike Barry and Brian Card's visualizations of the [Boston subway system data](http://mbtaviz.github.io) and [an interactive neural network](https://playground.tensorflow.org) courtesy of Tensorflow.

Even though D3 has a [serious learning curve](https://medium.com/@enjalot/the-hitchhikers-guide-to-d3-js-a8552174733a), it is possible to make use of its nice interactive visualizations **without really knowing anything about JavaScript**.  As it turns out, it is really not that hard to get your own data in an interactive D3 graph!
This post will walk you through how to visualize a graph aka network in D3 with your own data.


For this post and for my website, I wanted to learn to visualize my own network data using D3.  I followed a lot of steps by other bloggers like [Andrew Mehrmann](http://dkmehrmann.github.io/blog/2016/05/01/d3.html) and [Eric Bickel](https://ehbick01.github.io/2017/05/09/embedding-d3-visuals-in-rmarkdown/) to get this to work.    Here's what the end result looks like: 

<div id='d3div'></div>

You are viewing a network of jazz scales.  The nodes represent different musical scales.  The edges connect any two scales that share 6 common tones.  The nodes are colored by scale type: blue nodes are Major scales.  This is a generalization of the [Circle of Fifths](https://en.wikipedia.org/wiki/Circle_of_fifths)  You can click and drag the nodes around and if you hover over the nodes, the name of the scale appears.

## The pieces behind making this work

First, take a look at the code from [this visualization](http://bl.ocks.org/mbostock/4062045)  in the [D3 gallery](https://github.com/mbostock/d3/wiki/Gallery).  

Notice that there is also a `.json` file that contains characters from *Les Miserables*.  **JSON** is the standard format for input and output to D3.js.  So you will definitely need to have your graph data into the JSON format.   

## JSON format for network data

If you already have your data in the JSON format, you can skip ahead.  My graph data happened to be in a custom `.csv` format, so I ended up writing a python script to convert the `.csv` to a `.json` file.  
Know that there are helpful JSON packages in **Python** and **R** that might work with standard formats.  Currently [igraph in R](http://kateto.net/networks-r-igraph) does not have an export JSON function. but [Cytoscape](http://cytoscape.org) and [NetworkX](https://networkx.github.io), a popular network library in Python, do support JSON output.  You may have to do something similar to what I did and write your own whatever-to-JSON conversion script, in which case, I'll walk you through the format.  

This is what a minimal non-hierarchical graph in JSON format looks like:

```javascript
{
  "nodes": [
    {"id": "Myriel", "group": 1},
    {"id": "Napoleon", "group": 1},
    {"id": "Mlle.Baptistine", "group": 2},
    {"id": "Mme.Magloire", "group": 3}
    ],
  "links": [
    {"source": "Napoleon", "target": "Myriel", "value": 1},
    {"source": "Mlle.Baptistine", "target": "Myriel", "value": 8},
    {"source": "Mme.Magloire", "target": "Myriel", "value": 10},
    {"source": "Mme.Magloire", "target": "Mlle.Baptistine", "value": 6}
    ]
 }
```
This JSON file contains all the information about the graph's links and nodes.   You can Specify what the color the nodes should be via the `"group"` argument.  In the `"links"` element, which refers to the edges in the graph, it specifies how the nodes are connected (as directed *source/target* edges) Lastly, the `"value"` argument will determine the thickness of the edge.  Very simple!

Once your data is in the format like this, you will also need to create a folder called `/scripts/` in the root directory of your github.io repository and move your `.json` file there.

## JavaScript 

Now go to [Andrew Mehrmann's blog post](http://dkmehrmann.github.io/blog/2016/05/01/d3.html) and copy the JavaScript (which was derived from [Mike Bostock's original force-directed graph visualization](http://bl.ocks.org/mbostock/4062045) and modified for size autoscaling) and paste it directly into your markdown file.  Make sure you also copy the `style tag`.  Thanks, Andrew and Mike!  

It doesn't really matter where you put the javascript or the style tag.  
The location of the graph will be wherever you put the following `div tag` into your markdown file:

```html
<div id='d3div'></div>
````

## Importing your data

In the argument of the `d3.json()` function, replace `miserables.json` with the name of your `.json` file but prepend the file name with 4 '../' (which the parent directory symbol in Unix) so that the line looks like:

```javascript
d3.json("../../../../scripts/YOUR_FILE_NAME.json", function(error, graph) {
```

I don't know why you need so many of the parent-directory symbols but I couldn't get it to work with any fewer.





<script src="//d3js.org/d3.v3.min.js"></script>
<script>

var width = $("#d3div").width(),
    height = 400;

var color = d3.scale.category20();

var force = d3.layout.force()
    .charge(-62)
    .linkDistance(80)
    .size([width, height]);

var svg = d3.select("#d3div").append("svg")
    .attr("width", width)
    .attr("height", height);

d3.json("../../../../scripts/jazz_scales_network_minCTs6.json", function(error, graph) {
  if (error) throw error;

  force
      .nodes(graph.nodes)
      .links(graph.links)
      .start();

  var link = svg.selectAll(".link")
      .data(graph.links)
    .enter().append("line")
      .attr("class", "link")
      .style("stroke-width", function(d) { return Math.sqrt(d.value); });

  var node = svg.selectAll(".node")
      .data(graph.nodes)
    .enter().append("circle")
      .attr("class", "node")
      .attr("r", 5)
      .style("fill", function(d) { return color(d.group); })
      .call(force.drag);

  node.append("title")
      .text(function(d) { return d.name; });

  force.on("tick", function() {
    link.attr("x1", function(d) { return d.source.x; })
        .attr("y1", function(d) { return d.source.y; })
        .attr("x2", function(d) { return d.target.x; })
        .attr("y2", function(d) { return d.target.y; });

    node.attr("cx", function(d) { return d.x; })
        .attr("cy", function(d) { return d.y; });
  });
});

</script>

## Conclusions

That's it!  If you followed along, you should now have a working interactive D3.js plot on your website so you can visualize your own graph or network data.  Thanks for reading.  Send me an email at rtylermclaughlin@gmail.com if you have any questions! 

I'd like to thank the following bloggers for their helpful posts.  Without them I'd have struggled quite a bit longer: [Andrew Mehrmann](http://dkmehrmann.github.io/blog/2016/05/01/d3.html), [Eric Bickel](https://ehbick01.github.io/2017/05/09/embedding-d3-visuals-in-rmarkdown/), and [Tyler Clavelle](https://tclavelle.github.io/blog/blogdown_github/)
