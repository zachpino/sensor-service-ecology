##### Week 11 TOC
- [Introduction](readme.md)
- [Datasets](datasets.md)
- [Homework](homework.md)

-----

### Datasets for Processing

In order to model migration, we will need to accumulate a bunch of independent datasets and attempt to join them together. For this week, let's work on two simple files.

- [World Country GDP 1990-2017](gdp.csv)
- [World Country Migrations Stock 1990-2017](UN_MigrantStockTotal_2017.csv)

To join these datasets together and plot possible relationships, we can use code like the following.

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
            migrantPercentage1990 = int(row[3]) / (int(row[10])*1000)
        except:
            migrantPercentage1990 = 0
            
        try:
            migrantPercentage1995 = int(row[4]) / (int(row[11])*1000)
        except:
            migrantPercentage1995 = 0
            
        try:
            migrantPercentage2000 = int(row[5]) / (int(row[12])*1000)
        except:
            migrantPercentage2000 = 0
                
        try:
            migrantPercentage2005 = int(row[6]) / (int(row[13])*1000)
        except:
            migrantPercentage2005 = 0            
        
        try:
            migrantPercentage2010 = int(row[7]) / (int(row[14])*1000)
        except:
            migrantPercentage2010 = 0            
       
        try:
            migrantPercentage2015 = int(row[8]) / (int(row[15])*1000)
        except:
            migrantPercentage2015 = 0
                   
        try:
            migrantPercentage2017 = int(row[9]) / (int(row[16])*1000)
        except:
            migrantPercentage2017 = 0  

        #add country with all read data as dictionary
        countryData.append({"name":row[1],"gdpMatch" : False, 
                            "1990pop": row[10], "1995pop":row[11], "2000pop":row[12], "2005pop":row[13],"2010pop":row[14],"2015pop":row[15],"2017pop":row[16],
                            "1990ms":row[3], "1995ms":row[4], "2000ms":row[5], "2005ms":row[6],"2010ms":row[7],"2015ms":row[8],"2017ms":row[9],
                            "1990%":migrantPercentage1990,"1995%":migrantPercentage1995,"2000%":migrantPercentage2000,"2005%":migrantPercentage2005,"2010%":migrantPercentage2015,"2015%":migrantPercentage2015,"2017%":migrantPercentage2017,
                           })
#open GDP file        
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
                country["1990gdp"] = row[2]
                country["1995gdp"] = row[3]
                country["2000gdp"] = row[4]
                country["2005gdp"] = row[5]
                country["2010gdp"] = row[6]                    
                country["2015gdp"] = row[7]
                country["2017gdp"] = row[8] 
                country["gdpMatch"] = True

#create empty lists for later population
matchedCountries = []
unmatchedCountries = []

matchedCountryNames = []
unmatchedCountryNames = []

x = []
y = []

#work through countries again
for i,country in enumerate(countryData) :
    #countries that were unmatched
    if ( country["gdpMatch"] ) == False :
        unmatchedCountries.append(country)
        unmatchedCountryNames.append(country["name"])

    #countries that were matched
    else :
        matchedCountries.append(country)
        matchedCountryNames.append(country["name"])
        
        #do a double check to makes sure we have a GDP
        if country["2017gdp"] != "":
            x.append( float(country["2017pop"] ))
            y.append( round( float(country["2017gdp"] ) ) )

#add country name annotations to plot
for i,name in enumerate(names):
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
#print(unmatchedCountryNames)

#show plot, now interactive due to first line in code!
plt.show()

#plt.savefig('countryplot.eps', format='eps')

```
