##### Week 09 TOC
- [Introduction](readme.md)
- [Abstract Bikeshare Model](abstract_model.md)
- [Real Data Bikeshare Model](data_model.md)
- [Homework for Next Week](homework.md)

-----

### Modeling a Bikeshare System

This week, let's attempt to computationally recreate, simulate, evaluate, and forecast an urban bikeshare system. We will do this through the creation of several, increasingly sophisticated Python models. Our models will evaluate performance at both the *agent level* as well as the *system level*. We will also evaluate how accurately our model matches past data, in order to test its ability to diagnose and predict future system behaviors.

Our model will simulate riders selecting a bike from a departure station, traveling through the city, and returning the bike to an arrival station — and we will also strive to determine how each station might be perceived by these simulated bikeshare riders through a `happiness` metric, both at the station and system level. 

The fanciest tactic we will leverage is an algorithm called *weighted random choice*, often used by statisticians to simulate [stochastic](https://en.wikipedia.org/wiki/Stochastic) agent choice in complex systems. Constrained randomness (is that an oxymoron?) is a complicated topic, but it is important that we embody a tacit assumption: that true randomness is inhuman. We have [no ability to produce real randomness](http://people.ischool.berkeley.edu/~nick/aaronson-oracle/index.html), and we can not even [prove whether or not any series of numbers is truly random](https://en.wikipedia.org/wiki/Pseudorandomness). Likewise, [no computational tool can be truly random](https://en.wikipedia.org/wiki/Pseudorandom_number_generator).

Yet, though we can't generate randomness, the world exhibits the appearance of bounded randomness everywhere. And, as designers, we often need to confront and probe that randomness as a modelable and definable input into our design research process. Users behave randomly in many situations, they are sized and shaped in random combinations, they live in random locales and have unpredictable reactions to stimuli... We often, fundamental to our design discipline, are tasked with probing this randomness and tamping its complexity down into a set of digestable patterns or categories for our clients, our partners, and ourselves. This is our job!

Because computational modeling is fundamentally driven by the productive friction between a controlling algorithm, a constraining or guiding dataset, and an uncontrollable set of (simulated) inputs — intelligently and intentionally engaging randomness is a powerful tool. 

-----

For this exercise, we will be invoking public data provided by Divvy, a bikeshare system that provides the Chicago metro area's ~9,533,040 citizens with around ~6,000 sharable bikes distributed through ~600 stations.

![divvy](https://www.firebellydesign.com/uploads/work/divvy/b0_0__01Divvy.jpg)
