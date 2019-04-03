##### Week 09 TOC
- [Homework Answers from Last Week](answers.md)
- [Introduction](readme.md)
- [Abstract Bikeshare Model](abstract_model.md)
- [Real Data Bikeshare Model](data_model.md)
- [Homework for Next Week](homework.md)

-----

### Python Bikeshare Model

Let's put everything together and simulate a simple bikeshare in Python. 

Here, we are abstracting a lot of the details of how a bikeshare system actually works, and using totally fake data. For instance, this code does nothing to keep the number of bikes in the system consistent through simulation steps!

```python
import random #bring in random number library functionality

# list of dictionaries describing bike share stations
# the keys are station "name", "in" likelihood, "out" likelihood, current "bikes", total bike "capacity", "unhappy" customer count, and "happy" customer count
stations = [{"name":"Station A", "in":.25, "out":.55, "bikes":6, "capacity":12, "unhappy":0, "happy":0}, {"name":"Station B", "in":.55, "out":.25, "bikes":4, "capacity":8, "unhappy":0, "happy":0}, {"name":"Station C", "in":1, "out":1, "bikes":6, "capacity":14, "unhappy":0, "happy":0}]

# reusable function for a bike leaving a station
def bikeDepart(station) :
	# check if there is a bike available to depart
	if station["bikes"] > 0 :
			# yes, we have a bike!
			# take away a bike and increase happiness score
			station["bikes"] -= 1
			station["happy"] += 1
	else :
			# no bike, unhappy customer
			station["unhappy"] += 1

# reusable function for a bike arriving at a station
def bikeArrive(station) :
	# check if there is a dock available
	if station["bikes"] < station["capacity"] :
		# yes, we have an available dock!
		# add a bike to the station and increase happiness score
		station["bikes"] += 1
		station["happy"] += 1
	else :
		# no dock, unhappy customer
		station["unhappy"] += 1

# reusable function to advance our simulation by one evaluation per station
def step():
	# loop though all stations
	for i in range( len(stations) ) : 
		
		# generate a random number
		departFlip = random.uniform(0, 1)			
		# use station departiure probability to determine if a bike left the station this simulation step
		if departFlip < stations[i]["out"] :
			#run bikeDepart function on current station 
			bikeDepart(stations[i])
			print("bike departed from  " + stations[i]["name"])

		# generate a second random number
		arriveFlip = random.uniform(0, 1)
		# use station arrival probability to determine if a bike arrived at the station this simulation step
		if arriveFlip < stations[i]["in"] : 
			# run bikeArrive function on current station 
			bikeArrive(stations[i])
			print("bike arrived at " + stations[i]["name"] )

# reusable function to simulate the system
def simulate(count):
	# run as many simulation steps as requested
	for i in range (count):
		print("simulation step " + str(i))
		step()
	# after simulation, print the results
	for i in range( len(stations) ) :
		print(stations[i]["name"] + " had " + str(stations[i]["happy"]) + " happy riders and " + str(stations[i]["unhappy"]) + " unhappy riders.")

# run the simulation!
simulate(100)
```