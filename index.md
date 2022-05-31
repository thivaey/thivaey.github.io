---
layout: default
title: Home
navigable: false
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



<div id='d3div'></div>



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

d3.json("../../../../scripts/jazz_scales_network_symmetric.json", function(error, graph) {
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


# Hello!


My name is Tyler McLaughlin and I'm a PhD-trained scientist living in San Francisco, CA.  I am currently working at [AbbVie biopharmaceuticals](https://www.abbvie.com/), doing a postdoc in Computational Immuno-Oncology.  My scientific career began with computational systems biology research in Pittsburgh, PA and Farmington, CT.  I had a math and molecular biology double major in undergrad and so this focus felt natural.  Continuing in this scientific direction, during my PhD in Systems, Synthetic, and Physical Biology at Rice University in Houston, TX, my research involved human cell biophysics and systems biology, conducting both wet lab experiments and extensive amounts of image-based and statistical data analysis.   After that I was a Health Data Science Fellow at [Insight Data Science](https://www.insighthealthdata.com) in Silicon Valley during the Fall 2018 session where I did a project involving deep learning and brain waves.  You can learn more about me [here on LinkedIn](www.linkedin.com/in/r-tyler-mclaughlin-phd).

# Posts

<ul style="padding-left:0px;">
  {% for post in site.categories.blog %}

      <h2>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </h2>

      <span class="text-warning">{{ post.date | date: "%b %-d, %Y" }}</span>
      <p>{{ post.content | strip_html | truncatewords:75}}</p>
      <a href="{{ post.url | prepend: site.baseurl }}">Read more...</a><br>

  {% endfor %}
</ul>




[md](_posts/2018-04-29-jazz-scale-networks.md)
[html](_posts/2018-04-29-jazz-scale-networks.html)


