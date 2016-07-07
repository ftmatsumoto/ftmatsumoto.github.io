---
layout: post
title:  "D3 basic tutorial"
tags: [D3 tutorial, D3 drag]
---

[D3](https://d3js.org/) is a JavaScript library for visualizing data in the browser.

This is a basic tutorial on how to make a draggable circle using D3.

1\. Create a SVG element where the circle will be appended to.

```javascript
var width = 400,
    height = 300,
    radius = 20;

var div = d3.select("#eg_drag")
            .style("width", width + "px")
            .style("height", height + "px")
            .style("border", "1px solid black")

var container = div.append("svg")
                  .attr("width", width)
                  .attr("height", height);
```

2\. Create a [new drag behavior](https://github.com/d3/d3-drag#drag). This object can be applied to the selected element via [selection.call](https://github.com/d3/d3-selection#selection_call). The drag behavior object will attach an event listener [selection.on](https://github.com/d3/d3-selection#selection_on) on the binded element.

```javascript
var drag = d3.drag()
             .on("drag", function(d){
                d3.select(this)
                  .attr("cx", d.x = Math.max(radius, Math.min(width - radius, d3.event.x)))
                  .attr("cy", d.y = Math.max(radius, Math.min(height - radius, d3.event.y)));
             })
```

3\. Create the element that you want to drag and apply the drag behavior using the [selection.call](https://github.com/d3/d3-selection#selection_call).

```javascript
var circle = container
                  .append("circle")
                  .data([{'x': width / 2, 'y': height / 2, 'r': radius}])
                  .attr("cx", function(d) { return d.x; })
                  .attr("cy", function(d) { return d.y; })
                  .attr("r", function(d) { return d.r; })
                  .attr("background-color", "blue")
                  .call(drag)
```

And this is the final result...

<div id="eg_drag"></div>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script>
  var width = 400,
      height = 300,
      radius = 20;

  var div = d3.select("#eg_drag")
              .style("width", width + "px")
              .style("height", height + "px")
              .style("border", "1px solid black")

  var container = d3.select("#eg_drag").append("svg")
                    .attr("width", width)
                    .attr("height", height);

  var drag = d3.drag()
               .on("drag", function(d){
                  d3.select(this)
                    .attr("cx", d.x = Math.max(radius, Math.min(width - radius, d3.event.x)))
                    .attr("cy", d.y = Math.max(radius, Math.min(height - radius, d3.event.y)));
               })


  var circle = container
                    .append("circle")
                    .data([{'x': width / 2, 'y': height / 2, 'r': radius}])
                    .attr("cx", function(d) { return d.x; })
                    .attr("cy", function(d) { return d.y; })
                    .attr("r", function(d) { return d.r; })
                    .attr("background-color", "blue")
                    .call(drag)

</script>
