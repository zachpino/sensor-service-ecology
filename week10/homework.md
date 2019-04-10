##### Week 09 TOC
- [Introduction](readme.md)
- [Abstract Bikeshare Model](abstract_model.md)
- [Real Data Bikeshare Model](data_model.md)
- [Homework for Next Week](homework.md)

-----

### Some Additions to the Model

As a *thought* exercise, consider the following questions related to this week's data model.

- What would be needed for our model take into account weekday/weekend cycles, the calendar and holidays, seasonality, construction/development/other transit obstacles, and weather?

- The simulation, as we built it this week, understands the bikeshare from the *system* and *station* point of view. In this way, it is an example of a *top->down* data model, which evaluates the successes and failures of a system as expressed in overall performativeness. The simulation does not consider the behaviors, parameters, and preferences of *individuals*; but rather simulates and analyzes the impact of those organisms (heavily simplified and randomized!) on the Divvy ecosystem as a whole. Could you imagine a different model that tries to understand the Divvy system from the *bottom->up* instead? What might the structure of the dataset required to do that look like? Could you write structural pseudo-code?

- What other datasets could we bring into this simulation to derive new and actionable insights?

- The simulation takes longer and longer to process trips as the simulation runs. What are some of the reasons for this?

-----

As a *coding* exercise, try to add the following system behaviors. 

- Could we add trucks to move bikes around, which aim to rebalance the bikes available at all the stations?

- How could we limit rides to a certain simulated and configurable radius and prevent departure and arrival stations from being too far away? For example, Divvy's 45 minute riding limitation (before additional fees kick-in) heavily constrains how far riders within the system travel. You will likely need a [distance calculation](https://www.w3resource.com/python-exercises/python-basic-exercise-40.php) for this!

- How could we add a [chaos monkey](https://en.wikipedia.org/wiki/Chaos_engineering#Chaos_Monkey) to our simulated system — code that randomly breaks and repairs existing docks — to simulate inevitable wear and tear on the Divvy infrastructure?

- How could we create new stations randomly and dynamically while the simulation is running? How about evaluating the results of the simulation after intentionally adding new stations? Or disabling a certain set of existing stations?

We'll talk through these additions in brief next week. Give it a try!