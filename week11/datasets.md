##### Week 11 TOC
- [Introduction](readme.md)
- [Datasets](datasets.md)
- [Homework for Next Week](homework.md)

-----

### Datasets for Processing

In order to model migration, we will need to accumulate a bunch of independent datasets and attempt to join them together. For this week, let's work on two simple files.

[World Country GDP 1990-2017](gdp.csv)
[World Country Migrations Stock 1990-2017](UN_MigrantStockTotal_2017.csv)

To join these datasets together, we can use code like the following.

```python
import csv
import matplotlib.pyplot as plt

countryData = []

with open('UN_MigrantStockTotal_2017.csv') as csvfile:
    readCSV = csv.reader(csvfile, delimiter=',')
    for row in readCSV:
        countryData.append({"name":row[1], 
                            "1990pop":row[3], "1995pop":row[4], "2000pop":row[5], "2005pop":row[6],"2010pop":row[7],"2015pop":row[8],"2017pop":row[9],
                            "1990ms":row[10], "1995ms":row[11], "2000ms":row[12], "2005ms":row[13],"2010ms":row[14],"2015ms":row[15],"2017ms":row[16]
                           })
        

with open('GDP.csv') as csvfile:
    readCSV = csv.reader(csvfile, delimiter=',')
    for row in readCSV:
        for i, country in enumerate(countryData) :
            if country["name"] == row[0] :
                country["1990gdp"] = row[2]
                country["1995gdp"] = row[3]
                country["2000gdp"] = row[4]
                country["2005gdp"] = row[5]
                country["2010gdp"] = row[6]                    
                country["2015gdp"] = row[7]
                country["2017gdp"] = row[8] 
```
