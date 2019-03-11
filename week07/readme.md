### Regions, Points, Counts, Forecasting

Let's add some nuance to our visualization tool, and use a more powerful modeling tool which permits data manipulation alongside point-based, areal, and projected data.

-----

#### Download the New Tool!

Grab this [zip file](census_cta_v3.zip) for our new data exploration tool. Make sure you have [Sublime](https://www.sublimetext.com) installed.

-----

#### For Review

Browsing the [long variable list](https://api.census.gov/data/2017/acs/acs5/variables.html) from the ACS is a painful process. The ACS provides a shorter list of variable clusters, called [variable groups](https://api.census.gov/data/2017/acs/acs5/groups.html), that is somewhat easier to scan.

Accessing total population from the ACS API for reference — Cook County, IL by tract.

```
https://api.census.gov/data/2017/acs/acs5?get=NAME,B01001_001E&for=tract:*&in=county:031&in=state:17&key=...
```

Loading the tool...

```
cd ~/Desktop/census_cta_v3/
python -m SimpleHTTPServer 8080 
```

Visit this page in Chrome, Safari, Firefox...

```
http://localhost:8080
```

-----

#### Alternate Data Store

The city of Chicago publishes municipal datasets at the [Chicago Data Portal](https://data.cityofchicago.org). 

-----

#### Reprojecting and Converting Geographic Data

Use the very useful [mapshaper](https://mapshaper.org) and [MyGeoData](https://mygeodata.cloud/converter/) to manipulate and convert shapefiles and geoJSON files.

-----

#### Recoloring and Downloading Maps

Visit the [D3 chromatic scale module page](https://github.com/d3/d3-scale-chromatic) to see possible color schemes for your maps. In particular, consider [diverging](https://github.com/d3/d3-scale-chromatic#diverging) and [sequential](https://github.com/d3/d3-scale-chromatic#sequential-multi-hue) color scales for better visibilty.

Many of these scales are based on the rigorously researched [ColorBrewer](http://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3) hue presets for perceptual uniformity and color-blind-safeness. 
