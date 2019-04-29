##### Week 11 TOC
- [Introduction](readme.md)
- [Datasets and Code](datasets.md)
- [Homework](homework.md)

-----

### Datasets for Processing

In order to model migration, we will need to accumulate a bunch of independent datasets and attempt to join them together. For this week, let's work on two simple files.

- [World Country GDP 1990-2017](gdp.csv)
- [World Country Migrations Stock 1990-2017](UN_MigrantStockTotal_2017.csv)

In class, we wrote the following code, which allowed us to analyze relationships internal to the UN Migration Stock dataset.

```python
%config IPCompleter.greedy=True
%matplotlib notebook

import numpy as np
import csv
import matplotlib.pyplot as plt

from scipy.stats import linregress

#keep axes of plot uniform 
plt.axis("equal")

#empty list for building dataset
countryData = []

#open file
with open('UN_MigrantStockTotal_2017.csv') as csvfile : 
    readCSV = csv.reader(csvfile, delimiter=',')
    
    #skip header row
    next(readCSV)
    
    #analyze each row
    for row in readCSV:
        #print(row)
        
        try:
            migrantPercentage1990 = int(row[3]) / ( int(row[10]) * 1000 )
            
        except:
            migrantPercentage1990 = 0
            
        try:
            migrantPercentage2017 = int(row[9]) / ( int(row[16]) * 1000 )
            
        except:
            migrantPercentage2017 = 0

        #build new dataset with more limited, synthetized data point    
        countryData.append( {"name":row[1], "mp1990": migrantPercentage1990, "mp2017": migrantPercentage2017} )

        
#visualization data formatting
x = []
y = []
names = []

#collect all data for plotting
for i, country in enumerate(countryData):
    x.append(country["mp1990"])
    y.append(country["mp2017"])
    names.append(country["name"])

#calculate and show statistical relationships
print(linregress(x,y))
fit = np.polyfit(x,y,1)
regressionEquation = np.poly1d(fit) 

#data visualization

#draw country names next to scattered circles
#for i, name in enumerate(names):
   # plt.annotate(name, (x[i] , y[i]) , size=6, color="black" )

#draw scattered circles
plt.plot(x,y,"o", color="blue", alpha=.5)        

#label axes
plt.xlabel('1990 Migration Percentage')
plt.ylabel('2017 Migration Percentage')

#draw regression line
plt.plot(x, regressionEquation(x), color="#519D9E", linewidth=.5)
     
# print(countryData)

```

To join these datasets together and plot possible relationships, we can add a bit of code to compare datapoints.

```python
%matplotlib notebook

#necessary modules
import csv
import matplotlib.pyplot as plt
from scipy.stats import linregress
from pylab import * 

#empty list for later population
countryData = []

#open csv file
with open('UN_MigrantStockTotal_2017.csv') as csvfile:
    #read csv content
    readCSV = csv.reader(csvfile, delimiter=',')
    
    #skip first row
    next(readCSV)

    #read each row of CSV file
    for row in readCSV:
        
        #handle any errors that might occur because of missing data, and convert to appropriate datatype
                   
        try:
            migrantPercentage2017 = int(row[9]) / (int(row[16])*1000)
        except:
            migrantPercentage2017 = 0  

        #add country with all read data as dictionary
        countryData.append({ "name":row[1],"gdpMatch" : False, "2017pop":row[16], "2017ms":row[9], "2017%":migrantPercentage2017 })

#open GDP data file        
with open('gdp.csv') as csvfile:
    #read csv data
    readCSV = csv.reader(csvfile, delimiter=',')
    
    #skip first row
    next(readCSV)

    #for each row in the CSV file...
    for row in readCSV:
        #check each country datapoint for a match
        for i, country in enumerate(countryData) :
            # are the names the same between the 2 datasets?
            if country["name"] == row[0] :
                country["2017gdp"] = row[8] 
                country["gdpMatch"] = True

#create empty lists for later population
unmatchedCountryNames = []
matchedCountryNames = []

x = []
y = []

#work through countries again
for i,country in enumerate(countryData) :
    
    #countries that were unmatched
    if ( country["gdpMatch"] ) == False :
        unmatchedCountryNames.append(country["name"])

    #countries that were matched
    else :        
        #do a double check to makes sure we have a GDP
        if country["2017gdp"] != "":
            x.append( float(country["2017pop"] ))
            y.append( round( float(country["2017gdp"] ) ) )
            matchedCountryNames.append(country["name"])

#add country name annotations to plot
for i,name in enumerate(matchedCountryNames):
    plt.annotate(name, (x[i],y[i]), size=3, alpha=.5 )

#print statistical relationships
print(linregress(x,y))

#calculate best fit line 
fit = np.polyfit(x,y,1)

#get equation for fit line
regressionEquation = np.poly1d(fit) 

#plot points as circles ('o')
#https://matplotlib.org/api/_as_gen/matplotlib.pyplot.plot.html
plt.plot(x, y, "o", color="#D1B6E1", alpha=.5)

#draw a line with the x coordinates array and the regression equation
plt.plot(x, regressionEquation(x), color="#519D9E", linewidth=.5)

#draw labels on each axis
plt.xlabel('2017 Population')
plt.ylabel('2017 GDP')

#print all data for debugging
#print(countryData)
print("\nUnmatched Countries:" + str(unmatchedCountryNames))

#show plot, now interactive due to first line in code!
plt.show()

#plt.savefig('countryplot.eps', format='eps')
```
