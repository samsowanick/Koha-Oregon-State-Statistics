# Koha Oregon State Statistics
A static html page that collects State statistics from Koha to be displayed nicely. Easily copy the text, or print the report. 

<img width="1190" height="566" alt="example-report" src="https://github.com/user-attachments/assets/52805387-53b7-440f-8f02-a1f4ebc4c608" />


*Please not that both **Base URL** and **Report ID's** will need updated to point to your Koha URL (public url, not staff).

# Report Defaults
Under the defaults section in the <script>, you can change what the parameters are filled in with on page load. This makes it easy to pre populate the data that you know will likely be the same from year to year.

# Koha Reports
**All reports in Koha should be made public, or else the data cannot be retrieved.**

#118 - Number of registered users
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)

FROM borrowers

WHERE borrowers.cardnumber is not null
````

#119 - Number of registered users added
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)

FROM (select * from borrowers UNION ALL select * from deletedborrowers) borrowers

WHERE borrowers.cardnumber is not null

AND borrowers.dateenrolled BETWEEN CONCAT((<<year>> - 1),('-07-01')) AND CONCAT((<<year>>),('-06-30'))
````

#501 - Number of physical units
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)
FROM items i

WHERE barcode <>  '' AND barcode IS NOT NULL

AND i.permanent_location in ('your','book','codes','here')

AND dateaccessioned <= CONCAT((<<year>>),'-07-01')
````

#502 - Number of physical units added
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)

FROM items i

WHERE barcode <>  '' AND barcode IS NOT NULL

AND i.permanent_location in ('your','book','codes','here')

AND dateaccessioned BETWEEN CONCAT((<<year>> - 1),'-06-30') AND CONCAT((<<year>>),'-07-01')
````

#503 - Audio materials - physical units
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)

FROM items i

WHERE barcode <>  '' AND barcode IS NOT NULL

AND i.permanent_location in ('your','audio','codes','here')

and dateaccessioned <= CONCAT(<<year>>,'-07-01')
````

#504 - Audio materials - physical units added
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)

FROM items i

WHERE barcode <>  '' AND barcode IS NOT NULL

AND i.permanent_location in ('your','audio','codes','here')

and dateaccessioned BETWEEN CONCAT(<<year>> - 1,'-06-30') AND CONCAT(<<year>>,'-07-01')
````

#505 - Video materials - physical units
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)

FROM items i

WHERE barcode <>  '' AND barcode IS NOT NULL

AND i.permanent_location in ('your','video','codes','here')

and dateaccessioned <= CONCAT(<<year>>,'-07-01')
````

#506 - Video materials - physical units added
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)

FROM items i

WHERE barcode <>  '' AND barcode IS NOT NULL

AND i.permanent_location in ('your','video','codes','here')

AND dateaccessioned BETWEEN CONCAT((<<year>> - 1),'-06-30') AND CONCAT((<<year>>),'-07-01')
````

#507 - Other materials - physical units
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)

FROM items i

WHERE barcode <>  '' AND barcode IS NOT NULL

AND i.permanent_location in ('your','other','codes','here')

AND dateaccessioned <= CONCAT((<<year>>),'-07-01')
````

#508 - Other materials - physical units added
````
SELECT CONCAT(COUNT(*),',',(<<year>> - 1),'/',<<year>>)

FROM items i

WHERE barcode <>  '' AND barcode IS NOT NULL

AND i.permanent_location in ('your','other','codes','here')

AND dateaccessioned BETWEEN CONCAT((<<year>> - 1),'-06-30') AND CONCAT((<<year>>),'-07-01')
````

#610/611 - Number of first-time/renewals circulation of adult materials
````
SELECT CONCAT(SUM( IF(type = 'issue', 1, 0 )),',',SUM( IF(type = 'renew', 1, 0 )),',',CONCAT(<<year>> - 1,'/',<<year>>))

FROM (SELECT * FROM statistics 
WHERE type IN ('issue','renew','return') 
AND date(datetime) BETWEEN CONCAT((<<year>> - 1),'-07-01') AND CONCAT(<<year>>,'-06-30') 
AND ccode IN ('ADULT','LEASED')
) s
````

#612/613 - Number of first-time/renewals circulation of young adult (YA) materials
````
SELECT CONCAT(SUM( IF(type = 'issue', 1, 0 )),',',SUM( IF(type = 'renew', 1, 0 )),',',CONCAT(<<year>> - 1,'/',<<year>>))

FROM (SELECT * FROM statistics 
WHERE type IN ('issue','renew','return') 
AND date(datetime) BETWEEN CONCAT((<<year>> - 1),'-07-01') AND CONCAT(<<year>>,'-06-30') 
AND ccode IN ('YOUNGADULT')
) s
````

#614/615 - Number of first-time/renewals circulation of juvenile materials
````
SELECT CONCAT(SUM( IF(type = 'issue', 1, 0 )),',',SUM( IF(type = 'renew', 1, 0 )),',',CONCAT(<<year>> - 1,'/',<<year>>))

FROM (SELECT * FROM statistics 
WHERE type IN ('issue','renew','return') 
AND date(datetime) BETWEEN CONCAT((<<year>> - 1),'-07-01') AND CONCAT(<<year>>,'-06-30') 
AND ccode IN ('JUVENILE')
) s
````

#616/617 - Number of first-time/renewals circulation of other materials
````
SELECT CONCAT(SUM( IF(type = 'issue', 1, 0 )),',',SUM( IF(type = 'renew', 1, 0 )),',',CONCAT(<<year>> - 1,'/',<<year>>))

FROM (SELECT * FROM statistics 
WHERE type IN ('issue','renew','return') 
AND date(datetime) BETWEEN CONCAT((<<year>> - 1),'-07-01') AND CONCAT(<<year>>,'-06-30') 
AND ccode IN ('LOTCOL')
) s
````

#618/619 - Number of first-time/renewals circulation of all materials
````
SELECT CONCAT(SUM( IF(type = 'issue', 1, 0 )),',',SUM( IF(type = 'renew', 1, 0 )),',',CONCAT(<<year>> - 1,'/',<<year>>))

FROM (SELECT * FROM statistics 
WHERE type IN ('issue','renew','return') 
AND date(datetime) BETWEEN CONCAT((<<year>> - 1),'-07-01') AND CONCAT(<<year>>,'-06-30') 
AND ccode like ('%')
) s
````

#660 - Number of first-time circulation to non-residents
````
SELECT CONCAT(SUM( IF(type = 'issue', 1, 0 )),',',CONCAT(<<year>> - 1,'/',<<year>>))

FROM (SELECT * FROM statistics 
WHERE type IN ('issue','renew','return') 
AND date(datetime) BETWEEN CONCAT((<<year>> - 1),'-07-01') AND CONCAT(<<year>>,'-06-30') 
AND statistics.categorycode IN ('NONRES')
) s
````
