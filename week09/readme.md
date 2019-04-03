##### Week 09 TOC
- [Homework Answers from Last Week](answers.md)
- [Introduction](readme.md)
- [Abstract Bikeshare Model](abstract_model.md)
- [Real Data Bikeshare Model](data_model.md)
- [Homework for Next Week](homework.md)

-----

### Modeling a Bikeshare System

This week, let's attempt to computationally recreate, simulate, evaluate, and forecast an urban bikeshare system. We will do this through the creation of several, increasingly sophisticated Python models. Our models will evaluate performance at both the *agent level* as well as the *system level*. We will also evaluate how accurately our model matches past data, in order to test its ability to simulate the future.

Our model will simulate riders selecting a bike from a departure station, traveling through the city, and returning the bike to an arrival station — and we will also strive to determine how each station might be perceived by these simulated bikeshare riders through a `happiness` metric. 

The fanciest tactic we will leverage is an algorithm called *weighted random choice*, often used by statisticians to simulate agent choice in complex systems.

In particular, we will be invoking public data provided by Divvy, a bikeshare system that provides the Chicago metro area's ~9,533,040 citizens with around ~6,000 sharable bikes distributed through ~600 stations.

![divvy](https://www.firebellydesign.com/uploads/work/divvy/b0_0__01Divvy.jpg)
