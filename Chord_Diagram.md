#Chord Diagram
Chord Diagram to represent a relation between a set of data.
This diagram however is not interactive.

[Simple Chord Diagram](SimpleChordDiagram.jpg "Simple Chord Diagram")

##HTML file.
```html
<!-- File type -->
<!DOCTYPE html>
<html>
	<head>
    <!-- Defining Language -->
		<meta charset="utf-8">
    
    <!-- Defining Style for body of HTML -->
		<style>
			body{ 
				font: 10px sans-serif;}
			.group-tick line {
  				stroke: #000;}
			.ribbons {
 		 		fill-opacity: 0.67;}
		</style>

    <!-- Defining svg object -->
		<svg width="700" height="700"></svg>

    <!-- Scripts in use -->
		<script src="https://d3js.org/d3.v4.min.js"></script>
		<script lang="javascript" src="ChordDiagram.js"></script>
	
    <!-- Web title -->
		<title>Chord Diagram</title>
	</head>
	<body>
	</body>
</html>
```

##JavaScript file.
```javascript
/* Created by Javier Rodriguez on 09/15/2017					
   Louisiana State University - EJ Ourso College of Business*/

	let matrix = [[55,58,96,56,52],
                [05,88,22,29,21],
                [55,58,96,56,52],
                [05,88,22,29,21]];											//Array to handle matrix.
	let aColor = ["FF0000","FFD000","69FF00","00FFD8"]; 	//Array to store html colors.										
	
	//Drawing.
	let svg = d3.select("svg"),								            //SVG object.
    width = +svg.attr("width"),								          //Defining  width.
    height = +svg.attr("height"),							          //Defining height.
    outerRadius = Math.min(width, height) * 0.5 - 40,		//Outer radius.
    innerRadius = outerRadius - 15;							        //Defining inner.
	
	let formatValue = d3.formatPrefix(",.0", 1e1);			  //Formatting text. ex 20k

	let chord = d3.chord()									              //Formatting chord diagram.
    .padAngle(0.01)											                //Space between groups.
    .sortSubgroups(d3.desceding);							          //How chords will be sort.

	let arc = d3.arc()										                //Variable to format where chords start/end.
    .innerRadius(innerRadius)
    .outerRadius(outerRadius);

	let ribbon = d3.ribbon()								               //Variable to determine where chords are born.
    .radius(innerRadius);

	let color = d3.scaleOrdinal()							             //Adding colors.
    .domain(matrix.length)									             //Number of groups.
    .range(aColor);											                 //Range of colors.

	let g = svg.append("g")									               //Creating g object.
    .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")")
    .datum(chord(matrix));
    
	let group = g.append("g")								              //Grouping elements.
    .attr("class", "groups")
  	.selectAll("g")
  	.data(function(chords) { return chords.groups; })
  	.enter().append("g");

	group.append("path")									                //Displaying Arc.
    .style("fill", function(d) { return color(d.index); })
    .style("stroke", function(d) { return d3.rgb(color(d.index)).darker(); })
    .attr("d", arc);


	let groupTick = group.selectAll(".group-tick")			//Defines separation between ticks.
  	.data(function(d) { return groupTicks(d, 1e3); })
  	.enter().append("g")
    .attr("class", "group-tick")
    .attr("transform", function(d) { return "rotate(" + (d.angle * 180 / Math.PI - 90) + ") translate(" + outerRadius + ",0)"; });


	groupTick.append("line")								           //Defines size of ticks.
    .attr("x2", 6);


	groupTick 												                 //Displays ticks on chart.
  	.filter(function(d) { return d.value % 5e3 === 0; })
  	.append("text")
    .attr("x", 8)											              //How far away do you want ticks to be.
    .attr("dy", ".35em")									          //How center you want ticks to be
    .attr("transform", function(d) { return d.angle > Math.PI ? "rotate(180) translate(-16)" : null; })
    .style("text-anchor", function(d) { return d.angle > Math.PI ? "end" : null; })
    .text(function(d) { return formatValue(d.value); });

	g.append("g")											                 //Formatting Chords. Colors and borders.
    .attr("class", "ribbons")
  	.selectAll("path")
  	.data(function(chords) { return chords; })
  	.enter().append("path")
    .attr("d", ribbon)
    .style("fill", function(d) { return color(d.target.index); })
    .style("stroke", function(d) { return d3.rgb(color(d.target.index)).darker(); });

	// Returns an array of tick angles and values for a given group and step.
	function groupTicks(d, step) {
  		let k = (d.endAngle - d.startAngle) / d.value;
  			return d3.range(0, d.value, step).map(function(value) {
    		return {value: value, angle: value * k + d.startAngle};
  		});
	}
```
