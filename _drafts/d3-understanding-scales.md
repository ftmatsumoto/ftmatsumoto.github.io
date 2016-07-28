---
layout: post
title:  "D3 - understanding scales"
---

> _If visualization is constructing “visual representations of abstract data to amplify cognition”, then perhaps the most important concept in D3 is the scale, which maps a dimension of abstract data to a visual variable._
>
> Mike Bostock, creator of #d3js

Scales in D3 are functions that 'shift' the input values from a certain interval (_input domain_) into another interval (_output range_).

<meta charset="utf-8">
<style>

.axis {
  font: 10px sans-serif;
}

.axis path,
.axis line {
  fill: none;
  stroke: #000;
  shape-rendering: crispEdges;
}

.axis--x path {
  display: none;
}

.line {
  fill: none;
  stroke: steelblue;
  stroke-width: 1.5px;
}

</style>

<div id="scale"></div>
<script src="https://d3js.org/d3.v4.min.js"></script>

<script>
var margin = {top: 20, right: 20, bottom: 30, left: 50},
    width = 960 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;

var ibov = ({{ site.data.ibov | jsonify }});

var parseTime = d3.timeParse("%d-%b-%y");

var xAxis = ibov.map(function(a){ return parseTime(a["﻿Date"]); });
var yAxis = ibov.map(function(a){ return Number(a["Close"]); });

// console.log([xAxis[xAxis.length - 1], xAxis[0]]);
var x = d3.scaleTime()
    .domain([xAxis[xAxis.length - 1], xAxis[0]])
    .range([0, width]);

// console.log(Math.min.apply(null, yAxis));
var y = d3.scaleLinear()
    .domain([Math.min.apply(null, yAxis), Math.max.apply(null, yAxis)])
    .range([height, 0])
    .nice();

var line = d3.line()
    .x(function(d) { return x(parseTime(d["﻿Date"])); })
    .y(function(d) { return y(Number(d["Close"])); });

var svg = d3.select("#scale").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
    .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

  svg.append("g")
      .attr("class", "axis axis--x")
      .attr("transform", "translate(0," + height + ")")
      .call(d3.axisBottom(x));

  svg.append("g")
      .attr("class", "axis axis--y")
      .call(d3.axisLeft(y))
      .append("text")
      .attr("class", "axis-title")
      .attr("transform", "rotate(-90)")
      .attr("y", 6)
      .attr("dy", ".71em")

  svg.append("path")
      .attr("class", "line")
      .attr("d", line(ibov));

</script>
