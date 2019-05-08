##### Week 14 TOC
- [Datasets and Code](datasets.md)

-----

### Putting Things Together

Grab the bundled datasets here for our example modeling exercise.

- [Migration Datasets](migration_datasets.zip)


### Code

Numpy!

```python
import folium
import pandas as pd
import warnings
warnings.filterwarnings('ignore')

#Map Section -----------------------------------------------------------
folium_map = folium.Map(
                location=[0,0],
                zoom_start=1,
                tiles='Stamen Toner'
            )

# Available Tiles...
# OpenStreetMap
# Stamen Terrain
# Stamen Toner
# Stamen Watercolor
# CartoDB positron
# CartoDB dark_matter

#Load Data Section -----------------------------------------------------------
migrationData = pd.read_csv("UN_MigrantStockTotal.csv")
corruptionData = pd.read_csv("corruption-rating.csv")
pollutionData = pd.read_csv("air-pollution-deaths.csv")
disasterData = pd.read_csv("disaster-displacement.csv")
capitalsData = pd.read_csv("country-capitals.csv")

#Data Alignment Section -----------------------------------------------------------
mergedData = pd.merge(corruptionData, pollutionData, on="Country Name", how="outer")
mergedData = pd.merge(mergedData, disasterData, on="Country Name", how="outer")
mergedData = pd.merge(mergedData, capitalsData, on="Country Name", how="outer")
mergedData = pd.merge(mergedData, migrationData, on="Country Name", how="outer")

#Data Debugging Section -----------------------------------------------------------
#print(mergedData.to_csv(index=True))
#print(mergedData["total pop 2017"])
#mergedData[ ["Country Name", "migrants 2015", "CapitalLongitude"] ]
#print(mergedData.max())


#Model Section -----------------------------------------------------------
modelVals = []

for index, country in mergedData.iterrows():
    totalPop = country["total pop 2015"]
    
    migrantPercentage = country["migrants 2015"] / totalPop
    
    migrantChange = (country["migrants 2015"] / totalPop) - (country["migrants 2010"] / totalPop)
    
    corruptionRating = country["Corruption Perception Rating"] / 4.8
    
    displacementPercentage = country["Internally displaced persons"] / totalPop
    
    modelValue = (migrantPercentage*.3) + migrantChange + (corruptionRating*.2) + (displacementPercentage*.5)
    
    modelVals.append({"country": country["Country Name"], "model": modelValue})

#Visualization Section -----------------------------------------------------------
    try:
        #place a marker on the Folium map
        #also try just "Circle" instead of "CircleMarker"
        marker = folium.CircleMarker( 
            location=[float(country["CapitalLatitude"]), float(country["CapitalLongitude"])], 
            tooltip=country["Country Name"], 
            #note the multiplier to get these circles into
            radius=50*modelValue, 
            popup= str(modelValue),
            color= '#000000',
            fill=True,
            fill_color='#ff0000',
            fill_opacity=.5,
            weight=1
        )
        marker.add_to(folium_map)
    
    except:
        pass  
        #useful for debugging which countries didn't merge well
        #print(country["Country Name"])

#draw map        
folium_map

#print(mergedData)


```

