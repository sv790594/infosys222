# <i class="fas fa-database"></i> Case study
### Northwind
[<i class="fab fa-creative-commons"></i>](https://creativecommons.org/licenses/by/4.0/) [Johnny Chan](mailto:jh.chan@auckland.ac.nz) | <i class="fas fa-calendar"></i> 2022-05-02



## Background
- Northwind is a sample database being commonly used for demos and testing purposes. It contains some business records of a trading company dated from 1996 to 1998

- The database can be created by running a single [SQL script](nw.sql)



## Specification
- The following specification captures the Northwind database schema through all the CREATE TABLE statements that build up the database:

  - Supplier
  - Category
  - Product
  - Customer
  - Employee
  - Region
  - Territory
  - EmployeeTerritory
  - Shipper
  - Order
  - OrderDetail


## Supplier
```
CREATE TABLE Supplier(
   SupplierID INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
   CompanyName TEXT NOT NULL,
   ContactName TEXT,
   ContactTitle TEXT,
   Address TEXT,
   City TEXT,
   Region TEXT,
   PostalCode TEXT,
   Country TEXT,
   Phone TEXT,
   Fax TEXT,
   HomePage TEXT
);
```


## Category
```
CREATE TABLE Category(      
  CategoryID INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  CategoryName TEXT,
  Description TEXT
);
```


## Product
```
CREATE TABLE Product(
   ProductID INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
   ProductName TEXT NOT NULL,
   SupplierID INTEGER,
   CategoryID INTEGER,
   QuantityPerUnit TEXT,
   UnitPrice REAL DEFAULT 0,
   UnitsInStock INTEGER DEFAULT 0,
   UnitsOnOrder INTEGER DEFAULT 0,
   ReorderLevel INTEGER DEFAULT 0,
   Discontinued INTEGER NOT NULL DEFAULT 0,
   CHECK (UnitPrice>=0),
   CHECK (ReorderLevel>=0),
   CHECK (UnitsInStock>=0),
   CHECK (UnitsOnOrder>=0),
   FOREIGN KEY (CategoryID) REFERENCES Category (CategoryID)
		ON DELETE NO ACTION ON UPDATE NO ACTION,
   FOREIGN KEY (SupplierID) REFERENCES Supplier (SupplierID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```
<!-- .element: style="font-size:75%" -->


## Customer
```
CREATE TABLE Customer
(      CustomerID TEXT NOT NULL,
       CompanyName TEXT,
       ContactName TEXT,
       ContactTitle TEXT,
       Address TEXT,
       City TEXT,
       Region TEXT,
       PostalCode TEXT,
       Country TEXT,
       Phone TEXT,
       Fax TEXT,
       PRIMARY KEY (CustomerID)
);
```
<!-- .element: style="font-size:100%" -->


## Employee
```
CREATE TABLE Employee(      
  EmployeeID INTEGER PRIMARY KEY AUTOINCREMENT,
  LastName TEXT,
  FirstName TEXT,
  Title TEXT,
  TitleOfCourtesy TEXT,
  BirthDate DATE,
  HireDate DATE,
  Address TEXT,
  City TEXT,
  Region TEXT,
  PostalCode TEXT,
  Country TEXT,
  HomePhone TEXT,
  Extension TEXT,
  Note TEXT,
  ReportsTo INTEGER,
  PhotoPath TEXT,
  FOREIGN KEY (ReportsTo) REFERENCES Employee (EmployeeID)
	 ON DELETE NO ACTION ON UPDATE NO ACTION
);
```
<!-- .element: style="font-size:70%" -->


## Region
```
CREATE TABLE Region(
   RegionID INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
   RegionDescription TEXT NOT NULL
);
```
<!-- .element: style="font-size:100%" -->


## Territory
```
CREATE TABLE Territory(
   TerritoryID TEXT NOT NULL,
   TerritoryDescription TEXT NOT NULL,
   RegionID INTEGER NOT NULL,
   PRIMARY KEY (TerritoryID),
   FOREIGN KEY (RegionID) REFERENCES Region (RegionID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```


## EmployeeTerritory
```
CREATE TABLE EmployeeTerritory(
   EmployeeID INTEGER NOT NULL,
   TerritoryID TEXT NOT NULL,
   PRIMARY KEY (EmployeeID, TerritoryID),
   FOREIGN KEY (EmployeeID) REFERENCES Employee (EmployeeID)
		ON DELETE NO ACTION ON UPDATE NO ACTION,
   FOREIGN KEY (TerritoryID) REFERENCES Territory (TerritoryID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```


## Shipper
```
CREATE TABLE Shipper(
   ShipperID INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
   CompanyName TEXT NOT NULL,
   Phone TEXT
);
```


## Order
```
CREATE TABLE 'Order'(
   OrderID INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
   CustomerID TEXT,
   EmployeeID INTEGER,
   OrderDate DATETIME,
   RequiredDate DATETIME,
   ShippedDate DATETIME,
   ShipVia INTEGER,
   Freight NUMERIC DEFAULT 0,
   ShipName TEXT,
   ShipAddress TEXT,
   ShipCity TEXT,
   ShipRegion TEXT,
   ShipPostalCode TEXT,
   ShipCountry TEXT,
   FOREIGN KEY (EmployeeID) REFERENCES Employee (EmployeeID)
		ON DELETE NO ACTION ON UPDATE NO ACTION,
   FOREIGN KEY (CustomerID) REFERENCES Customer (CustomerID)
		ON DELETE NO ACTION ON UPDATE NO ACTION,
   FOREIGN KEY (ShipVia) REFERENCES Shipper (ShipperID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```
<!-- .element: style="font-size:70%" -->


## OrderDetail
```
CREATE TABLE OrderDetail(
   OrderID INTEGER NOT NULL,
   ProductID INTEGER NOT NULL,
   UnitPrice REAL NOT NULL DEFAULT 0,
   Quantity INTEGER NOT NULL DEFAULT 1,
   Discount REAL NOT NULL DEFAULT 0,
   PRIMARY KEY (OrderID, ProductID),
   CHECK (Discount>=0 AND Discount<=1),
   CHECK (Quantity>0),
   CHECK (UnitPrice>=0),
   FOREIGN KEY (OrderID) REFERENCES 'Order' (OrderID)
		ON DELETE NO ACTION ON UPDATE NO ACTION,
   FOREIGN KEY (ProductID) REFERENCES Product (ProductID)
		ON DELETE NO ACTION ON UPDATE NO ACTION
);
```
<!-- .element: style="font-size:100%" -->



## Q01: Who am I (0.2%)
- Use the comment feature of SQL to print your full name, student ID and email from the university in three separate lines at the top of the script file. From this point onwards, you should use the comment feature to separate your answers for each question



## Q02: What are we selling (0.2%)
- Write a single SQL statement to list all the rows from the Product table that are not currently discontinued. Include all columns except the Discontinued column. Rename the columns with proper spacing using the alias feature. For example, the UnitPrice column should be renamed as Unit Price


## Sample output
```
Product ID  Product Name                      Supplier ID  Category ID  Quantity Per Unit     Unit Price  Units In Stock  Units On Order  Reorder Level
----------  --------------------------------  -----------  -----------  --------------------  ----------  --------------  --------------  -------------
1           Chai                              1            1            10 boxes x 20 bags    18.0        39              0               10           
2           Chang                             1            1            24 - 12 oz bottles    19.0        17              40              25           
3           Aniseed Syrup                     1            2            12 - 550 ml bottles   10.0        13              70              25           
4           Chef Anton's Cajun Seasoning      2            2            48 - 6 oz jars        22.0        53              0               0            
6           Grandma's Boysenberry Spread      3            2            12 - 8 oz jars        25.0        120             0               25           
7           Uncle Bob's Organic Dried Pears   3            7            12 - 1 lb pkgs.       30.0        15              0               10           
8           Northwoods Cranberry Sauce        3            2            12 - 12 oz jars       40.0        6               0               0            
10          Ikura                             4            8            12 - 200 ml jars      31.0        31              0               0            
11          Queso Cabrales                    5            4            1 kg pkg.             21.0        22              30              30           
12          Queso Manchego La Pastora         5            4            10 - 500 g pkgs.      38.0        86              0               0            
13          Konbu                             6            8            2 kg box              6.0         24              0               5            
14          Tofu                              6            7            40 - 100 g pkgs.      23.25       35              0               0            
15          Genen Shouyu                      6            2            24 - 250 ml bottles   15.5        39              0               5            
16          Pavlova                           7            3            32 - 500 g boxes      17.45       29              0               10           
18          Carnarvon Tigers                  7            8            16 kg pkg.            62.5        42              0               0            
19          Teatime Chocolate Biscuits        8            3            10 boxes x 12 pieces  9.2         25              0               5            
20          Sir Rodney's Marmalade            8            3            30 gift boxes         81.0        40              0               0            
21          Sir Rodney's Scones               8            3            24 pkgs. x 4 pieces   10.0        3               40              5            
22          Gustaf's Knackebrod               9            5            24 - 500 g pkgs.      21.0        104             0               25           
23          Tunnbrod                          9            5            12 - 250 g pkgs.      9.0         61              0               25           
25          NuNuCa Nuss-Nougat-Creme          11           3            20 - 450 g glasses    14.0        76              0               30           
26          Gumbar Gummibarchen               11           3            100 - 250 g bags      31.23       15              0               0            
27          Schoggi Schokolade                11           3            100 - 100 g pieces    43.9        49              0               30           
30          Nord-Ost Matjeshering             13           8            10 - 200 g glasses    25.89       10              0               15           
31          Gorgonzola Telino                 14           4            12 - 100 g pkgs       12.5        0               70              20           
32          Mascarpone Fabioli                14           4            24 - 200 g pkgs.      32.0        9               40              25           
33          Geitost                           15           4            500 g                 2.5         112             0               20           
34          Sasquatch Ale                     16           1            24 - 12 oz bottles    14.0        111             0               15           
35          Steeleye Stout                    16           1            24 - 12 oz bottles    18.0        20              0               15           
36          Inlagd Sill                       17           8            24 - 250 g  jars      19.0        112             0               20           
37          Gravad lax                        17           8            12 - 500 g pkgs.      26.0        11              50              25           
38          Cote de Blaye                     18           1            12 - 75 cl bottles    263.5       17              0               15           
39          Chartreuse verte                  18           1            750 cc per bottle     18.0        69              0               5            
40          Boston Crab Meat                  19           8            24 - 4 oz tins        18.4        123             0               30           
41          Jack's New England Clam Chowder   19           8            12 - 12 oz cans       9.65        85              0               10           
43          Ipoh Coffee                       20           1            16 - 500 g tins       46.0        17              10              25           
44          Gula Malacca                      20           2            20 - 2 kg bags        19.45       27              0               15           
45          Rogede sild                       21           8            1k pkg.               9.5         5               70              15           
46          Spegesild                         21           8            4 - 450 g glasses     12.0        95              0               0            
47          Zaanse koeken                     22           3            10 - 4 oz boxes       9.5         36              0               0            
48          Chocolade                         22           3            10 pkgs.              12.75       15              70              25           
49          Maxilaku                          23           3            24 - 50 g pkgs.       20.0        10              60              15           
50          Valkoinen suklaa                  23           3            12 - 100 g bars       16.25       65              0               30           
51          Manjimup Dried Apples             24           7            50 - 300 g pkgs.      53.0        20              0               10           
52          Filo Mix                          24           5            16 - 2 kg boxes       7.0         38              0               25           
54          Tourtiere                         25           6            16 pies               7.45        21              0               10           
55          Pate chinois                      25           6            24 boxes x 2 pies     24.0        115             0               20           
56          Gnocchi di nonna Alice            26           5            24 - 250 g pkgs.      38.0        21              10              30           
57          Ravioli Angelo                    26           5            24 - 250 g pkgs.      19.5        36              0               20           
58          Escargots de Bourgogne            27           8            24 pieces             13.25       62              0               20           
59          Raclette Courdavault              28           4            5 kg pkg.             55.0        79              0               0            
60          Camembert Pierrot                 28           4            15 - 300 g rounds     34.0        19              0               0            
61          Sirop d'erable                    29           2            24 - 500 ml bottles   28.5        113             0               25           
62          Tarte au sucre                    29           3            48 pies               49.3        17              0               0            
63          Vegie-spread                      7            2            15 - 625 g jars       43.9        24              0               5            
64          Wimmers gute Semmelknodel         12           5            20 bags x 4 pieces    33.25       22              80              30           
65          Louisiana Fiery Hot Pepper Sauce  2            2            32 - 8 oz bottles     21.05       76              0               0            
66          Louisiana Hot Spiced Okra         2            2            24 - 8 oz jars        17.0        4               100             20           
67          Laughing Lumberjack Lager         16           1            24 - 12 oz bottles    14.0        52              0               10           
68          Scottish Longbreads               8            3            10 boxes x 8 pieces   12.5        6               10              15           
69          Gudbrandsdalsost                  15           4            10 kg pkg.            36.0        26              0               15           
70          Outback Lager                     7            1            24 - 355 ml bottles   15.0        15              10              30           
71          Flotemysost                       15           4            10 - 500 g pkgs.      21.5        26              0               0            
72          Mozzarella di Giovanni            14           4            24 - 200 g pkgs.      34.8        14              0               0            
73          Rod Kaviar                        17           8            24 - 150 g jars       15.0        101             0               5            
74          Longlife Tofu                     4            7            5 kg pkg.             10.0        4               20              5            
75          Rhonbrau Klosterbier              12           1            24 - 0.5 l bottles    7.75        125             0               25           
76          Lakkalikoori                      23           1            500 ml                18.0        57              0               20           
77          Original Frankfurter grune Sosse  12           2            12 boxes              13.0        32              0               15           
```



## Q03: Product by price (0.2%)
- Write a single SQL statement to list all the rows from the Product table with ProductName, UnitPrice and ProjectedUnitsInStock columns only. The ProjectedUnitsInStock column combines the value from UnitsInStock and UnitsOnOrder. The rows should be sorted by UnitPrice in descending order


## Sample output
```
ProductName                       UnitPrice  ProjectedUnitsInStock
--------------------------------  ---------  ---------------------
Cote de Blaye                     263.5      17                   
Thuringer Rostbratwurst           123.79     0                    
Mishi Kobe Niku                   97.0       29                   
Sir Rodney's Marmalade            81.0       40                   
Carnarvon Tigers                  62.5       42                   
Raclette Courdavault              55.0       79                   
Manjimup Dried Apples             53.0       20                   
Tarte au sucre                    49.3       17                   
Ipoh Coffee                       46.0       27                   
Rossle Sauerkraut                 45.6       26                   
Schoggi Schokolade                43.9       49                   
Vegie-spread                      43.9       24                   
Northwoods Cranberry Sauce        40.0       6                    
Alice Mutton                      39.0       0                    
Queso Manchego La Pastora         38.0       86                   
Gnocchi di nonna Alice            38.0       31                   
Gudbrandsdalsost                  36.0       26                   
Mozzarella di Giovanni            34.8       14                   
Camembert Pierrot                 34.0       19                   
Wimmers gute Semmelknodel         33.25      102                  
Perth Pasties                     32.8       0                    
Mascarpone Fabioli                32.0       49                   
Gumbar Gummibarchen               31.23      15                   
Ikura                             31.0       31                   
Uncle Bob's Organic Dried Pears   30.0       15                   
Sirop d'erable                    28.5       113                  
Gravad lax                        26.0       61                   
Nord-Ost Matjeshering             25.89      10                   
Grandma's Boysenberry Spread      25.0       120                  
Pate chinois                      24.0       115                  
Tofu                              23.25      35                   
Chef Anton's Cajun Seasoning      22.0       53                   
Flotemysost                       21.5       26                   
Chef Anton's Gumbo Mix            21.35      0                    
Louisiana Fiery Hot Pepper Sauce  21.05      76                   
Queso Cabrales                    21.0       52                   
Gustaf's Knackebrod               21.0       104                  
Maxilaku                          20.0       70                   
Ravioli Angelo                    19.5       36                   
Gula Malacca                      19.45      27                   
Chang                             19.0       57                   
Inlagd Sill                       19.0       112                  
Boston Crab Meat                  18.4       123                  
Chai                              18.0       39                   
Steeleye Stout                    18.0       20                   
Chartreuse verte                  18.0       69                   
Lakkalikoori                      18.0       57                   
Pavlova                           17.45      29                   
Louisiana Hot Spiced Okra         17.0       104                  
Valkoinen suklaa                  16.25      65                   
Genen Shouyu                      15.5       39                   
Outback Lager                     15.0       25                   
Rod Kaviar                        15.0       101                  
NuNuCa Nuss-Nougat-Creme          14.0       76                   
Sasquatch Ale                     14.0       111                  
Singaporean Hokkien Fried Mee     14.0       26                   
Laughing Lumberjack Lager         14.0       52                   
Escargots de Bourgogne            13.25      62                   
Original Frankfurter grune Sosse  13.0       32                   
Chocolade                         12.75      85                   
Gorgonzola Telino                 12.5       70                   
Scottish Longbreads               12.5       16                   
Spegesild                         12.0       95                   
Aniseed Syrup                     10.0       83                   
Sir Rodney's Scones               10.0       43                   
Longlife Tofu                     10.0       24                   
Jack's New England Clam Chowder   9.65       85                   
Rogede sild                       9.5        75                   
Zaanse koeken                     9.5        36                   
Teatime Chocolate Biscuits        9.2        25                   
Tunnbrod                          9.0        61                   
Rhonbrau Klosterbier              7.75       125                  
Tourtiere                         7.45       21                   
Filo Mix                          7.0        38                   
Konbu                             6.0        24                   
Guarana Fantastica                4.5        20                   
Geitost                           2.5        112
```



## Q04: What is the number (0.2%)
- You need to locate the phone number of a shipper company named Speedy Express. Write a single SQL statement to retrieve only that piece of information from the appropriate table. Your SQL statement should cater for different casing scenarios


## Sample output
```
Phone         
--------------
(503) 555-9831
```



## Q05: Fax check (0.2%)
- You have been informed that some fax numbers of customers have been wrongfully inputed with IP addresses. Write a single SQL statement to list rows from the Customer table only if those customers have an IP address looking fax number


## Sample output
```
CustomerID  CompanyName                ContactName         ContactTitle           Address                       City        Region  PostalCode  Country  Phone            Fax            
----------  -------------------------  ------------------  ---------------------  ----------------------------  ----------  ------  ----------  -------  ---------------  ---------------
BLONP       Blondesddsl pere et fils   Frederique Citeaux  Marketing Manager      24, place Kleber              Strasbourg          67000       France   88.60.15.31      88.60.15.32    
BONAP       Bon app'                   Laurence Lebihan    Owner                  12, rue des Bouchers          Marseille           13008       France   91.24.45.40      91.24.45.41    
DUMON       Du monde entier            Janine Labrune      Owner                  67, rue des Cinquante Otages  Nantes              44000       France   40.67.88.88      40.67.89.89    
FOLIG       Folies gourmandes          Martine Rance       Assistant Sales Agent  184, chaussee de Tournai      Lille               59000       France   20.16.10.16      20.16.10.17    
FRANR       France restauration        Carine Schmitt      Marketing Manager      54, rue Royale                Nantes              44000       France   40.32.21.21      40.32.21.20    
LACOR       La corne d'abondance       Daniel Tonini       Sales Representative   67, avenue de l'Europe        Versailles          78000       France   30.59.84.10      30.59.85.11    
LAMAI       La maison d'Asie           Annette Roulet      Sales Manager          1 rue Alsace-Lorraine         Toulouse            31000       France   61.77.61.10      61.77.61.11    
PARIS       Paris specialites          Marie Bertrand      Owner                  265, boulevard Charonne       Paris               75012       France   (1) 42.34.22.66  (1) 42.34.22.77
SPECD       Specialites du monde       Dominique Perrier   Marketing Manager      25, rue Lauriston             Paris               75016       France   (1) 47.55.60.10  (1) 47.55.60.20
VICTE       Victuailles en stock       Mary Saveley        Sales Agent            2, rue du Commerce            Lyon                69004       France   78.32.54.86      78.32.54.87    
VINET       Vins et alcools Chevalier  Paul Henriot        Accounting Manager     59 rue de l'Abbaye            Reims               51100       France   26.47.15.10      26.47.15.11    
```



## Q06: Show some orders (0.2%)
- Write a single SQL statement to list rows from the Order table when their OrderDate values are within the month of August in 1997


## Sample output
```
OrderID  CustomerID  EmployeeID  OrderDate                RequiredDate             ShippedDate              ShipVia  Freight  ShipName                            ShipAddress                                 ShipCity         ShipRegion     ShipPostalCode  ShipCountry
-------  ----------  ----------  -----------------------  -----------------------  -----------------------  -------  -------  ----------------------------------  ------------------------------------------  ---------------  -------------  --------------  -----------
10618    MEREP       1           1997-08-01 00:00:00.000  1997-09-12 00:00:00.000  1997-08-08 00:00:00.000  1        154.68   Mere Paillarde                      43 rue St. Laurent                          Montreal         Quebec         H1J 1C3         Canada     
10619    MEREP       3           1997-08-04 00:00:00.000  1997-09-01 00:00:00.000  1997-08-07 00:00:00.000  3        91.05    Mere Paillarde                      43 rue St. Laurent                          Montreal         Quebec         H1J 1C3         Canada     
10620    LAUGB       2           1997-08-05 00:00:00.000  1997-09-02 00:00:00.000  1997-08-14 00:00:00.000  3        0.94     Laughing Bacchus Wine Cellars       2319 Elm St.                                Vancouver        BC             V3F 2K1         Canada     
10621    ISLAT       4           1997-08-05 00:00:00.000  1997-09-02 00:00:00.000  1997-08-11 00:00:00.000  2        23.73    Island Trading                      Garden House Crowther Way                   Cowes            Isle of Wight  PO31 7PJ        UK         
10622    RICAR       4           1997-08-06 00:00:00.000  1997-09-03 00:00:00.000  1997-08-11 00:00:00.000  3        50.97    Ricardo Adocicados                  Av. Copacabana, 267                         Rio de Janeiro   RJ             02389-890       Brazil     
10623    FRANK       8           1997-08-07 00:00:00.000  1997-09-04 00:00:00.000  1997-08-12 00:00:00.000  2        97.18    Frankenversand                      Berliner Platz 43                           Munchen                         80805           Germany    
10624    THECR       4           1997-08-07 00:00:00.000  1997-09-04 00:00:00.000  1997-08-19 00:00:00.000  2        94.8     The Cracker Box                     55 Grizzly Peak Rd.                         Butte            MT             59801           USA        
10625    ANATR       3           1997-08-08 00:00:00.000  1997-09-05 00:00:00.000  1997-08-14 00:00:00.000  1        43.9     Ana Trujillo Emparedados y helados  Avda. de la Constitucion 2222               Mexico D.F.                     5021            Mexico     
10626    BERGS       1           1997-08-11 00:00:00.000  1997-09-08 00:00:00.000  1997-08-20 00:00:00.000  2        138.69   Berglunds snabbkop                  Berguvsvagen �8                             Lulea                           S-958 22        Sweden     
10627    SAVEA       8           1997-08-11 00:00:00.000  1997-09-22 00:00:00.000  1997-08-21 00:00:00.000  3        107.46   Save-a-lot Markets                  187 Suffolk Ln.                             Boise            ID             83720           USA        
10628    BLONP       4           1997-08-12 00:00:00.000  1997-09-09 00:00:00.000  1997-08-20 00:00:00.000  3        30.36    Blondel pere et fils                24, place Kleber                            Strasbourg                      67000           France     
10629    GODOS       4           1997-08-12 00:00:00.000  1997-09-09 00:00:00.000  1997-08-20 00:00:00.000  3        85.46    Godos Cocina Tipica                 C/ Romero, 33                               Sevilla                         41101           Spain      
10630    KOENE       1           1997-08-13 00:00:00.000  1997-09-10 00:00:00.000  1997-08-19 00:00:00.000  2        32.35    Koniglich Essen                     Maubelstr. 90                               Brandenburg                     14776           Germany    
10631    LAMAI       8           1997-08-14 00:00:00.000  1997-09-11 00:00:00.000  1997-08-15 00:00:00.000  1        0.87     La maison d-Asie                    1 rue Alsace-Lorraine                       Toulouse                        31000           France     
10632    WANDK       8           1997-08-14 00:00:00.000  1997-09-11 00:00:00.000  1997-08-19 00:00:00.000  1        41.38    Die Wandernde Kuh                   Adenauerallee 900                           Stuttgart                       70563           Germany    
10633    ERNSH       7           1997-08-15 00:00:00.000  1997-09-12 00:00:00.000  1997-08-18 00:00:00.000  3        477.9    Ernst Handel                        Kirchgasse 6                                Graz                            8010            Austria    
10634    FOLIG       4           1997-08-15 00:00:00.000  1997-09-12 00:00:00.000  1997-08-21 00:00:00.000  3        487.38   Folies gourmandes                   184, chaussee de Tournai                    Lille                           59000           France     
10635    MAGAA       8           1997-08-18 00:00:00.000  1997-09-15 00:00:00.000  1997-08-21 00:00:00.000  3        47.46    Magazzini Alimentari Riuniti        Via Ludovico il Moro 22                     Bergamo                         24100           Italy      
10636    WARTH       4           1997-08-19 00:00:00.000  1997-09-16 00:00:00.000  1997-08-26 00:00:00.000  1        1.15     Wartian Herkku                      Torikatu 38                                 Oulu                            90110           Finland    
10637    QUEEN       6           1997-08-19 00:00:00.000  1997-09-16 00:00:00.000  1997-08-26 00:00:00.000  1        201.29   Queen Cozinha                       Alameda dos Canarios, 891                   Sao Paulo        SP             05487-020       Brazil     
10638    LINOD       3           1997-08-20 00:00:00.000  1997-09-17 00:00:00.000  1997-09-01 00:00:00.000  1        158.44   LINO-Delicateses                    Ave. 5 de Mayo Porlamar                     I. de Margarita  Nueva Esparta  4980            Venezuela  
10639    SANTG       7           1997-08-20 00:00:00.000  1997-09-17 00:00:00.000  1997-08-27 00:00:00.000  3        38.64    Sante Gourmet                       Erling Skakkes gate 78                      Stavern                         4110            Norway     
10640    WANDK       4           1997-08-21 00:00:00.000  1997-09-18 00:00:00.000  1997-08-28 00:00:00.000  1        23.55    Die Wandernde Kuh                   Adenauerallee 900                           Stuttgart                       70563           Germany    
10641    HILAA       4           1997-08-22 00:00:00.000  1997-09-19 00:00:00.000  1997-08-26 00:00:00.000  2        179.61   HILARION-Abastos                    Carrera 22 con Ave. Carlos Soublette #8-35  San Cristobal    Tachira        5022            Venezuela  
10642    SIMOB       7           1997-08-22 00:00:00.000  1997-09-19 00:00:00.000  1997-09-05 00:00:00.000  3        41.89    Simons bistro                       Vinbaeltet 34                               Kobenhavn                       1734            Denmark    
10643    ALFKI       6           1997-08-25 00:00:00.000  1997-09-22 00:00:00.000  1997-09-02 00:00:00.000  1        29.46    Alfreds Futterkiste                 Obere Str. 57                               Berlin                          12209           Germany    
10644    WELLI       3           1997-08-25 00:00:00.000  1997-09-22 00:00:00.000  1997-09-01 00:00:00.000  2        0.14     Wellington Importadora              Rua do Mercado, 12                          Resende          SP             08737-363       Brazil     
10645    HANAR       4           1997-08-26 00:00:00.000  1997-09-23 00:00:00.000  1997-09-02 00:00:00.000  1        12.41    Hanari Carnes                       Rua do Paco, 67                             Rio de Janeiro   RJ             05454-876       Brazil     
10646    HUNGO       9           1997-08-27 00:00:00.000  1997-10-08 00:00:00.000  1997-09-03 00:00:00.000  3        142.33   Hungry Owl All-Night Grocers        8 Johnstown Road                            Cork             Co. Cork                       Ireland    
10647    QUEDE       4           1997-08-27 00:00:00.000  1997-09-10 00:00:00.000  1997-09-03 00:00:00.000  2        45.54    Que Delicia                         Rua da Panificadora, 12                     Rio de Janeiro   RJ             02389-673       Brazil     
10648    RICAR       5           1997-08-28 00:00:00.000  1997-10-09 00:00:00.000  1997-09-09 00:00:00.000  2        14.25    Ricardo Adocicados                  Av. Copacabana, 267                         Rio de Janeiro   RJ             02389-890       Brazil     
10649    MAISD       5           1997-08-28 00:00:00.000  1997-09-25 00:00:00.000  1997-08-29 00:00:00.000  3        6.2      Maison Dewey                        Rue Joseph-Bens 532                         Bruxelles                       B-1180          Belgium    
10650    FAMIA       5           1997-08-29 00:00:00.000  1997-09-26 00:00:00.000  1997-09-03 00:00:00.000  3        176.81   Familia Arquibaldo                  Rua Oros, 92                                Sao Paulo        SP             05442-030       Brazil     
```



## Q07: Where are you from (0.2%)
- You want to generate a list of countries that covers all existing customers. Write a single SQL statement to retrieve a country list without any duplication (i.e. the same country should not appear twice in that list) from the Customer table. Sort the list by country in an ascending order.


## Sample output
```
Country    
-----------
Argentina  
Austria    
Belgium    
Brazil     
Canada     
Denmark    
Finland    
France     
Germany    
Ireland    
Italy      
Mexico     
Norway     
Poland     
Portugal   
Spain      
Sweden     
Switzerland
UK         
USA        
Venezuela
```



## Q08: Number of order (0.2%)
- Write a single SQL statement to return the total number of rows in the Order table. Rename the column in the output as “Number of Order”


## Sample output
```
Number of Order
---------------
830   
```



## Q09: Two ways to go (0.2%)
- Write a single SQL statement without using any function to retrieve a list of products where the ProductName is exactly 9 characters long
- Write another single SQL statement that uses appropriate function to do the exact same thing


## Sample output
```
ProductName
-----------
Spegesild  
Chocolade  
Tourtiere  
```



## Q10: Bottom 10 products (0.2%)
- You are interested to know the bottom 6th to 15th stocked products in the inventory. Write a single SQL statement to retrieve that information from the Product table with both the ProductName and UnitsInStock columns


## Sample output
```
ProductName                 UnitsInStock
--------------------------  ------------
Sir Rodney's Scones         3           
Louisiana Hot Spiced Okra   4           
Longlife Tofu               4           
Rogede sild                 5           
Northwoods Cranberry Sauce  6           
Scottish Longbreads         6           
Mascarpone Fabioli          9           
Nord-Ost Matjeshering       10          
Maxilaku                    10          
Gravad lax                  11          
```



## Q11: Tidy list of employee (0.5%)
- Write a single SQL statement to generate a tidy employee list with only two columns, one with their full name and one with their full address. The full name is composed of the FirstName, the LastName in all caps, and a comma in between


## Sample output
```
Name               Address                                          
-----------------  -------------------------------------------------
DAVOLIO, Nancy     507 - 20th Ave. E.Apt. 2A, Seattle 98122, USA    
FULLER, Andrew     908 W. Capital Way, Tacoma 98401, USA            
LEVERLING, Janet   722 Moss Bay Blvd., Kirkland 98033, USA          
PEACOCK, Margaret  4110 Old Redmond Rd., Redmond 98052, USA         
BUCHANAN, Steven   14 Garrett Hill, London SW1 8JR, UK              
SUYAMA, Michael    Coventry House Miner Rd., London EC2 7JR, UK     
KING, Robert       Edgeham Hollow Winchester Way, London RG1 9SP, UK
CALLAHAN, Laura    4726 - 11th Ave. N.E., Seattle 98105, USA        
DODSWORTH, Anne    7 Houndstooth Rd., London WG2 7LT, UK            
```



## Q12: Show the details (0.5%)
- You need to examine the order detail of the order 10300. Write a SQL statement that would retrieve the rows from the OrderDetail table with the columns OrderID, ProductID, UnitPrice, Quantity, Discount and a derived column named Subtotal which is calculated from other columns. For the output, the columns UnitPrice and Subtotal should have a dollar sign ($) as prefix, and Discount should be shown as a percentage with the sign (%) as suffix


## Sample output
```
OrderID  ProductID  UnitPrice  Quantity  Discount  Subtotal
-------  ---------  ---------  --------  --------  --------
10300    66         $13.6      30        0.0%      $408.0  
10300    68         $10.0      20        0.0%      $200.0  
```



## Q13: Multiple conditions (0.5%)
- Write a single SQL statement that can list all products with the following features: ProductName begins with C; CategoryID equals to either 1 or 2; UnitPrice that is more than $20; and Discontinued is false


## Sample output
```
ProductName                   CategoryID  UnitPrice  Discontinued
----------------------------  ----------  ---------  ------------
Chef Anton's Cajun Seasoning  2           22.0       0           
Cote de Blaye                 1           263.5      0           
```



## Q14: Three new shippers (0.5%)

- Write the SQL statements to insert 3 rows to the Shipper table:

CompanyName | Phone
--- | ---
Trustworthy Delivery | (503) 555-1122
Amazing Pace | (503) 555-3421
Your Named Limited | (500) Your Student ID

- For the last row use your full name as the CompanyName and use the first 7 digits of your own student ID as the number for the Phone after the area code 500. Make sure the ShipperID is automatically generated and so you do not enter them manually



## Q15: How old are you (0.5%)

- Write a single SQL statement to generate a list of employees with FirstName, LastName and their exact age from the Employee table. For the output, the age should be rounded to the nearest number without any decimal value, and it should be shown as a whole number


## Sample output
```
LastName   FirstName  Age
---------  ---------  ---
Davolio    Nancy      73
Fuller     Andrew     70
Leverling  Janet      59
Peacock    Margaret   85
Buchanan   Steven     67
Suyama     Michael    59
King       Robert     62
Callahan   Laura      64
Dodsworth  Anne       56
```



## Q16: We are married (0.5%)
- Two employees of Northwind, Nancy Davolio and Andrew Fuller, are married and you are asked to update Nancy’s details in the Employee table. Write a SQL statement to update Nancy’s LastName from Davolio to Fuller, and also the TitleOfCourtesy from Ms. to Mrs. Your SQL statement should cater for different casing scenarios



## Q17: We live together (0.5%)
- Since Nancy has moved in with Andrew after they get married, you need to change her Address, City, Region, PostalCode and HomePhone columns from the Employee table. Write a SQL statement with subqueries to update Nancy’s contact details. Your SQL statement should cater for different casing scenarios



## Q18: Product history (0.5%)
- Write a single SQL statement to create a new table called ProductHistory with the following column specifications:

Column name | Data type | Nullability
--- | --- | ---
ProductID	| INTEGER	| No
EntryDate	| DATE	| No
UnitPrice	| REAL	| Yes
UnitsInStock	| INTEGER	| Yes
UnitsOnOrder	| INTEGER	| Yes
ReorderLevel	| INTEGER	| Yes
Discontinued	| INTEGER	| No

- You need to set a primary key and a foreign key for the table. Add a primary key constraint based on both the ProductID and EntryDate columns. Add a foreign key constraint based on the ProductID columns of both ProductHistory and Product tables



## Q19: Fill up the history (1%)
- Write a single SQL statement to fill up the ProductHistory table with all existing rows in the Product table. The EntryDate column should be filled with the current date and time that can be obtained from the appropriate function



## Q20: Good day to hire (1%)
- Write a single SQL statement to create a list with 2 columns: Day of Week and Hired. The Day of Week column has a possible value ranged from Monday to Sunday, and the Hired column shows the exact number of employees from the Employee table being hired on a particular Day of Week


## Sample output
```
Day of Week  Hired
-----------  -----
Friday       2    
Monday       1    
Saturday     1    
Sunday       3    
Tuesday      1    
Wednesday    1    
```



## Q21: Top sales representative (1%)
- You are asked to find out the top sales representative (by the accumulated dollar amount of orders) in Northwind. Write a single SQL statement to return the LastName, FirstName, and the total dollar amount of orders associated with that employee under the column Total with a dollar sign ($) as prefix


## Sample output
```
LastName  FirstName  Total      
--------  ---------  -----------
Peacock   Margaret   $232890.846
```



## Q22: Who is the boss (2%)
- Write a single SQL statement with no subquery to create a list with 2 columns: Employee and Manager. The column Employee shows the FirstName of the employee, and the column Manager shows the FirstName of the manager of that employee. Make sure that all employees are listed whether they have a manager or not. For those employees who do not have a manager, the value “No manager” should show up in the second column


## Sample output
```
Employee  Manager   
--------  ----------
Nancy     Andrew    
Andrew    No manager
Janet     Andrew    
Margaret  Andrew    
Steven    Andrew    
Michael   Steven    
Robert    Steven    
Laura     Andrew    
Anne      Steven    
```



## Q23: You have a discount (2%)
- You notice that some products are sold lower than their recommended prices (i.e. the UnitPrice with Discount in OrderDetail table is lower than the UnitPrice in the Product table). Write a single SQL statement to generate a full customer list with five columns:
  1. CompanyName column from the Customer table renamed as Company;
  2. Recommended column with the total amount they should have paid for all the products ordered based on the UnitPrice from the Product table,
  3. Ordered column with the total they have actually paid (calculated from the OrderDetail table),
  4. Discount column showing discount in absolute value, and
  5. Percentage column showing discount in percentage value

- All columns with numeric values should be rounded to two decimal places and displayed with appropriate prefix or suffix. Sort the list by the Percentage column in descending order to show which customer is enjoying the highest percentage of discount from Northwind


## Sample output
```
Company                             Recommended  Ordered     Discount   Percentage
----------------------------------  -----------  ----------  ---------  ----------
Queen Cozinha                       $34043.9     $25717.5    $8326.4    24.46%    
Bolido Comidas preparadas           $5543.3      $4232.85    $1310.45   23.64%    
Split Rail Beer & Ale               $14718.52    $11441.63   $3276.89   22.26%    
Piccolo und mehr                    $29595.0     $23128.86   $6466.14   21.85%    
Mere Paillarde                      $36878.5     $28872.19   $8006.31   21.71%    
Simons bistro                       $21062.25    $16817.1    $4245.15   20.16%    
Centro comercial Moctezuma          $126.0       $100.8      $25.2      20.0%     
Furia Bacalhau e Frutos do Mar      $8025.0      $6427.42    $1597.58   19.91%    
La maison d'Asie                    $11514.05    $9328.2     $2185.85   18.98%    
LILA-Supermercado                   $19819.66    $16076.6    $3743.06   18.89%    
Blondesddsl pere et fils            $22606.05    $18534.08   $4071.97   18.01%    
Comercio Mineiro                    $4637.6      $3810.75    $826.85    17.83%    
Die Wandernde Kuh                   $11623.55    $9588.43    $2035.13   17.51%    
Hungry Owl All-Night Grocers        $60397.91    $49979.91   $10418.0   17.25%    
Vins et alcools Chevalier           $1771.4      $1480.0     $291.4     16.45%    
Frankenversand                      $31794.43    $26656.56   $5137.87   16.16%    
Old World Delicatessen              $18039.25    $15177.46   $2861.79   15.86%    
Princesa Isabel Vinhos              $5987.9      $5044.94    $942.96    15.75%    
GROSELLA-Restaurante                $1764.6      $1488.7     $275.9     15.64%    
Wartian Herkku                      $18467.3     $15648.7    $2818.6    15.26%    
Victuailles en stock                $10823.3     $9182.43    $1640.87   15.16%    
Toms Spezialitaten                  $5632.15     $4778.14    $854.01    15.16%    
Bottom-Dollar Markets               $24429.15    $20801.6    $3627.55   14.85%    
Que Delicia                         $7789.53     $6664.81    $1124.72   14.44%    
Lehmanns Marktstand                 $22506.87    $19261.41   $3245.46   14.42%    
Wellington Importadora              $7085.3      $6068.2     $1017.1    14.36%    
Seven Seas Imports                  $18929.15    $16215.33   $2713.83   14.34%    
Familia Arquibaldo                  $4771.7      $4107.55    $664.15    13.92%    
Supremes delices                    $27918.4     $24088.78   $3829.62   13.72%    
Magazzini Alimentari Riuniti        $8313.35     $7176.22    $1137.13   13.68%    
Save-a-lot Markets                  $120718.85   $104361.95  $16356.9   13.55%    
Bon app'                            $25360.0     $21963.25   $3396.75   13.39%    
Ernst Handel                        $120390.09   $104874.98  $15515.11  12.89%    
Rattlesnake Canyon Grocery          $58562.42    $51097.8    $7464.62   12.75%    
Galeria del gastronomo              $955.5       $836.7      $118.8     12.43%    
Berglunds snabbkop                  $28355.75    $24927.58   $3428.17   12.09%    
Vaffeljernet                        $17976.82    $15843.93   $2132.9    11.86%    
Let's Stop N Shop                   $3490.02     $3076.47    $413.55    11.85%    
Laughing Bacchus Wine Cellars       $592.5       $522.5      $70.0      11.81%    
Folk och fa HB                      $33477.95    $29567.56   $3910.39   11.68%    
Romero y tomillo                    $1653.58     $1467.29    $186.29    11.27%    
HILARION-Abastos                    $25633.38    $22768.76   $2864.62   11.18%    
Tradic�o Hipermercados              $7684.62     $6850.66    $833.96    10.85%    
LINO-Delicateses                    $18429.55    $16476.57   $1952.99   10.6%     
Consolidated Holdings               $1916.5      $1719.1     $197.4     10.3%     
Pericles Comidas clasicas           $4727.16     $4242.2     $484.96    10.26%    
Ricardo Adocicados                  $13848.25    $12450.8    $1397.45   10.09%    
Ottilies Kaseladen                  $13893.7     $12496.2    $1397.5    10.06%    
White Clover Markets                $30417.6     $27363.61   $3054.0    10.04%    
QUICK-Stop                          $122199.74   $110277.31  $11922.44  9.76%     
Lazy K Kountry Store                $394.0       $357.0      $37.0      9.39%     
Reggiani Caseifici                  $7762.2      $7048.24    $713.96    9.2%      
Richter Supermarkt                  $21210.45    $19343.78   $1866.67   8.8%      
B's Beverages                       $6638.55     $6089.9     $548.65    8.26%     
Eastern Connection                  $16037.75    $14761.04   $1276.72   7.96%     
Antonio Moreno Taqueria             $7616.15     $7023.98    $592.17    7.78%     
Chop-suey Chinese                   $13336.1     $12348.88   $987.22    7.4%      
Tortuga Restaurante                 $11667.25    $10812.15   $855.1     7.33%     
Alfreds Futterkiste                 $4596.2      $4273.0     $323.2     7.03%     
Hungry Coyote Import Store          $3285.05     $3063.2     $221.85    6.75%     
Maison Dewey                        $10430.58    $9736.08    $694.51    6.66%     
Around the Horn                     $14264.5     $13390.65   $873.85    6.13%     
Great Lakes Food Market             $19711.13    $18507.45   $1203.68   6.11%     
Koniglich Essen                     $32902.62    $30908.38   $1994.24   6.06%     
Gourmet Lanchonetes                 $8957.23     $8414.14    $543.09    6.06%     
Hanari Carnes                       $34916.6     $32841.37   $2075.23   5.94%     
Godos Cocina Tipica                 $12143.1     $11446.36   $696.74    5.74%     
Morgenstern Gesundkost              $5345.0      $5042.2     $302.8     5.67%     
Folies gourmandes                   $12263.7     $11666.9    $596.8     4.87%     
Sante Gourmet                       $6000.35     $5735.15    $265.2     4.42%     
Island Trading                      $6429.7      $6146.3     $283.4     4.41%     
Lonesome Pine Restaurant            $4437.1      $4258.6     $178.5     4.02%     
Du monde entier                     $1683.1      $1615.9     $67.2      3.99%     
Rancho grande                       $2956.08     $2844.1     $111.98    3.79%     
Drachenblut Delikatessen            $3896.61     $3763.21    $133.4     3.42%     
Wolski  Zajazd                      $3646.7      $3531.95    $114.75    3.15%     
The Big Cheese                      $3446.0      $3361.0     $85.0      2.47%     
Oceano Atlantico Ltda.              $3540.0      $3460.2     $79.8      2.25%     
Ana Trujillo Emparedados y helados  $1425.15     $1402.95    $22.2      1.56%     
Franchi S.p.A.                      $1558.36     $1545.7     $12.66     0.81%     
Wilman Kala                         $3161.35     $3161.35    $0.0       0.0%      
Trail's Head Gourmet Provisioners   $1571.2      $1571.2     $0.0       0.0%      
The Cracker Box                     $1947.24     $1947.24    $0.0       0.0%      
Specialites du monde                $2423.35     $2423.35    $0.0       0.0%      
North/South                         $649.0       $649.0      $0.0       0.0%      
La corne d'abondance                $1992.05     $1992.05    $0.0       0.0%      
France restauration                 $3172.16     $3172.16    $0.0       0.0%      
Cactus Comidas para llevar          $1814.8      $1814.8     $0.0       0.0%      
Blauer See Delikatessen             $3239.8      $3239.8     $0.0       0.0%      
```



## Q24: Best shipper (2%)
- According to the Order table, Northwind ships their products to over 20 different countries via three companies in the Shipper table. Over the years staff in Northwind have learnt from which shipper company would be the best for which country. Your task is to reveal that piece of tacit knowledge from the existing data, assuming that the preferred shipper would have earned the highest total Freight from Northwind for shipments to a particular country. Write a single SQL statement with subqueries to generate a list with two columns: the ShipCountry from the Order table and also the preferred CompanyName from the Shipper table for that country


## Sample output
```
ShipCountry  Shipper         
-----------  ----------------
France       Federal Shipping
Switzerland  Federal Shipping
Italy        Speedy Express  
Argentina    United Package  
Austria      United Package  
Belgium      United Package  
Brazil       United Package  
Canada       United Package  
Denmark      United Package  
Finland      United Package  
Germany      United Package  
Ireland      United Package  
Mexico       United Package  
Norway       United Package  
Poland       United Package  
Portugal     United Package  
Spain        United Package  
Sweden       United Package  
UK           United Package  
USA          United Package  
Venezuela    United Package  
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

Northwind is awesome in <span class="country">everywhere</span>!

[<i class="fas fa-print"></i>](?print-pdf#)
