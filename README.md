D3.js Tech Talk
=================

An introduction to the [D3.js](http://d3js.org/) visualization library based on Scotty Murray's D3 tutorials.

#The Seven Stages of Visualizing Data
*By Ben Fry*

1. Acquire
2. Parse
3. Filter
4. Mine
5. Represent <-- We will focus mainly on 5-7.
6. Refine
7. Interact


#Getting Started
Download the tutorial files [here](https://github.com/morengab/d3sleepinghackers/archive/master.zip) OR by cloning the project.


#Setting Up the Canvas
```javascript
var w = 1000;
var h = 400;
var barPadding = 1;

var svg = d3.select("body")
  .append("svg")
  .attr("width", w)
  .attr("height", h)
```

#Drawing Shapes
```javascript
var dataset = [ 5, 10, 15, 20, 25 ];

svg.selectAll("rect")
  .data(dataset)
  .enter()
    .append("rect")
    .attr("x", function(d, i) {
      return i * (w / dataset.length); //Bar width of 20 plus 1 for padding
    })
    .attr("y", function(d) {
      return h - (d * 2);
    })
    .attr("width", w / dataset.length - barPadding)
    .attr("height", function(d) {
      return d * 2;
    })
    .attr("fill", "teal")
    .style("opacity", "0.7")
    .on("mouseover", function() {
      d3.select(this)
        .transition()
        .style("opacity", 1.0);                   
      })
    .on("mouseout", function() {
      d3.select(this)
        .transition()
        .style("opacity", 0.7);
    });
```

#Labeling
```javascript
svg.selectAll("text")
  .data(dataset)
  .enter()
    .append("text")
    .text(function(d) {
      return d;
    })
    .attr("x", function(d, i) {
      return i * (w / dataset.length) +  (w / dataset.length - barPadding) / 2;
    })
    .attr("y", function(d) {
      return h - (d * 2) + 14;
    })
    .attr("font-family", "sans-serif")
    .attr("font-size", "11px")
    .attr("fill", "white")
    .attr("text-anchor", "middle");
```

#Processing Data
```javascript
var data = new Object();
for (var i=0; i < PeopleNames.length; i++) {
  data[PeopleIDs[i]] = [PeopleNames[i], new Array()];
}
        
for (var i=0; i < AvailableAtSlot.length; i++) {
  for (var j=0; j < AvailableAtSlot[i].length; j++) {
    var ID = AvailableAtSlot[i][j];
    var slots = data[ID][1];
    slots.push(TimeOfSlot[i]);
  }
}

var dataset = new Array();
var keys = Object.keys(data);
for (var i=0; i < keys.length; i++) {
  var key = keys[i];
  var personObj = data[key];
  var numSlots = personObj[1].length;
  dataset.push(personObj);
}
```

#Adding Tooltips
```javascript
$('svg rect').tipsy({ 
  gravity: "w", 
  html: true, 
  title: function() {
    var d = this.__data__;
      return d[0] + " slept " + d[1].length / 4 + " hours"; 
    }
});
```

#References: 
* [D3 Tutorials by Scott Murray](http://alignedleft.com/tutorials/d3/)
* [Visualizing Data by Ben Fry](http://benfry.com/writing/)
* [D3 API Reference](https://github.com/mbostock/d3/wiki/API-Reference)
