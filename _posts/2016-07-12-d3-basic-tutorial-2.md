---
layout: post
title:  "D3 basic tutorial 2"
tags: [D3 tutorial, D3 bar chart]
---

[D3](https://d3js.org/) is a JavaScript library for visualizing data in the browser.

This is a basic tutorial on how to create a simple horizontal bar chart using D3.
The chart will render this data:

```javascript
var data = [
  {name: "first bar", value: 3},
  {name: "second bar", value: 7},
  {name: "third bar", value: 23},
  {name: "fourth bar", value: 17},
  {name: "fifth bar", value: 39}
];
```

1\. Create a SVG element where the bar chart will be appended to.

```javascript
var width = 420,
    barHeight = 20;

var chart = d3.select("#bar_chart_20160712")
    .append("svg")
    .attr("width", width)
    .attr("height", barHeight * data.length);
```

2\. Set up the [scale](https://github.com/d3/d3-scale) for the x axis. The [scaleLinear](https://github.com/d3/d3-scale#linear-scales) function construct a new continuous scale with the input domain [0, 1] and output range [0, 1].
By calling the domain and range methods, we're transforming the domain and the range interval.

```javascript
var xAxis = d3.scaleLinear()
    .domain([0, d3.max(data, function(d) { return d.value; })])
    .range([0, width]);
```

3\. Append one [\<g\>](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/g) (SVG element) for each data. The __g element__ is a container used to group other SVG elements. After append the g element, move the elements so they don't stack over each other. After translating the elements, append the [rectangle](https://developer.mozilla.org/pt-BR/docs/Web/SVG/Element/rect) and the [text](https://developer.mozilla.org/pt-BR/docs/Web/SVG/Element/text) for each data point, setting the width of the rect element to the data value (adjusted by the xAxis scale)

```javascript
var bar = chart.selectAll("g")
    .data(data)
    .enter().append("g")
    .attr("transform", function(d, i) { return "translate(0," + i * barHeight + ")"; });

    bar.append("rect")
        .attr("width", function(d) { return xAxis(d.value); })
        .attr("height", barHeight - 1)
        .attr("fill", "blue");

    bar.append("text")
        .attr("x", function(d) { return xAxis(d.value) - 3; })
        .attr("y", barHeight / 2)
        .attr("dy", ".35em")
        .attr("fill", "white")
        .attr("font-size", "10px")
        .attr("font-family", "sans-serif")
        .attr("text-anchor", "end")
        .text(function(d) { return d.value; });
```
And this is the final result...

<div id="bar_chart_20160712"></div>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script>
(function(){
var data = [
  {name: "first bar", value: 3},
  {name: "second bar", value: 7},
  {name: "third bar", value: 23},
  {name: "fourth bar", value: 17},
  {name: "fifth bar", value: 39}
];

var width = 420,
    barHeight = 20;

var chart = d3.select("#bar_chart_20160712")
    .append("svg")
    .attr("width", width)
    .attr("height", barHeight * data.length);

var xAxis = d3.scaleLinear()
    .domain([0, d3.max(data, function(d) { return d.value; })])
    .range([0, width]);

var bar = chart.selectAll("g")
    .data(data)
    .enter().append("g")
    .attr("transform", function(d, i) { return "translate(0," + i * barHeight + ")"; });

    bar.append("rect")
        .attr("width", function(d) { return xAxis(d.value); })
        .attr("height", barHeight - 1)
        .attr("fill", "blue");

    bar.append("text")
        .attr("x", function(d) { return xAxis(d.value) - 3; })
        .attr("y", barHeight / 2)
        .attr("dy", ".35em")
        .attr("fill", "white")
        .attr("font-size", "10px")
        .attr("font-family", "sans-serif")
        .attr("text-anchor", "end")
        .text(function(d) { return d.value; });

})()

</script>
