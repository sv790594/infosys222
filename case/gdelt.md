# <i class="fas fa-database"></i> Case study
### [The GDELT Project](https://www.gdeltproject.org/)
[<i class="fab fa-creative-commons"></i>](https://creativecommons.org/licenses/by/4.0/) [Johnny Chan](mailto:jh.chan@auckland.ac.nz) | <i class="fas fa-calendar"></i> 2018-05-04



## Background
- [The GDELT Project](https://www.gdeltproject.org/) is a global database of society. It monitors the world's broadcast, print, and web news from every country. All events happened in every minute from anywhere are collected in the database and made available for anyone to access

- In particular, [the GDELT event database](http://data.gdeltproject.org/documentation/GDELT-Event_Codebook-V2.0.pdf) stores over 300 [categories](http://data.gdeltproject.org/documentation/CAMEO.Manual.1.1b3.pdf) of activities around the world, where it updates every 15 minutes

- A 1000-row data sample from the event database captured in CSV file format is available [here](https://johnny723.github.io/infosys222/case/results-20180416-200748.csv). (It was downloaded directly from Google BigQuery on the 2018-04-16)



## Specification
- There is one Event table in the database with 61 columns defined. The specification of the table and its columns is documented in the [GDELT Event Codebook](http://data.gdeltproject.org/documentation/GDELT-Event_Codebook-V2.0.pdf)

- To fully understand the data stored in the Event table, extensive [documentation and reference](https://www.gdeltproject.org/data.html#documentation)  are available. For example, the lists of [CAMEO Country Codes](https://www.gdeltproject.org/data/lookups/CAMEO.country.txt) and the [FIPS GEO Country Codes](https://www.gdeltproject.org/data/lookups/FIPS.country.txt) are useful for examining the data associated with country from the Event table. Similarly the list of [CAMEO Event Codes](https://www.gdeltproject.org/data/lookups/CAMEO.eventcodes.txt) explains the category of events



## Objective
- The objective of this case study is to identify the degree of happiness among countries through all the reported events from the sample data



## Q1: Setup
- Use appropriate SQLite commands and SQL statements to setup the working environment:
  - create a new SQLite database
  - create a table named Event that follows the specification; it must be consistent and compatible with the sample data
  - import the sample data into the Event table

- Hint: It may require some non-intrusive tinkering to the sample data CSV file before they could be imported; it is very important to be familiar with the relevant SQLite commands



## Q2: Happiness
- Write a single SQL statement to project a list with appropriate columns that ranks the happiness of all countries from the sample data

- Hint: One way to proxy happiness is to examine the average tone of the reported events to a country; another way is to study the Goldstein scale which reflects the impact to the stability of a country



# THE END
<canvas width=400 height=400 class="anything">
<!--
{
  "initialize": "function(container) {
	var width = container.width,
	    height = container.height;
	var projection = d3.geo.orthographic()
	    .translate([width / 2, height / 2])
	    .scale(width / 2 - 20)
	    .clipAngle(90)
	    .precision(0.6);

	var c = container.getContext('2d');

	var path = d3.geo.path()
	    .projection(projection)
	    .context(c);

	var title = container.parentElement.querySelector('.country');
	queue()
	    .defer(d3.json, '../asset/globe/world-110m.json')
	    .defer(d3.tsv, '../asset/globe/world-country-names.tsv')
	    .await(ready);

	function ready(error, world, names) {
	  if (error) throw error;

	  var globe = {type: 'Sphere'},
	      land = topojson.feature(world, world.objects.land),
	      countries = topojson.feature(world, world.objects.countries).features,
	      borders = topojson.mesh(world, world.objects.countries, function(a, b) { return a !== b; }),
	      i = -1,
	      n = countries.length;

	  countries = countries.filter(function(d) {
	    return names.some(function(n) {
	      if (d.id == n.id) return d.name = n.name;
	    });
	  }).sort(function(a, b) {
	    return a.name.localeCompare(b.name);
	  });

	  (function transition() {
	    d3.transition()
	        .duration(1250)
	        .each('start', function() {
			while ( !countries[i = (i + 1) % n] ) {};			
			title.innerHTML = (countries[i].name);
	        })
	        .tween('rotate', function() {
	          var p = d3.geo.centroid(countries[i]),
	              r = d3.interpolate(projection.rotate(), [-p[0], -p[1]]);
	          return function(t) {
	            projection.rotate(r(t));
	            c.clearRect(0, 0, width, height);
	            c.fillStyle = '#fff', c.lineWidth = 2, c.beginPath(), path(globe), c.fill();
	            c.fillStyle = '#42affa', c.beginPath(), path(land), c.fill();
	            c.fillStyle = '#f00', c.beginPath(), path(countries[i]), c.fill();
	            c.strokeStyle = '#ccc', c.lineWidth = .5, c.beginPath(), path(borders), c.stroke();
	            c.strokeStyle = '#ccc', c.lineWidth = 2, c.beginPath(), path(globe), c.stroke();
	          };
	        })
	      .transition()
	        .each('end', transition);
	  })();
	}

	d3.select(self.frameElement).style('height', height + 'px');

    }"
}
-->
</canvas>

Database is awesome in <span class="country">everywhere</span>!

[<i class="fas fa-print"></i>](?print-pdf#)
