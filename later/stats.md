### Statistical Basics

To shore up statistical knowledge, it's helpful to have precise language to describe emergent phenomena in a population. The world of statistics has come up with an incredible diversity of mathematical models and projection logics for describing patterns in data. Here are some basics.

-----

Use [this page](https://hackernoon.com/quick-intro-to-statistics-power-your-stories-with-data-a3a35785692b) for more information and additional examples.

Additionally, [this article](https://towardsdatascience.com/how-to-properly-tell-a-story-with-data-and-common-pitfalls-to-avoid-317d8817e0c9) descibes common pitfalls of data-driven storytelling, as does this [Guardian story](https://www.theguardian.com/science/2016/jul/17/politicians-dodgy-statistics-tricks-guide?imm_mid=0e667a&cmp=em-data-na-na-newsltr_20160803).

-----

### Statistical Definitions

- Population: The overall set of things to measure. For example, the population of Illinois.

- Sample: The available and collected data. Perhaps we can only interview 500 people in Illinois, though we want to make predictions based on that for all of Illinois.

- Sample Size: How many datapoints there are in the sample.

- Sample Parameter: A number between 0 and 1 that descibes what percentage of the population is available in the sample.

- Range: The number of possible values between the lowest and highest reported values in a dataset.

-----

### Types of Statistical Data

- Raw Values: The actual recorded datapoint

- Percentage: A datapoint that is measured partitively, indexed to a max of 100.

- Parameter: A datapoint that is measured partitively, indexed to a minimum of 0 and a max of 1.

-----

### Different Ways to Define Expectation

- Mean: Sometimes called the *average*, the mean is just all the data points in a sample summed and divided by the count of sample. This approximates an *expected convergence point*. All else being equal, the mean of a datapoint is where we should expect any given additional data point to trend towards.

```js 
d3.mean(dataset, function(d){return d.comparisonData})
```

- Mode: The mode of a datapoint is the exact value that occurs the most. If any given mode is dominant, we should expect additional datapoints to trend towards the mean, while repeating the mode as often as possible.
```js
var mode = function mode(arr) {
    var numMapping = {};
    var greatestFreq = 0;
    var mode;
    arr.forEach(function findMode(number) {
        numMapping[number] = (numMapping[number] || 0) + 1;

        if (greatestFreq < numMapping[number]) {
            greatestFreq = numMapping[number];
            mode = number;
        }
    });
    return +mode;
}
```

- Median: If we take the range of the datapoints, the median is the exact middle number. If we looked at a *different population or sample* and had no other information, we should expect that the new datapoints would trend towards the median.

```js
d3.median(dataset, function(d){return d.comparisonData})
```

- Maximum: The highest occuring value

```js
d3.max(dataset, function(d){return d.comparisonData})
```

- Minimum: The lowest occuring value

```js
d3.min(dataset, function(d){return d.comparisonData})
```

- Extents: The highest and lowest occuring values

```js
d3.extent(dataset, function(d){return d.comparisonData})
```

-----

### Dependency

- Independent Variable: A datapoint that moves entirely separate from another, often a (universal) constant. The passage of time, for example, isn't impacted by anything else. If you are looking at population dynamics, independent variables might include standing public policies or locations of static community resources.

- Dependent Variable: A datapoint that moves in accordance with others. Demographic characteristics, especially those indexed to place or time, are nearly always dependent variables since people can *move locales* at *any time* for *any reason*.

- Positive Correlation: As one variable in a dataset increases, another is observed to increase alongside it *proportionally*.

- Negative Correlation: As one variable in a dataset increases, another is observed to decrease alongside it *proportionally*.

- Causation: One variable in a dataset direcly causes an increase or decrease in another. These movements could be linear, exponential, logarithmic, hyperbolic, etc. But, if a connection is truly causal, the relationship should be *determinisitic*, given a new datapoint in one causally connected dataset, we should be able to predict with certainty how it will manifest in the other.

- Regression: The linear or single degree of curvature line that best defines the relationship between two variables in a sample.

- [Pearson R Value](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient), called PCC or *rho*: A measure, between -1 and 1, that reflexts how correlated a sample is with regards to two variables. -1 is total negative correlation, and 1 is total positive correlation. Visit this [great interactive tool](http://rpsychologist.com/d3/correlation/) to experiment with Pearson values.

```js
function getPearsonCorrelation(x, y) {
    var shortestArrayLength = 0;
     
    if(x.length == y.length) {
        shortestArrayLength = x.length;
    } else if(x.length > y.length) {
        shortestArrayLength = y.length;
        console.error('x has more items in it, the last ' + (x.length - shortestArrayLength) + ' item(s) will be ignored');
    } else {
        shortestArrayLength = x.length;
        console.error('y has more items in it, the last ' + (y.length - shortestArrayLength) + ' item(s) will be ignored');
    }
  
    var xy = [];
    var x2 = [];
    var y2 = [];
  
    for(var i=0; i<shortestArrayLength; i++) {
        xy.push(x[i] * y[i]);
        x2.push(x[i] * x[i]);
        y2.push(y[i] * y[i]);
    }
  
    var sum_x = 0;
    var sum_y = 0;
    var sum_xy = 0;
    var sum_x2 = 0;
    var sum_y2 = 0;
  
    for(var i=0; i< shortestArrayLength; i++) {
        sum_x += x[i];
        sum_y += y[i];
        sum_xy += xy[i];
        sum_x2 += x2[i];
        sum_y2 += y2[i];
    }
  
    var step1 = (shortestArrayLength * sum_xy) - (sum_x * sum_y);
    var step2 = (shortestArrayLength * sum_x2) - (sum_x * sum_x);
    var step3 = (shortestArrayLength * sum_y2) - (sum_y * sum_y);
    var step4 = Math.sqrt(step2 * step3);
    var answer = step1 / step4;
  
    return answer;
 }
 ``` 

-----

### Variation

- [Variance](https://en.wikipedia.org/wiki/Variance): A unitless measure of how much variation there is in a datapoint from the average.
```js
d3.variance(dataset, function(d){return d.comparisonData})
```

- Valence: A unitless measure of which direction a datapoint would be expected to trend through time or some other independent variable. *The valence of health insurance in America is towards covered care*.



-----

Now, we can [explore some real data](stats-exercise.md) with these tools.
