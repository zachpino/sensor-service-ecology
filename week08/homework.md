##### Week 08 TOC
- [Python History and Philosophy](readme.md)
- [Terminal Basics](terminal.md)
- [Intro to Python](python.md)
- [Homework](homework.md)

-----

### Homework

##### Divvy Data

![divvy](https://dailynorthwestern.com/wp-content/uploads/2016/06/divvyfile-1-900x600.jpg)

Divvy, the Chicago bikeshare program, provides [comprehensive data](https://www.divvybikes.com/system-data) on all trips that move through their transit network (published yearly) in addition to their awesome, [realtime API](https://feeds.divvybikes.com/stations/stations.json). 

The trip CSV datasets are rich in potential insights, but the source arrangement by trip prevents easy manipulation or trend discovery. 

```csv
Rental ID,Start Time,End Time,Bike ID,Duration In Seconds,Station ID,Start Station Name,End Station ID,End Station Name,User Type,Member Gender,Member Birth Year
17537179,2018-01-01 18:55:40,2018-01-01 19:05:55,2246,615.0,18,Wacker Dr & Washington St,26,McClurg Ct & Illinois St,Subscriber,Male,1964
17537180,2018-01-01 18:59:10,2018-01-01 19:02:14,6339,184.0,283,LaSalle St & Jackson Blvd,98,LaSalle St & Washington St,Subscriber,Male,1983
17537181,2018-01-01 19:05:19,2018-01-01 19:08:56,3806,217.0,133,Kingsbury St & Kinzie St,74,Kingsbury St & Erie St,Subscriber,Female,1978
17537182,2018-01-01 19:08:20,2018-01-01 19:14:21,3229,361.0,56,Desplaines St & Kinzie St,198,Green St & Madison St,Subscriber,Male,1990
17537183,2018-01-01 19:08:21,2018-01-01 19:23:36,5667,915.0,340,Clark St & Wrightwood Ave,312,Clarendon Ave & Gordon Ter,Subscriber,Female,1992
17537184,2018-01-01 19:12:32,2018-01-01 19:27:00,3891,868.0,110,Dearborn St & Erie St,38,Clark St & Lake St,Subscriber,Male,1994
```

For modeling purposes, we often want information from the *system's* point of view, and not from the point of view of the individual *agent* in the system. These datasets are also massive, more than 100MB of just text in each CSV, per quarter! 

To make this exercise a bit easier, the trip-oriented data for 2018 provided by Divvy was transformed into a station-oriented dataset via Python, which will make it much more useful for simulating the Divvy bikeshare system. Longitude, latitude, and bike capacity for each station were also pulled from the realtime API, and matched with station data based on the Divvy unique station `ID`. These conversions and data-matching processes will be discussed in detail in future weeks.

The new data structure is a list of dictionaries structured like this:

```python
{'name': 'Michigan Ave & 18th St', 'arrivals': 9885, 'longitude': -87.62455, 'departures': 9412, 'latitude': 41.857813, 'capacity': 23, 'ID': 273}
```

The data keys are:

```
'name'      : Divvy's name for the station, usually defined by the nearest two-street intersection
'ID'        : Unique numerical identifier for each station
'arrivals'  : The count of how many bikes arrived at the station in calendar year 2018
'departures': The count of how many bikes departed from the station in calendar year 2018
'longitude' : Location east-west, in degrees of longitude
'latitude'  : Location north-south, in degrees of latitude
'capacity'  : The number of available bike docks
```

Please write the Python code to evaluate this list of Python dictionaries, and try to answer the following questions through list and dictionary manipulation. Copy and past the code that you used to answer each question and keep it around for discussion next week.

1. What was the average count for arrivals to Divvy stations in 2018? What was the average count for departures?
2. What stations had the highest, and lowest, departure counts? Where are they? Where are they, and why might they be the extremes?
3. What stations had the highest, and lowest, arrival counts? Where are they, and why might they be the extremes?
4. What is the mean deviation of arrivals? What is the mean deviation of departures? Why might they be different, and what might that mean?
4. How do you think the Divvy station `ID`s were assigned?
5. Where is the geographic center of the Divvy system? Where is it in the city?
6. What are the different counts of bikes accomodated by Divvy docks? 
7. Add a *sharing differential* (`arrivals` - `departures`) key to each station. What is the average sharing differential? What are the max and min sharing differentials? How might these numbers be useful to Divvy system planners?
8. Add a *sharing ratio* (`arrivals` / `departures`) key to each station (beware that you should coerce both arrivals and departures into floats to prevent integer rounding!). What is the average sharing ratio? What are the max and min sharing ratios? How might these numbers be useful to Divvy system planners?

You can take a small subset of the data to work on (data for 50 Divvy stations), or chew through all of 622 Divvy stations!

-----

##### Partial Dataset

Copy and paste the data below for 50 Divvy stations, chosen randomly with `random.randint()`. Use this to test your code, as the complete dataset can be unwieldly.

```python
[{'name': 'Halsted St & 63rd St', 'arrivals': 408, 'longitude': -87.6446208145, 'departures': 384, 'latitude': 41.77938114228, 'capacity': 11, 'ID': 388}, {'name': 'Ashland Ave & 66th St', 'arrivals': 10, 'longitude': -87.663815, 'departures': 15, 'latitude': 41.774074, 'capacity': 7, 'ID': 565}, {'name': 'Greenwood Ave & 47th St', 'arrivals': 1296, 'longitude': -87.599383, 'departures': 1353, 'latitude': 41.809835, 'capacity': 15, 'ID': 252}, {'name': 'Kedzie Ave & Harrison St', 'arrivals': 82, 'longitude': -87.7048707493, 'departures': 69, 'latitude': 41.87359967462, 'capacity': 11, 'ID': 433}, {'name': 'Sedgwick St & Webster Ave', 'arrivals': 12078, 'longitude': -87.638888, 'departures': 12253, 'latitude': 41.922167, 'capacity': 15, 'ID': 143}, {'name': 'Larrabee St & Division St', 'arrivals': 8838, 'longitude': -87.6433534936, 'departures': 9575, 'latitude': 41.90348607004, 'capacity': 23, 'ID': 359}, {'name': 'Greenwood Ave & 79th St', 'arrivals': 43, 'longitude': -87.597552, 'departures': 45, 'latitude': 41.751294, 'capacity': 11, 'ID': 576}, {'name': 'Wentworth Ave & 35th St', 'arrivals': 1462, 'longitude': -87.632504, 'departures': 1439, 'latitude': 41.830777, 'capacity': 15, 'ID': 405}, {'name': 'Ashland Ave & Belle Plaine Ave', 'arrivals': 1301, 'longitude': -87.668835, 'departures': 1444, 'latitude': 41.956057, 'capacity': 11, 'ID': 246}, {'name': 'Damen Ave & Walnut (Lake) St (*)', 'arrivals': 137, 'longitude': -87.677009, 'departures': 117, 'latitude': 41.885951, 'capacity': 16, 'ID': 656}, {'name': 'Knox Ave & Montrose Ave', 'arrivals': 470, 'longitude': -87.745359, 'departures': 581, 'latitude': 41.960631, 'capacity': 11, 'ID': 592}, {'name': 'Michigan Ave & Congress Pkwy', 'arrivals': 7033, 'longitude': -87.624426, 'departures': 7802, 'latitude': 41.876243, 'capacity': 15, 'ID': 45}, {'name': 'Green St & Madison St', 'arrivals': 17588, 'longitude': -87.648789, 'departures': 16259, 'latitude': 41.881892, 'capacity': 27, 'ID': 198}, {'name': 'Ritchie Ct & Banks St', 'arrivals': 7248, 'longitude': -87.626217, 'departures': 7611, 'latitude': 41.906866, 'capacity': 15, 'ID': 180}, {'name': 'State St & 19th St', 'arrivals': 2547, 'longitude': -87.627542, 'departures': 2433, 'latitude': 41.856594, 'capacity': 15, 'ID': 178}, {'name': 'Halsted St & 47th Pl', 'arrivals': 270, 'longitude': -87.6456165545, 'departures': 240, 'latitude': 41.80813391481, 'capacity': 11, 'ID': 411}, {'name': 'Clark St & Chicago Ave', 'arrivals': 9985, 'longitude': -87.630931, 'departures': 10827, 'latitude': 41.896544, 'capacity': 19, 'ID': 337}, {'name': 'Prairie Ave & 43rd St', 'arrivals': 497, 'longitude': -87.6194124619, 'departures': 392, 'latitude': 41.81665889302, 'capacity': 15, 'ID': 410}, {'name': 'Shields Ave & 28th Pl', 'arrivals': 1131, 'longitude': -87.635491, 'departures': 1030, 'latitude': 41.842733, 'capacity': 11, 'ID': 401}, {'name': 'Western Blvd & 48th Pl', 'arrivals': 138, 'longitude': -87.683392, 'departures': 185, 'latitude': 41.805661, 'capacity': 11, 'ID': 594}, {'name': 'Halsted St & 37th St', 'arrivals': 581, 'longitude': -87.64572, 'departures': 532, 'latitude': 41.827059, 'capacity': 11, 'ID': 262}, {'name': 'May St & Taylor St', 'arrivals': 9577, 'longitude': -87.6554864, 'departures': 9437, 'latitude': 41.8694821, 'capacity': 15, 'ID': 22}, {'name': 'Racine Ave (May St) & Fulton St', 'arrivals': 7473, 'longitude': -87.656919, 'departures': 6507, 'latitude': 41.886926, 'capacity': 15, 'ID': 217}, {'name': 'DIVVY Map Frame B/C Station', 'arrivals': 269, 'longitude': False, 'departures': 71, 'latitude': False, 'capacity': False, 'ID': 360}, {'name': 'Racine Ave & Garfield Blvd', 'arrivals': 41, 'longitude': -87.655073, 'departures': 51, 'latitude': 41.794228, 'capacity': 11, 'ID': 559}, {'name': 'Federal St & Polk St', 'arrivals': 12692, 'longitude': -87.6295437729, 'departures': 12174, 'latitude': 41.87207763285, 'capacity': 19, 'ID': 41}, {'name': 'Hoyne Ave & Balmoral Ave', 'arrivals': 184, 'longitude': -87.681932, 'departures': 150, 'latitude': 41.979851, 'capacity': 15, 'ID': 655}, {'name': 'Michigan Ave & Lake St', 'arrivals': 22386, 'longitude': -87.624117, 'departures': 22198, 'latitude': 41.886024, 'capacity': 35, 'ID': 52}, {'name': 'Lakeview Ave & Fullerton Pkwy', 'arrivals': 14060, 'longitude': -87.638973, 'departures': 13453, 'latitude': 41.925858, 'capacity': 19, 'ID': 313}, {'name': 'DIVVY Map Frame B/C Station', 'arrivals': 269, 'longitude': False, 'departures': 71, 'latitude': False, 'capacity': False, 'ID': 360}, {'name': 'Central Park Ave & 24th St', 'arrivals': 147, 'longitude': -87.715068, 'departures': 149, 'latitude': 41.8481, 'capacity': 11, 'ID': 440}, {'name': 'Morgan St & Polk St', 'arrivals': 14370, 'longitude': -87.65103, 'departures': 14497, 'latitude': 41.871737, 'capacity': 19, 'ID': 241}, {'name': 'Lake Shore Dr & Monroe St', 'arrivals': 30049, 'longitude': -87.616743, 'departures': 36174, 'latitude': 41.880958, 'capacity': 39, 'ID': 76}, {'name': 'Princeton Ave & Garfield Blvd', 'arrivals': 338, 'longitude': -87.633124, 'departures': 379, 'latitude': 41.794982, 'capacity': 11, 'ID': 385}, {'name': 'Albany Ave & 26th St', 'arrivals': 230, 'longitude': -87.7020130128, 'departures': 195, 'latitude': 41.84447501366, 'capacity': 11, 'ID': 444}, {'name': 'Rockwell St & Eastwood Ave', 'arrivals': 2285, 'longitude': -87.6936384935, 'departures': 1939, 'latitude': 41.96590013976, 'capacity': 15, 'ID': 478}, {'name': 'Sedgwick St & Schiller St', 'arrivals': 2758, 'longitude': -87.638566, 'departures': 2861, 'latitude': 41.907626, 'capacity': 15, 'ID': 236}, {'name': 'Ritchie Ct & Banks St', 'arrivals': 7248, 'longitude': -87.626217, 'departures': 7611, 'latitude': 41.906866, 'capacity': 15, 'ID': 180}, {'name': 'Southport Ave & Irving Park Rd', 'arrivals': 4753, 'longitude': -87.664358, 'departures': 4837, 'latitude': 41.954177, 'capacity': 15, 'ID': 318}, {'name': 'Peoria St & Jackson Blvd', 'arrivals': 12928, 'longitude': -87.649633, 'departures': 13783, 'latitude': 41.877749, 'capacity': 19, 'ID': 134}, {'name': 'Logan Blvd & Elston Ave', 'arrivals': 4215, 'longitude': -87.684158, 'departures': 3977, 'latitude': 41.929465, 'capacity': 27, 'ID': 258}, {'name': 'California Ave & Altgeld St', 'arrivals': 3050, 'longitude': -87.697668, 'departures': 2867, 'latitude': 41.92669, 'capacity': 15, 'ID': 502}, {'name': 'Southport Ave & Clark St', 'arrivals': 2417, 'longitude': -87.664199, 'departures': 2714, 'latitude': 41.957081, 'capacity': 11, 'ID': 292}, {'name': 'Shields Ave & 28th Pl', 'arrivals': 1131, 'longitude': -87.635491, 'departures': 1030, 'latitude': 41.842733, 'capacity': 11, 'ID': 401}, {'name': 'Southport Ave & Waveland Ave', 'arrivals': 10349, 'longitude': -87.66394, 'departures': 10228, 'latitude': 41.94815, 'capacity': 23, 'ID': 227}, {'name': 'Austin Blvd & Madison St', 'arrivals': 110, 'longitude': -87.774453, 'departures': 150, 'latitude': 41.880281, 'capacity': 11, 'ID': 544}, {'name': 'Kedzie Ave & Milwaukee Ave', 'arrivals': 10213, 'longitude': -87.707857, 'departures': 9460, 'latitude': 41.929567, 'capacity': 35, 'ID': 260}, {'name': 'Greenview Ave & Diversey Pkwy', 'arrivals': 3743, 'longitude': -87.665939, 'departures': 3689, 'latitude': 41.932595, 'capacity': 15, 'ID': 319}, {'name': 'Field Museum', 'arrivals': 7294, 'longitude': -87.617867, 'departures': 9184, 'latitude': 41.865312, 'capacity': 55, 'ID': 97}, {'name': 'Western Ave & 24th St', 'arrivals': 627, 'longitude': -87.685109, 'departures': 599, 'latitude': 41.84847, 'capacity': 7, 'ID': 281}]
```

-----

##### Complete Dataset

Data for [all 622 Divvy stations](data.txt) is also available. Make sure you click 'raw' on the next screen to see and copy all of it.
