# <i class="fas fa-database"></i> Week 00
### Practical information
[<i class="fab fa-creative-commons"></i>](https://creativecommons.org/licenses/by/4.0/) [Johnny Chan](mailto:jh.chan@auckland.ac.nz)



## <i class="fas fa-users"></i> People
Role | Name
--- | ---
Lecturer | Johnny Chan [<i class="far fa-envelope fa-pull-right"></i>](mailto:jh.chan@auckland.ac.nz)
Coordinator | Udayangi Muthupoltotage [<i class="far fa-envelope fa-pull-right"></i>](mailto:u.muthupoltotage@auckland.ac.nz)
Tutor | Jono Davis, Haley Duong, Jing Geng, Destin Gong, Mahmudul Hasan, Jamie Weon
Student | 200+ of you from A to Z



## <i class="fas fa-road"></i> Activity
- Lecture (Week01 - Week12)
	- Mon 3-5 @ 260-092
	- Tue 2-3 @ 260-092

- Lab (Week02 - Week11)
	- A 1.5-hr lab session with assessment
	- Attend only your enrolled session

- Working on your own
	- No less than 6 hours per week (reading, practicing etc)
	- Knowledge → Understanding → Skill



## <i class="fas fa-list-ol"></i> Assessment
- Internal (60%)
	- A1 (P1): due on 2019-08-09 Fri 18:00 (2%)
	- A1 (P2): due on 2019-08-23 Fri 18:00 (8%)
	- A2 (P1): due on 2019-10-04 Fri 18:00 (5%)
	- A2 (P2): due on 2019-10-11 Fri 18:00 (15%)
	- Lab: due on selected weeks (3,4,6,7,8,9,11) (10%)
	- Test: held on 2019-09-20 Fri 18:30-20:00 (20%)

- Exam (40%)
	- A 3-hr closed book examination

📢 Students __must__ [pass](https://uoa.custhelp.com/app/answers/detail/a_id/2748/~/marking-schemes-or-grade-scales-at-the-university-of-auckland) both internal and exam separately



## <i class="fas fa-calendar-alt"></i> Schedule
Week | Lecture | Lab
--- | --- | ---
01 | Introduction | No lab
02 | Relational model | Introduction
03 | ER modelling | ER diagram
04 | Data modelling | Data modelling
05 | Data modelling | Workshop
06 | Normalisation | Normalisation
07 | SQL | SQL
08 | SQL | SQL
09 | SQL | SQL
10 | DBMS fundamentals | Workshop
11 | Data warehouse | Data warehouse
12 | Review | No lab



## <i class="fas fa-wrench"></i> Resource
- LMS: All course related material could be found in [Canvas](https://canvas.auckland.ac.nz/courses/38861) including announcements, lecture slides, lab slides, assessment specifications, and marks

- Forum: [Piazza](http://piazza.com/aucklanduni.ac.nz/semester22019/infosys222)

- Reading: There is no textbook nor course book. Most readings for this course could be accessed online via [Talis reading lists](https://rl.talis.com/3/auckland/lists/7802C21C-4BDD-E5F6-B995-2891E3861399.html)

- Practice: [Previous exams](https://www.library.auckland.ac.nz/search/INFOSYS%20222#uoa-lib-ms-exams)

- Software: [Draw.io](https://www.draw.io), [SQLite](http://sqlite.org/) and [Atom](https://atom.io/)



## <i class="fas fa-phone-volume"></i> Communication
- Forum: This is the primary platform for Q&A related to any course material and administrative matter. Please check if your question has been asked and answered before posting one

- Email: Use it for _private message_ only. Direct all administrative matter and lab material to the [coordinator](mailto:u.muthupoltotage@auckland.ac.nz); lecture material and assessment question to the [lecturer](mailto:jh.chan@auckland.ac.nz)

- Office Hour: By appointment only (Tue 1-2 @ 260-441)

- Class Rep: Anyone?

- Twitter: [@infosys222](https://twitter.com/infosys222)

- Slido: [#INFOSYS222](https://canvas.auckland.ac.nz/courses/38861/modules/items/674768)



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
