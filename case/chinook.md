# <i class="fas fa-database"></i> Case study
### Chinook
[<i class="fab fa-creative-commons"></i>](https://creativecommons.org/licenses/by/4.0/) [Johnny Chan](mailto:jh.chan@auckland.ac.nz) | <i class="fas fa-calendar"></i> 2019-09-23



## Background
- [Chinook](https://github.com/lerocha/chinook-database) is a sample database being commonly used for demos and testing purposes on multiple products including SQLite. It is an open source project

- The database can be created by running a single [SQL script](https://noct15.github.io/infosys222/case/chinook.sql)

- The company Chinook runs a business of selling tracks and albums of music and video online. The database manages all the transactions, and all relevant information related to the products and customers



## Specification
- The following specification captures the Chinook database schema through all the CREATE TABLE statements that build up the database:

  - Album
  - Artist
  - Customer
  - Employee
  - Genre
  - Invoice
  - InvoiceLine
  - MediaType
  - PlayList
  - PlayListTrack
  - Track


## Album
```
CREATE TABLE Album
(
    AlbumID INTEGER PRIMARY KEY NOT NULL,
    Title TEXT NOT NULL,
    ArtistID INTEGER NOT NULL,
    FOREIGN KEY (ArtistID) REFERENCES Artist (ArtistID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```


## Artist
```
CREATE TABLE Artist
(
    ArtistID INTEGER PRIMARY KEY NOT NULL,
    Name TEXT
);
```


## Customer
```
CREATE TABLE Customer
(
    CustomerID INTEGER PRIMARY KEY NOT NULL,
    FirstName TEXT NOT NULL,
    LastName TEXT NOT NULL,
    Company TEXT,
    Address TEXT,
    City TEXT,
    State TEXT,
    Country TEXT,
    PostalCode TEXT,
    Phone TEXT,
    Fax TEXT,
    Email TEXT NOT NULL,
    SupportRepID INTEGER,
    FOREIGN KEY (SupportRepID) REFERENCES Employee (EmployeeID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```
<!-- .element: style="font-size:85%" -->


## Employee
```
CREATE TABLE Employee
(
    EmployeeID INTEGER PRIMARY KEY NOT NULL,
    LastName TEXT NOT NULL,
    FirstName TEXT NOT NULL,
    Title TEXT,
    ReportsTo INTEGER,
    BirthDate DATE,
    HireDate DATE,
    Address TEXT,
    City TEXT,
    State TEXT,
    Country TEXT,
    PostalCode TEXT,
    Phone TEXT,
    Fax TEXT,
    Email TEXT,
    FOREIGN KEY (ReportsTo) REFERENCES Employee (EmployeeID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```
<!-- .element: style="font-size:75%" -->


## Genre
```
CREATE TABLE Genre
(
    GenreID INTEGER PRIMARY KEY NOT NULL,
    Name TEXT
);
```


## Invoice
```
CREATE TABLE Invoice
(
    InvoiceID INTEGER PRIMARY KEY NOT NULL,
    CustomerID INTEGER NOT NULL,
    InvoiceDate DATE NOT NULL,
    BillingAddress TEXT,
    BillingCity TEXT,
    BillingState TEXT,
    BillingCountry TEXT,
    BillingPostalCode TEXT,
    Total REAL NOT NULL,
    FOREIGN KEY (CustomerID) REFERENCES Customer (CustomerID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```
<!-- .element: style="font-size:95%" -->


## InvoiceLine
```
CREATE TABLE InvoiceLine
(
    InvoiceLineID INTEGER PRIMARY KEY NOT NULL,
    InvoiceID INTEGER NOT NULL,
    TrackID INTEGER NOT NULL,
    UnitPrice REAL NOT NULL,
    Quantity INTEGER NOT NULL,
    FOREIGN KEY (InvoiceID) REFERENCES Invoice (InvoiceID)
		ON DELETE NO ACTION ON UPDATE NO ACTION,
    FOREIGN KEY (TrackID) REFERENCES Track (TrackID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```


## MediaType
```
CREATE TABLE MediaType
(
    MediaTypeID INTEGER PRIMARY KEY NOT NULL,
    Name TEXT
);
```


## PlayList
```
CREATE TABLE Playlist
(
    PlaylistID INTEGER PRIMARY KEY NOT NULL,
    Name TEXT
);
```


## PlayListTrack
```
CREATE TABLE PlaylistTrack
(
    PlaylistID INTEGER NOT NULL,
    TrackID INTEGER NOT NULL,
    PRIMARY KEY (PlaylistID, TrackID),
    FOREIGN KEY (PlaylistID) REFERENCES Playlist (PlaylistID)
		ON DELETE NO ACTION ON UPDATE NO ACTION,
    FOREIGN KEY (TrackID) REFERENCES Track (TrackID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```
<!-- .element: style="font-size:95%" -->


## Track
```
CREATE TABLE Track
(
    TrackID INTEGER PRIMARY KEY NOT NULL,
    Name TEXT NOT NULL,
    AlbumID INTEGER,
    MediaTypeID INTEGER NOT NULL,
    GenreID INTEGER,
    Composer TEXT,
    Millisecond INTEGER NOT NULL,
    Byte INTEGER,
    UnitPrice REAL NOT NULL,
    FOREIGN KEY (AlbumID) REFERENCES Album (AlbumID)
		ON DELETE NO ACTION ON UPDATE NO ACTION,
    FOREIGN KEY (GenreID) REFERENCES Genre (GenreID)
		ON DELETE NO ACTION ON UPDATE NO ACTION,
    FOREIGN KEY (MediaTypeID) REFERENCES MediaType (MediaTypeID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```
<!-- .element: style="font-size:85%" -->



## Q01: Who am I (1%)
- Write a single SELECT statement to project your first name, last name, student ID and a short description of yourself as four string literals. Rename the column headings appropriately


## Sample output

```
First Name  Last Name   ID          Description
----------  ----------  ----------  -------------
Johnny      Chan        9999999     I am the king
```



## Q02: Who the F (1%)
- Write a single SQL statement to list all the artists that begin their name with the letter F (including both upper and lower cases). The output should include all columns from the Artist table


## Sample output
```
ArtistID    Name
----------  -------------------------------
23          Frank Zappa & Captain Beefheart
39          Fernanda Porto
82          Faith No More
83          Falamansa
84          Foo Fighters
85          Frank Sinatra
86          Funk Como Le Gusta
241         Felix Schmidt, London Symphony
251         Fretwork
```



## Q03: Magic twenty-three (1%)
- Write a single SQL statement to list all the albums that have an AlbumID divisible by 23. The output should include all columns from the Album table


## Sample output
```
AlbumID     Title                                     ArtistID
----------  ----------------------------------------  ----------
23          Minha Historia                            17
46          Supernatural                              59
69          Djavan Ao Vivo - Vol. 02                  80
92          Use Your Illusion II                      88
115         Sex Machine                               91
138         The Song Remains The Same (Disc 2)        22
161         Demorou...                                108
184         Os Caes Ladram Mas A Caravana Nao Pára    121
207         Mezmerize                                 135
230         Lost, Season 1                            149
253         Battlestar Galactica (Classic), Season 1  158
276         Bach: Violin Concertos                    210
299         Scheherazade                              233
322         Frank                                     252
345         Monteverdi: L'Orfeo                       273
```
<!-- .element: style="font-size:85%" -->



## Q04: Order by name (1%)
- Write a single SQL statement to list all employee names (each combining the last name and the first name with a comma as separator) from the Employee table. Sort the output by the length of the employee name, with the shortest one to go first in the list


## Sample output
```
Name
--------------------
King, Robert
Adams, Andrew
Peacock, Jane
Edwards, Nancy
Park, Margaret
Johnson, Steve
Callahan, Laura
Mitchell, Michael
```



## Q05: Fix the sign (1%)
- You are told that one employee has a phone number and a fax number stored with a missing + sign at the front, but you do not know which employee has done that. All other employees have their contact numbers starting with the + sign

- Write a single SQL statement (without using subquery) to update that particular phone number and fax number by adding a + sign at the front



## Q06: Log me in (1%)
- Write a single SQL statement to create a new table named Login with five columns: Username (Text), Password (Text), LastUpdate (Date), Status (Text) and CustomerID (Integer)

  - Assign Username as the primary key of the table
  - The Password should never be NULL
  - Set the default value of LastUpdate to the current date and time (NZ time)
  - Define a check on Status to make sure the value could either be 'active' or 'inactive'
  - Assign CustomerID as a foreign key referencing the Customer table



## Q07: Find the right song (1%)
- Write a single SQL statement to list all the tracks that have the exact word 'right' (including both upper and lower cases) as part of the name in the Track table. In other words, it will not include track names with words like 'rights' or 'righteous' in the output


## Sample output
```
Name
--------------------------------------------------
Right Through You
Night Time Is The Right Time
You Can't Do it Right (With the One You Love)
Right Next Door to Hell
Always Be All Right
Get Right
Right On Time
The Right Thing
Right Now
You Got No Right
I Guess You're Right
```



## Q08: Find the wrong time (1%)
- Write a single SQL statement to list all the tracks that do not have the exact word 'time' (including both upper and lower cases) as part of the name in the Track table, but they have 'time' being part of a word in the name. For instance, it should include track names with words like 'wartime' or 'times' in the output


## Sample output
```
Name
--------------------------------------------------
How Many More Times
Playtime
Summertime
Overtime
Sometimes I Feel Like Screaming
Times Like These
Good Times Bad Times
How Many More Times
Life During Wartime
Sometimes Salvation
Luminous Times (Hold On To Love)
Sometimes You Can't Make It On Your Own
Where Have All The Good Times Gone?
Times of Trouble
Jewel of the Summertime
```



## Q09: Album luxury (1%)
- Write a single SQL statement to list albums with three columns: the title of the album, the total number of tracks in that album, and the price of the album. Exclude albums that are priced lower than thirty dollars from the output. Rename the column headings appropriately, and sort the output by the price in descending order


## Sample output
```
Album                                               Tracks      Price
--------------------------------------------------  ----------  ----------
Greatest Hits                                       57          56.43
Lost, Season 3                                      26          51.74
Lost, Season 1                                      25          49.75
The Office, Season 3                                25          49.75
Battlestar Galactica (Classic), Season 1            24          47.76
Lost, Season 2                                      24          47.76
Heroes, Season 1                                    23          45.77
The Office, Season 2                                22          43.78
Battlestar Galactica, Season 3                      19          37.81
LOST, Season 4                                      17          33.83
Minha Historia                                      34          33.66
```
<!-- .element: style="font-size:85%" -->



## Q10: Youngest manager (1%)
- Write a single SQL statement to show the first name and the age of an employee who is the youngest manager in the company. The age should be shown as a whole number (i.e. without any decimal places) in the output


## Sample output
```
Name             Age
---------------  ---------------
Michael          46
```



## Q11: Background music (1%)
- Write a single SQL statement to create a new playlist called 'Background music'; and write another single SQL statement to associate the new playlist with the 10 longest duration tracks of jazz music from the Track table



## Q12: AAC (1%)
- Write a single SQL statement to generate three rows of information: one row showing the number of tracks formatted in AAC media type, one row showing the number of tracks formatted in non-AAC media type, and one row showing the total number of tracks in the database. There should be two columns in the output: first column is named Media which has the value AAC or non-AAC; second column is named Tracks which shows the total number of tracks


## Sample output
```
Media       Tracks
----------  ----------
AAC         255
non-AAC     3248
Total       3503
```



## Q13: Multi genre (1%)
- Write a single SQL statement to project two columns for a list: the title of an album in the first column, and the names of genre that the album is associated with in the second column. Exclude albums that associate with only one genre, and rename the column headings appropriately


## Sample output
```
Album                           Genre
------------------------------  -----------------------------------------
Battlestar Galactica, Season 3  TV Shows,Science Fiction,Sci Fi & Fantasy
Greatest Hits                   Rock,Reggae,Metal
Heroes, Season 1                TV Shows,Drama
LOST, Season 4                  Drama,TV Shows
Live After Death                Heavy Metal,Metal
Lost, Season 2                  TV Shows,Drama
Lost, Season 3                  TV Shows,Drama
Rock In Rio CD2                 Rock,Metal
The Number of The Beast         Metal,Rock
The Office, Season 3            TV Shows,Comedy
Unplugged                       Blues,Latin
```
<!-- .element: style="font-size:90%" -->



## Q14: Email provider (1%)

- Write a single SQL statement to generate a list with two columns: Provider and Percentage
  - The first column displays email provider in upper cases (e.g. GMAIL, YAHOO), and the information could be obtained from the email of customer. Email from the same provider with different country code (e.g. yahoo.com, yahoo.de, yahoo.ca) should be considered as part of the same email provider (e.g. YAHOO)
  - The second column displays the percentage of customer with two decimal places
  - Only include providers with a percentage of more than 5 in the output
  - Sort the output by Percentage in descending order


## Sample output
```
Provider              Percentage     
--------------------  ---------------
YAHOO                 30.51          
GMAIL                 13.56          
APPLE                 11.86          
HOTMAIL               6.78           
SHAW                  5.08           
```



## Q15: View the customer (1%)

- Write a single SQL statement to create a view named CustomerView. The view should have three columns: Country, Individual and Company
  - The first column includes all the countries found in the Customer table
  - The second column and the third column display the number of individual and company customers in each country respectively
  - Both second and third columns display only whole number
  - A company customer is defined by the presence of value in the column Company of the Customer table; an individual customer is defined by the absence of value in that same column
  - Sort the output by country ascendingly
  - You cannot use OUTER JOIN for this particular task


## Sample output
```
SELECT * FROM CustomerView;

Country          Individual       Company        
---------------  ---------------  ---------------
Argentina        1                0              
Australia        1                0              
Austria          1                0              
Belgium          1                0              
Brazil           1                4              
Canada           6                2              
Chile            1                0              
Czech Republic   1                1              
Denmark          1                0              
Finland          1                0              
France           5                0              
Germany          4                0              
Hungary          1                0              
India            2                0              
Ireland          1                0              
Italy            1                0              
Netherlands      1                0              
Norway           1                0              
Poland           1                0              
Portugal         2                0              
Spain            1                0              
Sweden           1                0              
USA              10               3              
United Kingdom   3                0              
```



## Q16: We are so lost (Bonus 1%)
- Write a single SQL statement to create a trigger named UndeleteLostTrack. The objective of this trigger is to cancel the effect of deleting any track from the Track table that associates with LOST the TV show (which could be referenced from the title of an album)
  - The trigger does not stop the deletion; but it cancels its effect by recreating the exact same row or rows of deleted track or tracks
  - The trigger ignores deletion of track that has no association with LOST



## Q17: Average employee (Bonus 1%)
- Write a single SQL statement to show the first name and the total year of employment of an employee who has been hired closest to the average total year of employment among all employees in the company


## Sample output
```
Name        Year
----------  ----------------
Margaret    16.3581573031779
```



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

Chinook is awesome in <span class="country">everywhere</span>!

[<i class="fas fa-print"></i>](?print-pdf#)
