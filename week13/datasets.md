##### Week 13 TOC
- [Introduction](readme.md)
- [Datasets and Code](datasets.md)
- [Homework](homework.md)

-----

### Computational Cartography

In an Anaconda Terminal, install [Florium](https://python-visualization.github.io/folium/)! Florium is a simple wrapper around [Leaflet](https://leafletjs.com), a commonly used presentation layer for [Open Street Maps](https://www.openstreetmap.org/#map=5/38.007/-95.844).


### Datasets for Processing

Let's use a dataset from the [World Bank](https://data.worldbank.org/indicator/sm.pop.refg) on refugees, as well as a simple dataset that offers us the longitude and latitude of different world capitals.

- [World Refugees by Country](full_refugee.csv)
- [Country Capital Locations](country_capitals.csv)


```python
import folium
import csv
from statsmodels.tsa.arima_model import ARIMA
import warnings
warnings.filterwarnings('ignore')


#initialize map 
folium_map = folium.Map(location=[0,0],
                zoom_start=1,
                tiles='Stamen Toner',)

# Available Tiles...
# OpenStreetMap
# Stamen Terrain
# Stamen Toner
# Stamen Watercolor
# CartoDB positron
# CartoDB dark_matter

refugeeData = []

#open refugee file
with open('full_refugee.csv') as csvfile : 
    readCSV = csv.reader(csvfile, delimiter=',')
       
    #skip header row
    next(readCSV)

    #analyze each row
    for row in readCSV:
        refugees = []

        #gather all the data per country
        for i in range(1,29):
            if row[i] is not "":
                refugees.append( int(row[i] ))
            
        #make sure we have enough data points for analysis    
        if len(refugees) > 10 :
            
            #do ARIMA work    
            model = ARIMA(refugees, order=(1,1,0))
            model_fit = model.fit(disp=0)
            forecast = model_fit.forecast()[0]
            
            #print summary of ARIMA model
            print(row[0])
            print(model_fit.summary())
            print("\n")
            
            #Log Data
            refugeeData.append({ "name": row[0], "prediction":forecast[0] })
        
        else:
            #Log Data
            refugeeData.append({ "name": row[0], "prediction": False})

#open capital location file      
with open('country_capitals.csv') as csvfile : 
    readCSV = csv.reader(csvfile, delimiter=',')
    
    #skip header row
    next(readCSV)
    
    #analyze each row
    for row in readCSV:
        
        #work through refugee data and look for a match
        for i, country in enumerate(refugeeData):
            if row[0] == country["name"]:
                
                #check if we have a prediction
                if country["prediction"] is not False:
                    prediction = country["prediction"]

                    #place a marker on the Folium map
                    marker = folium.CircleMarker( 
                        location=[float(row[2]), float(row[3])], 
                        tooltip=row[1], 
                        radius=prediction/100000, 
                        popup= row[1] + ", " + row[0], 
                        color='#00ff00',
                        fill=True,
                        fill_color='#00cc33',
                        fill_opacity=.5,
                        weight=1
                    )
                    marker.add_to(folium_map)

#draw map        
folium_map
```
