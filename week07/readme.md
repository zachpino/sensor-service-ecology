### Regions, Points, Counts, Forecasting

Let's add some nuance to our visualization tool, and build a better modeling tool from composite data sources.

#### Download the New Tool!

Grab this [zip file](census_cta_v2.zip) for our new data exploration tool. Make sure you have [Sublime](https://www.sublimetext.com) installed.

#### For Reference

Accessing total population from the ACS API for reference — Cook County, IL by tract.

```
https://api.census.gov/data/2017/acs/acs5?get=NAME,B01001_001E&for=tract:*&in=county:031&in=state:17&key=...
```

Loading the tool...

```
cd ~/Desktop/census_cta_v2/
python -m SimpleHTTPServer 8080 
```

Visit this page in Chrome, Safari, Firefox...

```
http://localhost:8080
```

#### Alternate Data Store

The city of Chicago publishes municipal datasets at the [Chicago Data Portal](https://data.cityofchicago.org). We'll reference datasets from this data store, and several are already included in the zip package.