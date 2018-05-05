# <i class="fas fa-database"></i> Case study
### Book database
[<i class="fab fa-creative-commons"></i>](https://creativecommons.org/licenses/by/4.0/) [Johnny Chan](mailto:jh.chan@auckland.ac.nz) | <i class="fas fa-calendar"></i> 2018-05-04



## Background
- Book is a sample SQLite database designed by INFOSYS 222

- The database can be created by running a single [SQL script](book)

- The company behind the database runs a business of selling books with multiple branches. The database manages all the transactions, and all relevant information related to the books, authors and their staff members



## Specification
- The following specification captures the Book database schema through all the CREATE TABLE statements that build up the database:

  - Branch
  - Publisher
  - Author
  - Book
  - BookPrice
  - Writing
  - TransactionType
  - Inventory
  - BookGrade
  - Role
  - Staff
  - StaffAssignment


## Branch
```
CREATE TABLE Branch
(
  branchNo INTEGER PRIMARY KEY NOT NULL,
  branchName TEXT,
  branchStreet TEXT,
  branchSuburb TEXT,
  branchCity TEXT
);
```


## Publisher
```
CREATE TABLE Publisher
(
  pubCode INTEGER PRIMARY KEY NOT NULL,
  pubName TEXT,
  pubCity TEXT,
  pubCityCode TEXT,
  parentPubCode INTEGER,
  FOREIGN KEY (parentPubCode) REFERENCES Publisher (pubCode)
  ON UPDATE NO ACTION ON DELETE NO ACTION
);
```


## Author
```
CREATE TABLE Author
(
  authorNo INTEGER PRIMARY KEY NOT NULL,
  authorFirstName TEXT,
  authorLastName TEXT,
  authorStreet TEXT,
  authorSuburb TEXT,
  authorCity TEXT
);
```


## Book
```
CREATE TABLE Book
(
  bookCode INTEGER PRIMARY KEY NOT NULL,
  bookTitle TEXT,
  bookType TEXT,
  paperback TEXT CHECK (paperback IN ('Y','N'))
);
```


## BookPrice
```
CREATE TABLE BookPrice
(
  bookCode INTEGER NOT NULL,
  startDate DATE NOT NULL,
  endDate DATE,
  price REAL,
  PRIMARY KEY (bookCode, startDate),
  FOREIGN KEY (bookCode) REFERENCES Book (bookCode)
  ON UPDATE NO ACTION ON DELETE NO ACTION
);
```


## Writing
```
CREATE TABLE Writing
(
  bookCode INTEGER NOT NULL,
  authorNo INTEGER NOT NULL,
  pubCode INTEGER,
  edition INTEGER,
  pubDate DATE,
  PRIMARY KEY (bookCode, authorNo),
  FOREIGN KEY (bookCode) REFERENCES Book (bookCode)
  ON UPDATE NO ACTION ON DELETE NO ACTION,
  FOREIGN KEY (authorNo) REFERENCES Author (authorNo)
  ON UPDATE NO ACTION ON DELETE NO ACTION,
  FOREIGN KEY (pubCode) REFERENCES Publisher (pubCode)
  ON UPDATE NO ACTION ON DELETE NO ACTION
);

```


## TransactionType
```
CREATE TABLE TransactionType
(
  transactionTypeID INTEGER PRIMARY KEY NOT NULL,
  transactionType TEXT
);
```


## Inventory
```
CREATE TABLE Inventory
(
  transactionNo INTEGER PRIMARY KEY NOT NULL,
  transactionDate DATE,
  bookCode INTEGER,
  branchNo INTEGER,
  quantity INTEGER,
  transactionTypeID INTEGER,
  FOREIGN KEY (bookCode) REFERENCES Book (bookCode)
  ON UPDATE NO ACTION ON DELETE NO ACTION,
  FOREIGN KEY (branchNo) REFERENCES Branch (branchNo)
  ON UPDATE NO ACTION ON DELETE NO ACTION,
  FOREIGN KEY (transactionTypeID) REFERENCES TransactionType (transactionTypeID)
  ON UPDATE NO ACTION ON DELETE NO ACTION
);
```


## BookGrade
```
CREATE TABLE BookGrade
(
  bookGrade TEXT PRIMARY KEY NOT NULL,
  minValue REAL,
  maxValue REAL
);
```


## Role
```
CREATE TABLE Role
(
  roleID INTEGER PRIMARY KEY NOT NULL,
  role TEXT
);
```


## Staff
```
CREATE TABLE Staff
(
  staffCode INTEGER PRIMARY KEY NOT NULL,
  staffFirstName TEXT,
  staffLastName TEXT,
  staffStreet TEXT,
  staffSuburb TEXT,
  staffCity TEXT
);
```


## StaffAssignment
```
CREATE TABLE StaffAssignment
(
  assignmentID INTEGER PRIMARY KEY NOT NULL,
  roleID INTEGER,
  branchNo INTEGER,
  staffCode INTEGER,
  startDate DATE,
  endDate DATE,
  salary REAL,
  FOREIGN KEY (roleID) REFERENCES Role (roleID)
  ON UPDATE NO ACTION ON DELETE NO ACTION,
  FOREIGN KEY (branchNo) REFERENCES Branch (branchNo)
  ON UPDATE NO ACTION ON DELETE NO ACTION,
  FOREIGN KEY (staffCode) REFERENCES Staff (staffCode)
  ON UPDATE NO ACTION ON DELETE NO ACTION
);
```
<!-- .element: style="font-size:95%" -->



## Q1: Stock of books
- Write a single SQL statement to project two columns for a list: the title of a book in the first column named Title, and the total stock of that book in the second column named Stock. All books from the database must be included in the list

- Hint: You are told that transactionTypeID 1 means sold and 2 means received


## Sample output

```
Title                      Stock     
-------------------------  ----------
Far from the Crowd         100       
A Loud Game                110       
The Artist                 144       
Passage to Freedom         0         
Tornado                    530       
Knockdown                  0         
Judo                       694       
Sneaky Stories             0         
Pilgrim's Progress         0         
```



## Q2: Salary distribution
- Write a single SQL statement to generate two rows of information: one row showing the combined salary of all managers, and the other row showing the combined salary of all other employees. There should be three columns in the output. First column is named Type which has the value Manager or non-Manager. Second column is named Salary which shows the total salary as an integer with a dollar sign. Third column is named Percentage which shows the percentage of the total salary in 2 decimal places with a percentage sign


## Sample output
```
Type             Salary           Percentage     
---------------  ---------------  ---------------
Manager          $235000          39.97%         
Non-Manager      $353000          60.03%         
```



## Q3: Grade of books
- Write a single SQL statement to project two columns for a list: the book grade in the first column named Grade, and the second column named Books showing the full name of all books belonging to that grade based on the latest price. The full name of a book composes of the book title followed by the published year in a bracket. Each book is separated by a comma with a space among each other


## Sample output
```
Grade            Books                                                                 
---------------  ----------------------------------------------------------------------
Very Low         Knockdown (1972)                                                      
Low              Passage to Freedom (2003)                                             
Medium           Far from the Crowd (2006), Tornado (2007), Judo (1985)                
High             A Loud Game (2004)                                                    
Very High        The Artist (2000)                                                     
```
<!-- .element: style="font-size:80%" -->



## Q4: View of status
- Write a single SQL statement to create a view named StaffStatus with three columns: first column named Staff which shows the first name of staff; second column named Status which shows the value Active or Inactive reflecting the employment status of staff; and third column named Duration which shows the number of years and the number of months each staff has worked for. The output of the view should be ordered first by Status accendingly and then by Duration decendingly

- Hint: You are told that on average there are 365.25 days in a year and 30.44 days in a month


## Sample output
```
Staff            Status           Duration                 
---------------  ---------------  -------------------------
Deepak           Active           10 Year(s) 10 Month(s)   
Frances          Active           10 Year(s) 10 Month(s)   
Karen            Active           10 Year(s) 9 Month(s)    
Stephen          Active           10 Year(s) 9 Month(s)    
James            Active           10 Year(s) 9 Month(s)    
Viana            Active           10 Year(s) 8 Month(s)    
Brendan          Active           10 Year(s) 8 Month(s)    
Martin           Active           10 Year(s) 8 Month(s)    
Sherene          Active           10 Year(s) 5 Month(s)    
Charles          Inactive         8 Year(s) 1 Month(s)     
Sean             Inactive         2 Year(s) 9 Month(s)     
Ethel            Inactive         1 Year(s) 4 Month(s)     
```



## Q5: Undelete and assign
- Write two SQL statements to create two triggers. First trigger is named UndeleteStaff and its purpose is to reverse the deletion of a staff after it happens. It assigns the value of current timestamp to the end date of that staff's assignment. Second trigger is named AssignStaff and its purpose is to add a new assignment and assign the value of current timestamp to the start date after any insertion of new staff happens. In other words, it will not be activated if no new staff is being inserted



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
