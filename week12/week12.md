# 🗄 Week 12
### Review
[©](https://creativecommons.org/licenses/by/4.0/) [Johnny Chan](mailto:jh.chan@auckland.ac.nz)



## 🕒 Previously ...

- Data warehouse

- Star schema



## 📌 Agenda

- Case studies

- Course review



# 💼 Case study
### Fisheries corporation<!-- .slide: data-background="fishery.jpg" data-background-transition="zoom" -->


## Background
- The Fisheries Corporation has decided to store information regarding fishing vessels as well as catches (of fish) in a database. The database will also hold other organisational data. The case study given is not comprehensive and has been adapted to keep within the scope of the examination


## Company
- Many companies may be formed under a single parent company. When a company lists within the Fisheries Corporation, the company is given a registration number. The company address and contact numbers are also recorded with the Fisheries Corporation. the address and contact details may change over time


## Vessel
- Each vessel has an FVN (fishing vessel number), a date of construction, a size (meters) and displacement (tonnes). Each vessel also has a fishing configuration (e.g. long-line, trawl, seine, and jig), which may change over time. Each fishing vessel is given a vessel name

- Each vessel must be registered to a single company at any point in time. However, over a period of time a vessel may be registered to many companies. Each company may have multiple vessels registered at any point in time


## Area
- Each fishing vessel is assigned to a fishing area. A vessel may only be assigned to a single area at any point in time, but over a period of time it may be assigned to any number of areas. Each fishing area has an area identifier, and a total allowable catch quota (TAC) for each fish species that live in the area. Note that this TAC quota is updated on a yearly basis

- Each fish species recorded by the Fisheries Corporation has a species code, a common name and a scientific name


## Quota
- In order to commercially fish, a quota must be purchased from the Fisheries Corporation. Each quota is designated by a fishing quota number (FQN), and specifies the area, the species, and the quantity that a company may fish. A quota is for a fixed period of time. Each quota specifies a single species and a single area

- For each fishing assignment, a vessel has a number of catches recorded against it. The information recorded about a catch includes the exact location (longitude and latitude), the depth, the sea temperature, and when the catch was taken. Each species may be caught in one or more catches, and a catch may contain a number of different species of fish. For each species in the catch the quantity (tonnes) is recorded


## Data modelling
- Draw a logical ERD in crow’s foot notation for the case

- Please note: all important design decisions and assumptions made must be clearly 	listed. All entity sets (with specialisation / generalisation as needed), all attributes including primary and foreign keys, and all relationships including cardinalities must be shown as appropriate. Following good database design principle, the ERD should not contain redundant entity sets, relationships or attributes


## Does this solution satisfy the case?
![fish-erd](fish-erd.png)
<!-- .element: height="600px" -->


## Normalisation
- The Senior Members Fishing Association wishes to keep track of its members, their type of membership, and who attended the Association's Annual General Meetings (AGM). The Association has created one table to store all this information as shown below

```
AGM_ATTENDANCE(AGMDate, memberNo, memberName,
membershipType, membershipTypeDescription,
comments_made_at_AGM)
```

- Derive a set of relations in third normal form (3NF) from the given table. Show each step of normalisation clearly. Indicate primary keys and foreign keys with solid / dashed lines or labels. State your assumptions clearly in your answer


## Data warehouse
- It is identified that a data warehouse will be useful to analyse the information held in the Fisheries Corporation's database

- The Fisheries Corporation wishes to analyse data monthly about the fish species caught in each fishing area, on different vessels over a period of three years. They would also like to find out which companies caught which species of fish in which area

- Draw a star schema for the required data mart



# 💼 Case study
### Book database<!-- .slide: data-background="book.jpg" data-background-transition="zoom" -->


## Specification
![](bookerd.png)
<!-- .element: height="600px" -->


## Question 01
- Write an SQL statement to create the table WRITING


## Question 02
- Write an SQL statement to list all staff who do not live in Auckland. The output should be displayed to show the Staff_Code in one column and the Staff_Firstname and Staff_Lastname as staffname in another column


## Question 03
- A customer calls in at the bookshop to inquire who the author is of a particular book. Unfortunately he cannot remember the whole title and gives the following string - 'One flew'. Write a SQL query to list all the books that have this string as part of the book title


## Question 04
- The Inventory manager of the book store wants you to list all the book titles and the total quantity received for each book. Make sure all books that do not have a quantity received are also listed


## Question 05
- List the current quantity of books available for each title. Assume that this can be calculated by first working out the total quantity received for each book as well as the total quantity of each book sold. You may use one or more views or sub-queries as needed



# 💼 Case study
### Jump-a-lot gym database<!-- .slide: data-background="gym.jpg" data-background-transition="zoom" -->


## Background
- Theo Lee, the manager of the Jump-a-lot Gym, wishes to store the details of gym memberships and course payments in a database. You are required to design the logical data model that captures the information in preparation for implementing the application in a relational database. The data that Theo wishes to store are as follow


## Membership
- The gym sells two categories of membership:
  - an individual (i.e. for one person) category
  - an organisation category

- Each of those category must be one of four membership types:
  - one-month membership type
  - three-month membership type
  - six-month membership type
  - twelve-month membership type


## Purchase
- The purchase charge for a membership can differ for each membership type, for each category, and may change over time

- Individuals and organisations purchase a specific type of membership for their respective category

- The gym wishes to store the details about a person who purchases membership: member number, first name, family name, first line of address, second line of address, city, postcode, email address, preferred telephone contact number, date of birth, gender, description of any physical problems

- The data required for an organisation that buys a membership is: member number, organisation name, first line of address, second line of address, city, postcode, email address, preferred telephone contact number. An organisation membership simply allows employees of the organisation to use the gym


## Member
- The member number is a unique number that identifies the member, whether individual or organisation

- A member is an individual or an organisation that has purchased a type of membership. It is important to record when a membership is bought and the type of membership that is bought

- It is planned to retain in the database the details of people and organisations whose memberships have expired, but they must have purchased a membership at least once in order to obtain a member number

- A person or organisation will only become a member when the purchase price is paid. When a membership expires, a new membership must be purchased. Theo wishes to query the database to discover which memberships are close to expiry, so that reminder emails may be sent


## Class
- Only individual members may pay to attend aerobic classes. Classes are held weekly for a term. The details to be stored about an aerobic class are: class code, class description, duration of one class in hours, and the total number of weeks over which that class is held

- The individual is charged one amount for the whole term for the class and the charge may change over time. The enrolment will only be accepted with payment


## Stream
- There may be several streams of a particular class offered concurrently and over time. The details to be stored about a stream are: the stream number which is individual to the class, and the stream start date, and stream start time. Thus several classes may have a stream number one, for example

- Individuals enrol in a stream. The enrolment date needs to be retained in the database for the financial records as individuals must pay on enrolment


## Booking
- Organisations that are members can book the gym for the exclusive use of their staff. The time that may be booked by an organisation are to be stored in the database: day of the week, start time, duration

- The data to be retained about a booking are: unique booking number, date of booking, available time chosen


## Data modelling
- Draw a logical ERD in crow’s foot notation for the case

- Please note: all important design decisions and assumptions made must be clearly 	listed. All entity sets (with specialisation / generalisation as needed), all attributes including primary and foreign keys, and all relationships including cardinalities must be shown as appropriate. Following good database design principle, the ERD should not contain redundant entity sets, relationships or attributes


## Does this solution satisfy the case?
![gym-erd](gym-erd.png)
<!-- .element: height="600px" -->



# 💼 Case study
### Auckland linen supplies<!-- .slide: data-background="linen.jpg" data-background-transition="zoom" -->


## Specification
![](invoice.png)
<!-- .element: height="600px" -->


## Normalisation
- Derive a set of relations in third normal form (3NF) from the given table. Show each step of normalisation clearly. Indicate primary keys and foreign keys with solid / dashed lines or labels. State your assumptions clearly in your answer



# 💼 Case study
### Magazine subscription system<!-- .slide: data-background="magazine.jpg" data-background-transition="zoom" -->


## Specification
![](magazineerd.jpg)


## Question 01
- Write a SQL statement to create the table SUB COST which stores the history of the costs of subscriptions for a magazine for specific periods (number of months). Assume that all related tables have already been created. The maximum cost for a subscription will be $400.00


## Question 02
- Write a SQL statement to find the original cost of a subscription for six months (periodID = '06'), to the magazine with the magazineID of '12345'


## Question 03
- Write a SQL statement to list the names of all articles that have been printed in magazine issues between and including the dates of 2008-01-01 and now, with their authors names. Authors names should be displayed under a column heading of "Author" and should be in the form of family name, initial (first character) of first name (e.g. Costain, G). Ensure that all articles are included in the list, including those that do not have authors. Note: Authors are always persons


## Question 04
- Write a SQL statement to display the order number, the subscriber first name and family name, the organisation name, and the total cost of all order items for order number 12234


## Question 05
- Subscribers may be people or organisations. An organisation's name is stored in the attribute orgName in the table SUBSCRIBER. Write a SQL statement to create a view of all subscribers who are organisations. Use a relational operator to list all subscribers who are not organisations (i.e. have no organisation name)



## Course review
Week | Content
--- | ---
01 | Introduction ✓
02 | Relational model ✓
03 | ER modelling ✓
04 | Data modelling ✓
05 | Data modelling✓
06 | Normalisation ✓
07 | SQL ✓
08 | SQL ✓
09 | SQL ✓
10 | DBMS fundamentals ✓
11 | Data warehouse ✓
12 | Review ✓


## Beyond RDBMS
- A NoSQL database provides a mechanism for storage and retrieval of data which is modelled in means other than tabular relations used in relational databases

- A NoSQL database may choose to trade consistency for availability and performance
  - lack of true ACID transaction
  - provide "eventual consistency"
  - suffer from lost update and dirty read
  - support real-time analytics on huge amount of data

- [MongoDB](https://www.mongodb.com/)
  - document-based (JSON)
  - open source
  - adapted by Adobe, eBay, FIFA (video game), Foursquare, LinkedIn, McAfee, CERN



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

[🖨](?print-pdf)
