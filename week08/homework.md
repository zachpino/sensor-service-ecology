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
7. Add a *sharing differential* (`arrivals` - `departures`) key to each station dictionary. What is the average sharing differential? What are the max and min sharing differentials? How might these numbers be useful to Divvy system planners?
8. Add a *sharing ratio* (`arrivals` / `departures`) key to each station dictionary (beware that you should coerce both arrivals and departures into floats to prevent integer rounding!). What is the average sharing ratio? What are the max and min sharing ratios? How might these numbers be useful to Divvy system planners?

You can take a small subset of the data to work on (data for 50 Divvy stations), or chew through all of 622 Divvy stations!

-----

##### Partial Dataset

Copy and paste the data below for 50 Divvy stations, chosen randomly with `random.randint()`. Use this to test your code, as the complete dataset can be unwieldly.

```python
[{'name': 'Eberhart Ave & 61st St', 'arrivals': 222, 'longitude': -87.6133078304, 'departures': 210, 'latitude': 41.78414169317, 'capacity': 11, 'ID': 431},
{'name': 'Wabash Ave & Grand Ave', 'arrivals': 21357, 'longitude': -87.626761, 'departures': 21029, 'latitude': 41.891466, 'capacity': 31, 'ID': 199},
{'name': 'Clark St & Jarvis Ave', 'arrivals': 763, 'longitude': -87.675005, 'departures': 782, 'latitude': 42.015963, 'capacity': 11, 'ID': 517},
{'name': 'Wood St & Milwaukee Ave', 'arrivals': 10799, 'longitude': -87.672552, 'departures': 10332, 'latitude': 41.907655, 'capacity': 19, 'ID': 61},
{'name': 'May St & 69th St', 'arrivals': 121, 'longitude': -87.652934, 'departures': 123, 'latitude': 41.768938, 'capacity': 11, 'ID': 567},
{'name': 'Lincoln Ave & Waveland Ave', 'arrivals': 3872, 'longitude': -87.675278, 'departures': 3467, 'latitude': 41.948797, 'capacity': 15, 'ID': 257},
{'name': 'Lincoln Ave & Roscoe St', 'arrivals': 9200, 'longitude': -87.67097, 'departures': 8199, 'latitude': 41.94334, 'capacity': 19, 'ID': 230},
{'name': 'Morgan St & 18th St', 'arrivals': 4550, 'longitude': -87.651073, 'departures': 3959, 'latitude': 41.858086, 'capacity': 15, 'ID': 14},
{'name': 'Damen Ave & Chicago Ave', 'arrivals': 11040, 'longitude': -87.67722, 'departures': 9847, 'latitude': 41.895769, 'capacity': 19, 'ID': 128},
{'name': 'Ashland Ave & Grand Ave', 'arrivals': 4276, 'longitude': -87.666611, 'departures': 3970, 'latitude': 41.891072, 'capacity': 15, 'ID': 277},
{'name': 'Marshfield Ave & Cortland St', 'arrivals': 14871, 'longitude': -87.668879, 'departures': 15413, 'latitude': 41.916017, 'capacity': 23, 'ID': 58},
{'name': 'Halsted St & Archer Ave', 'arrivals': 1463, 'longitude': -87.646795, 'departures': 1553, 'latitude': 41.847203, 'capacity': 15, 'ID': 206},
{'name': 'Broadway & Cornelia Ave', 'arrivals': 11370, 'longitude': -87.646439, 'departures': 11231, 'latitude': 41.945529, 'capacity': 23, 'ID': 303},
{'name': 'Kedzie Ave & 24th St', 'arrivals': 197, 'longitude': -87.7054137458, 'departures': 224, 'latitude': 41.84819094491, 'capacity': 11, 'ID': 441},
{'name': 'Laramie Ave & Kinzie St', 'arrivals': 82, 'longitude': -87.755527, 'departures': 65, 'latitude': 41.887832, 'capacity': 11, 'ID': 530},
{'name': 'Elizabeth St & 47th St', 'arrivals': 165, 'longitude': -87.656526, 'departures': 156, 'latitude': 41.80839, 'capacity': 11, 'ID': 553},
{'name': 'Kimbark Ave & 53rd St', 'arrivals': 8017, 'longitude': -87.594747, 'departures': 7704, 'latitude': 41.799568, 'capacity': 19, 'ID': 322},
{'name': 'Damen Ave & Pershing Rd', 'arrivals': 222, 'longitude': -87.676597, 'departures': 200, 'latitude': 41.823192, 'capacity': 11, 'ID': 546},
{'name': 'Morgan St & Pershing Rd', 'arrivals': 59, 'longitude': -87.650931, 'departures': 61, 'latitude': 41.823613, 'capacity': 7, 'ID': 548},
{'name': 'California Ave & Division St', 'arrivals': 2242, 'longitude': -87.697474, 'departures': 2024, 'latitude': 41.903029, 'capacity': 15, 'ID': 216}]
```

-----

##### Complete Dataset

Data for [all 622 Divvy stations](data.txt) is also available. Make sure you click 'raw' on the next screen to see and copy all of it.
