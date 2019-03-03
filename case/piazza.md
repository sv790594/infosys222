# <i class="fas fa-suitcase"></i> Case study
### Piazza: Discussion Management System
[<i class="fab fa-creative-commons"></i>](https://creativecommons.org/licenses/by/4.0/) [Johnny Chan](mailto:jh.chan@auckland.ac.nz) | <i class="fas fa-calendar"></i> 2019-03-18



## Specification
- The following specification captures only part of the current release of Piazza at the time of writing, a discussion management system that is used by many tertiary institutions

- This case is aimed for learning and practicing data modelling; it is not affiliated with nor supported by any organisation



## Background
- [Piazza][1] is created with one goal: to enable students learning from each other collaboratively in a shared virtual space through questions and answers, even they are physically separated or being too shy to engage in face-to-face

- It offers a [web service][2] that combines a forum with a wiki, which means the contents are version controlled and editable among participants
[1]: https://piazza.com/about/story
[2]: https://en.wikipedia.org/wiki/Piazza_(web_service)



## Account
- To join Piazza for the first time, signing up an account is required. An account consists of account ID, first name, last name, preferred email, password and avatar. It may associate with more emails for receiving notification

- After an account is created, it may be enrolled to a class as either student or instructor but not both. However, an account could have an instructor role in one class, and a student role in another class

- As part of the enrolment, an account specifies their preference of notification for new post and updated post. The options for preference is standardised (e.g. real time, daily digest, smart digest and no emails). There is an additional option to automatically follow every post in a class



## Class
- A class is composed of class ID, class number, name, estimated size, start date, status and url. The class ID is used as unique identifier, whereas the class number and name are common references used in the school

- When a class is created, it must be associated with a school (e.g. The University of Auckland) and a term (e.g. Semester 1 2018). A term is named specifically to follow a school's convention; and a school must register with one domain or more (e.g. aucklanduni.ac.nz and auckland.ac.nz)

- The instructor who creates a class is always the first account enrolled; and a list of folders (e.g. lecture, lab, general etc) is defined to manage posts



## Post
- While a post associates with a class ID and a post number, a post ID is used as the unique identifier for each post among all classes. It has post type, post date, summary, detail, and whether it is pinned. Each post must be tagged with one or more folders

- There are two post types: note or question. A note is a post that requires no answer; whereas a question expects answers. Each question associates at most with one student's answer and one instructor's answer

- A post may have follow-ups, and a follow-up may have comments. Whether it is an answer, a follow-up or a comment, essentially they all have a detail attribute to store the content. A follow-up has an additional attribute to indicate if it has been resolved



## Version
- When a writing (i.e. post, answer, follow-up or comment) is composed, a version is created. A writing must associate with one version or more

- In Piazza, the latest version of a writing would be displayed by default; but all versions are accessible for tracking the development of the writing

- Each version of a writing is composed by one and only one account, and the composer could choose if their identity is revealed or anonymised



## Read
- When a post is viewed by an account the first time (which may include answers, follow-ups and comments), a read is created with a read date. Any further views or actions from the reader would update the read date

- A reader may endorse a post and / or the associated answers if it is a question. Furthermore, a reader may follow a post to receive update notification. A reader may star a post as one of their favorites

- The actions of endorsement, following and starring could be undone and / or redone if a reader chooses to do so



## Login (Bonus)
- Piazza could tell the total number of enrolled accounts in a class that are currently logged on, and the total number of enrolled accounts in a class that have logged on in the last seven days



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
