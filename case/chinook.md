# <i class="fas fa-database"></i> Case study
### Chinook
[<i class="fab fa-creative-commons"></i>](https://creativecommons.org/licenses/by/4.0/) [Johnny Chan](mailto:jh.chan@auckland.ac.nz) | <i class="fas fa-calendar"></i> 2018-09-17



## Background
- [Chinook](https://github.com/lerocha/chinook-database) is a sample database being commonly used for demos and testing purposes on multiple products including SQLite. It is an open source project

- The database can be created by running a single [SQL script](https://johnny723.github.io/infosys222/case/chinook.sql)

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



## Q01: Magic seventeen (0.5%)
- Write a single SQL statement to list all the artists that have an ArtistID divisible by 17. The output should include all columns from the Artist table


## Sample output
```
ArtistID    Name                                    
----------  ----------------------------------------
17          Chico Buarque                           
34          Nando Reis                              
51          Queen                                   
68          Miles Davis                             
85          Frank Sinatra                           
102         Marillion                               
119         Peter Tosh                              
136         Terry Bozzio, Tony Levin & Steve Stevens
153         Velvet Revolver                         
170         Jack Johnson                            
187         Los Hermanos                            
204         Temple of the Dog                       
221         Sir Georg Solti & Wiener Philharmoniker
238         Maurizio Pollini                        
255         Yehudi Menuhin                          
272         Emerson String Quartet                  
```
<!-- .element: style="font-size:85%" -->



## Q02: Fix the contact (0.5%)
- You are told that one employee has a phone number and a fax number stored with a missing + sign at the front, but you do not know which employee has done that. All other employees have their contact numbers starting with the + sign

- Write a single SQL statement (without using subquery) to update that particular phone number and fax number by adding a + sign at the front



## Q03: Log me in (0.5%)
- Write a single SQL statement to create a new table named Login with five columns: Username (Text), Password (Text), LastUpdate (Date), Status (Text) and CustomerID (Integer)

  - Assign Username as the primary key of the table
  - The Password should never be NULL
  - Set the default value of LastUpdate to the current date and time (NZ time)
  - Define a check on Status to make sure the value could either be 'active' or 'inactive'
  - Assign CustomerID as a foreign key referencing the Customer table



## Q04: Find the exact time (0.5%)
- Write a single SQL statement to list all the tracks that have the exact word 'time' (including both upper and lower cases) as part of the name in the Track table. In other words, it will not include track names with words like 'wartime' or 'times' in the output


## Sample output
```
Name                                              
--------------------------------------------------
Your Time Has Come                                
First Time I Met The Blues                        
My Time After Awhile                              
In My Time Of Dying                               
Now's The Time                                    
Time After Time                                   
Night Time Is The Right Time                      
This Time Around / Owed to 'G' Instrumental       
Child In Time                                     
Child In Time (Son Of Aleric - Instrumental)      
Time To Kill                                      
In Time                                           
Caught Somewhere in Time                          
Stillness In Time                                 
Every Time I Look At You                          
Your Time Is Gonna Come                           
You've Been A Long Time Coming                    
Killing Time                                      
Time                                              
Time                                              
Right On Time                                     
Wiser Time                                        
Time Is On My Side                                
The Last Time                                     
The First Time                                    
Your Time Is Gonna Come                               
```



## Q05: Album discount (0.5%)
- Write a single SQL statement to list albums with four columns: the title of the album, the total number of tracks in that album, the price of the album, and the discounted price of the album. All albums priced above forty dollars would be given a ten percent discount. All other albums would be given a discounted price of 'N/A' in the output.

- Exclude albums that are priced lower than thirty dollars from the output. Rename the column headings appropriately, and sort the output by price descendingly


## Sample output
```
Album                           Tracks    Price       Discounted Price
------------------------------  --------  ----------  ----------------
Greatest Hits                   57        56.43       50.787          
Lost, Season 3                  26        51.74       46.566          
Lost, Season 1                  25        49.75       44.775          
The Office, Season 3            25        49.75       44.775          
Battlestar Galactica (Classic)  24        47.76       42.984          
Lost, Season 2                  24        47.76       42.984          
Heroes, Season 1                23        45.77       41.193          
The Office, Season 2            22        43.78       39.402          
Battlestar Galactica, Season 3  19        37.81       N/A             
LOST, Season 4                  17        33.83       N/A             
Minha Historia                  34        33.66       N/A    
```
<!-- .element: style="font-size:85%" -->



## Q06: Oldest manager (0.5%)
- Write a single SQL statement to show the first name, the total year of employment, and the age of an employee who is the oldest manager in the company. The total year of employment and age should be shown as a whole number (i.e. without any decimal places) in the output


## Sample output
```
Name             Year        Age       
---------------  ----------  ----------
Nancy            15          58        
```



## Q07: Background music (1%)
- Write a single SQL statement to create a new playlist called 'Background Music'; and write another single SQL statement to associate the new playlist with the 10 longest duration tracks from the Track table



## Q08: DRM (1%)
- Write a single SQL statement to generate two rows of information: one row showing the number of tracks formatted in protected media type, and the other row showing the number of tracks formatted in non-protected media type. There should be two columns in the output: first column is named Media which has the value Protected or non-Protected; second column is named Tracks which shows the total number of tracks


## Sample output
```
Media            Tracks         
---------------  ---------------
Protected        451            
non-Protected    3052           
```



## Q09: Popular album (1%)
- Write a single SQL statement to project two columns for a list: the name of an album in the first column, and the names of all playlists that the album is associated with in the second column. Only include albums that associate with at least five playlists, and rename the column headings appropriately


## Sample output
```
Album                                                                        Playlist                                          
---------------------------------------------------------------------------  --------------------------------------------------
A Soprano Inspired                                                           Music 1,90’s Music,Music 2,Classical,Classical 1
Adorate Deum: Gregorian Chant from the Proper of the Mass                    Music 1,90’s Music,Music 2,Classical,Classical 1
Allegri: Miserere                                                            Music 1,90’s Music,Music 2,Classical,Classical 1
Bach: Goldberg Variations                                                    Music 1,90’s Music,Music 2,Classical,Classical 1
Bach: Orchestral Suites Nos. 1 - 4                                           Music 1,90’s Music,Music 2,Classical,Classical 1
Bach: The Cello Suites                                                       Music 1,90’s Music,Music 2,Classical,Classical 1
Bach: Toccata & Fugue in D Minor                                             Music 1,90’s Music,Music 2,Classical,Classical 1
Beethoven Piano Sonatas: Moonlight & Pastorale                               Music 1,90’s Music,Music 2,Classical,Classical 1
Beethoven: Symhonies Nos. 5 & 6                                              Music 1,90’s Music,Music 2,Classical,Classical 1
Berlioz: Symphonie Fantastique                                               Music 1,90’s Music,Music 2,Classical,Classical 1
Bizet: Carmen Highlights                                                     Music 1,90’s Music,Music 2,Classical,Classical 1
Carmina Burana                                                               Music 1,90’s Music,Music 2,Classical,Classical 1
Chopin: Piano Concertos Nos. 1 & 2                                           Music 1,90’s Music,Music 2,Classical,Classical 1
English Renaissance                                                          Music 1,90’s Music,Music 2,Classical,Classical 1
Great Opera Choruses                                                         Music 1,90’s Music,Music 2,Classical,Classical 1
Great Recordings of the Century - Mahler: Das Lied von der Erde              Music 1,90’s Music,Music 2,Classical,Classical 1
Górecki: Symphony No. 3                                                     Music 1,90’s Music,Music 2,Classical,Classical 1
Handel: Music for the Royal Fireworks (Original Version 1749)                Music 1,90’s Music,Music 2,Classical,Classical 1
Handel: The Messiah (Highlights)                                             Music 1,90’s Music,Music 2,Classical,Classical 1
Holst: The Planets, Op. 32 & Vaughan Williams: Fantasies                     Music 1,90’s Music,Music 2,Classical,Classical 1
J.S. Bach: Chaconne, Suite in E Minor, Partita in E Major & Prelude, Fugue   Music 1,90’s Music,Music 2,Classical,Classical 1
Koyaanisqatsi (Soundtrack from the Motion Picture)                           Music 1,90’s Music,Music 2,Classical,Classical 1
Locatelli: Concertos for Violin, Strings and Continuo, Vol. 3                Music 1,90’s Music,Music 2,Classical,Classical 1
Mascagni: Cavalleria Rusticana                                               Music 1,90’s Music,Music 2,Classical,Classical 1
Mendelssohn: A Midsummer Night's Dream                                       Music 1,90’s Music,Music 2,Classical,Classical 1
Mozart Gala: Famous Arias                                                    Music 1,90’s Music,Music 2,Classical,Classical 1
Mozart: Symphonies Nos. 40 & 41                                              Music 1,90’s Music,Music 2,Classical,Classical 1
Pavarotti's Opera Made Easy                                                  Music 1,90’s Music,Music 2,Classical,Classical 1
Prokofiev: Romeo & Juliet                                                    Music 1,90’s Music,Music 2,Classical,Classical 1
Respighi:Pines of Rome                                                       Music 1,90’s Music,Music 2,Classical,Classical 1
Scheherazade                                                                 Music 1,90’s Music,Music 2,Classical,Classical 1
Sibelius: Finlandia                                                          Music 1,90’s Music,Music 2,Classical,Classical 1
Strauss: Waltzes                                                             Music 1,90’s Music,Music 2,Classical,Classical 1
Szymanowski: Piano Works, Vol. 1                                             Music 1,90’s Music,Music 2,Classical,Classical 1
Tchaikovsky: 1812 Festival Overture, Op.49, Capriccio Italien & Beethoven:   Music 1,90’s Music,Music 2,Classical,Classical 1
The Last Night of the Proms                                                  Music 1,90’s Music,Music 2,Classical,Classical 1
The World of Classical Favourites                                            Music 1,90’s Music,Music 2,Classical,Classical 1
Wagner: Favourite Overtures                                                  Music 1,90’s Music,Music 2,Classical,Classical 1
Weill: The Seven Deadly Sins                                                 Music 1,90’s Music,Music 2,Classical,Classical 1     
```
<!-- .element: style="font-size:50%" -->



## Q10: Email provider (1%)

- Write a single SQL statement to generate a list with two columns: Provider and Percentage
  - The first column displays email provider in upper cases (e.g. GMAIL, YAHOO), and the information could be obtained from the email of customer. Email from the same provider with different country code (e.g. yahoo.com, yahoo.de, yahoo.ca) should be considered as part of the same email provider (e.g. YAHOO)
  - The second column displays the percentage of customer with two decimal places
  - Only include providers with a percentage of more than 5 in the output
  - Sort the output by Percentage descendingly


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



## Q11: View the customer (1%)

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



## Q12 (Bonus): U2 forever (1%)

- Write a single SQL statement to create a trigger named UndeleteU2Track. The objective of this trigger is to cancel the effect of deleting any track from the Track table that associates with an album where the artist is U2.
  - The trigger does not stop the deletion; but it cancels its effect by recreating the exact same row or rows of deleted track or tracks
  - The trigger ignores deletion of track that has no association with U2



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
