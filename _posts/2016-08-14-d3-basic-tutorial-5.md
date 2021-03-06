---
layout: post
title:  "D3 basic tutorial 5"
tags: [D3 tutorial, D3 donut chart]
---

[D3](https://d3js.org/) is a JavaScript library for visualizing data in the browser.

This is a basic tutorial on how to create a simple donut chart using D3. A donut chart is just a [pie chart](https://ftmatsumoto.github.io/2016/08/07/d3-basic-tutorial-4/) with a hole in the middle and the process to create it is very similar to the pie chart.
The chart will render random data generated by the following function:

```javascript
var data = [];
for (var i = 1; i < 8; i++) {
  data.push({id: i, value: (Math.random() * 100)})
}
```

1\. Create a SVG element where the pie chart will be appended to. This SVG element has margins that follow the [margin convetion](https://bl.ocks.org/mbostock/3019563). Append \<g\> element to the SVG and translate it to the middle of the SVG element.

```javascript
var margin = {top: 20, right: 30, bottom: 30, left: 50},
    width = 700 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom,
    radius = Math.min(width, height) / 2;

var svg = d3.select("#donut_chart_20160814").append("svg")
    .attr("width", width)
    .attr("height", height)
  .append("g")
    .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");
```

2\. Set up arcs using [D3 arc generator](https://github.com/d3/d3-shape#arcs). The first arc will be set as the [d attribute](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/d) in the SVG path element, the second arc will hold the text element. **This is the only step that is different from the [pie chart](https://ftmatsumoto.github.io/2016/08/07/d3-basic-tutorial-4/).**

```javascript
var arc = d3.arc()
    .outerRadius(radius - 10)
    .innerRadius(radius - radius * 0.5);

var labelArc = d3.arc()
    .outerRadius(radius - 40)
    .innerRadius(radius - 40);
```

3\. Create a color scale using the [scaleOrdinal function](https://github.com/d3/d3-scale#ordinal-scales).

```javascript
var color = d3.scaleOrdinal()
    .range(["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56", "#d0743c", "#ff8c00"]);
```

4\. Now, create the pie chart that will be appended to the \<g\> element inside the SVG element. To do that we need to use the [pie generator](https://github.com/d3/d3-shape#pies) that does not produce a shape directly but computes the necessary angles to produce the chart. *Value method* sets the value accessor for each element in the input data array, if no function is passed, the default value accessor assumes that the input data are numbers. After that, append \<g\> element with .arc class that later will hold the [path element](http://www.w3schools.com/svg/svg_path.asp) that will shape the 'slice' and the [text element](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/text) that will hold the label for that 'slice'.

```javascript
var pie = d3.pie()
    .value(function(d) { return d.value; })(data);

var g = svg.selectAll(".arc")
    .data(pie)
  .enter().append("g")
    .attr("class", "arc");
```

5\. Finally, append [path element](http://www.w3schools.com/svg/svg_path.asp) and [text element](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/text) to each \<g\> element.

```javascript
g.append("path")
    .attr("d", arc)
    .style("stroke", "#fff")
    .style("fill", function(d) { return color(d.data.id); });

g.append("text")
    .attr("transform", function(d) { return "translate(" + labelArc.centroid(d) + ")"; })
    .attr("dy", ".35em")
    .style("font-size", "16px")
    .style("text-anchor", "middle")
    .text(function(d) { return d.data.id; })
```

And this is the final result...

<div id="donut_chart_20160814"></div>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script>

(function(){
  var data = [];
  for (var i = 1; i < 8; i++) {
    data.push({id: i, value: (Math.random() * 100)})
  }

  var margin = {top: 20, right: 30, bottom: 30, left: 50},
      width = 700 - margin.left - margin.right,
      height = 500 - margin.top - margin.bottom,
      radius = Math.min(width, height) / 2;

  var svg = d3.select("#donut_chart_20160814").append("svg")
      .attr("width", width)
      .attr("height", height)
    .append("g")
      .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");

  var arc = d3.arc()
      .outerRadius(radius - 10)
      .innerRadius(radius - radius * 0.5);

  var labelArc = d3.arc()
      .outerRadius(radius - 40)
      .innerRadius(radius - 40);

  var color = d3.scaleOrdinal()
      .range(["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56", "#d0743c", "#ff8c00"]);

  var pie = d3.pie()
      .value(function(d) { return d.value; })(data);

  var g = svg.selectAll(".arc")
      .data(pie)
    .enter().append("g")
      .attr("class", "arc");

  g.append("path")
      .attr("d", arc)
      .style("stroke", "#fff")
      .style("fill", function(d) { return color(d.data.id); });

  g.append("text")
      .attr("transform", function(d) { return "translate(" + labelArc.centroid(d) + ")"; })
      .attr("dy", ".35em")
      .style("font-size", "16px")
      .style("text-anchor", "middle")
      .text(function(d) { return d.data.id; });
})()

</script>
