### Accessing the Census API

#### First Steps

Download and install [Sublime](https://www.sublimetext.com/), and request a [Census API Key](https://api.census.gov/data/key_signup.html).

#### What is the Census?

The [US Census](https://en.wikipedia.org/wiki/United_States_Census) — an extraordinary resource for data scientists, policymakers, and social science researchers — is an attempt to collect information on every single person living within the boundaries of sovereign United States territory every ten years. It is called for by the Constitution of the United States, and has never been missed (even taking place during the United States Civil War ). Not only is the census vital for a better understanding of the makeup and condition of the United States, but it also serves as the official mechanism for drawing congressional and state goverment district boundaries — rendering the census a vital component of the United States political mechanism. Thousands of taxpayer-supported census workers campus the country, knock on doors, collect data, and ensure as accurate a count as possible through some [functionally questionable instruments](https://www.census.gov/2010census/pdf/2010_Questionnaire_Info.pdf).

Though an effort is made to deliver a wholistic and objective count with financial punishment for those who do not complete the census form ($100/household in 2010) and a promise of total confidentiality and indemnity, participation in the census is estimated at 74%, with some populations statistically underreporting. For instance, undocumented immigrant families are less likely to participate for fear of governmental reprisal. This skew has been modeled, and serves as a statistical adjustment in some of the census datasets which do not report raw numbers. Most census datasets, however, report nothing more than pure integer counts, and contextual offsets are left up to the data modeler. 

Modeled on an Ancient Roman equivalent that tracked citizenry for military draft purposes (*censore* in Latin and its derivative in most romance languages means *to determine value of something*), the success of the US census has served as a model for similar efforts across the globe and supported by the United Nations. Many countries complete their own censuses, and provide them to citizens in a similar way to the United States model. 


#### Criticisms and Limitations of the Census

> Representatives and direct Taxes shall be apportioned among the several States which may be included within this Union, according to their respective Numbers, which shall be determined by adding to the whole Number of free Persons, including those bound to Service for a Term of Years, and excluding Indians not taxed, three fifths of all other Persons. The actual Enumeration shall be made within three Years after the first Meeting of the Congress of the United States, and within every subsequent Term of ten Years, in such Manner as they shall by Law direct. The Number of Representatives shall not exceed one for every thirty Thousand, but each State shall have at Least one Representative; and until such enumeration shall be made, the State of New Hampshire shall be entitled to chuse three, Massachusetts eight, Rhode-Island and Providence Plantations one, Connecticut five, New-York six, New Jersey four, Pennsylvania eight, Delaware one, Maryland six, Virginia ten, North Carolina five, South Carolina five, and Georgia three.

As is obvious from this Article 2 US Constitutional mandate, the incredible resource that is the census is definitively tarnished by historical realities. The censuses of the early United States did not count women, people of color, or indigineous peoples equitably, until the landmark census of 1850 — which was administered by the ahead-of-his-time sociopoltical activist and Civil War cartographer and strategist [Joseph C.G. Kennedy](https://en.wikipedia.org/wiki/Joseph_C._G._Kennedy). There were regressions later, however — for example the census of 1940 was used surreptitiously to identify potential Italian, German, and Japanese sympathizers for illegal incarceration. Because the census strives to offer consistent data over time, it often uses non-contemporary terminology and selection biases. For instance, the census still maintains binary gender definitions, counts same-sex marriages in some states and not others, and mixes up racial and ethic identity, Counting people, inherently, requires the drawing of lines between people — and that is often done by the census in unenlighted, conservative ways — with consistency as the ultimate goal.

Moreover, because of the role of the census in determining House of Representative seat counts, census data collection is often politicized. The 2020 census is already being accused of potential misuse mostly surrounding the counting of undocumented immigrants, and despite its importance, is under threat of defunding by both political sides. Additionally, contemporary gross population trends are much more dynamic than the every 10 year original model designed for the on-horseback census worker. As a result, contemporary census data collection efforts have expanded.


#### American Community Survey
Since the early aughts, the 10 year cadence of the census — clearly an outdated model in an era of constant data production and collection — has been supported by a motley assortment of supplemental data gathering efforts. One of the more successful attempts has been the American Community Survey, which aims to provide census-level details of a statistical sampling of the full American population at a higher frequency for large population centers. The ACS occurs on an ongoing basis through the calendar year with a (relatively) small sample size (population centers of 65,000 individuals or greater), and is also tallied every five years with a larger sampling (population centers of 20,000 individuals or greater).

The ACS tracks around 60,000 *variables*, and provides projections and estimates for another 20,000 or so. The data is voluminous, to say the least. Many of these indicators are aimed at constructing a picture of the United States' identity makeup — gender, race, ethnicity, derivation, age. Others are financial — income, employment status, employment industry, labor force participation. Further indicators support a better understanding of the American household — children/family, household size, number of rooms, rent/mortgage cost, frequency of housing changes, languages spoken at home. A large proportion of these variables have been tracked in the same way over many years, allowing data scientists and visualizers to trace and understand the increasingly tumultuous social dynamics of the United States.

This course will provide walkthroughs for accessing this ACS data, cleaning and transforming it, combining it with other datasets, finding insights, using statistical tools for proving those insights, and rendering it visual through the D3 data visualization library.


----------


#### Census API

The ACS are available online, along with an increasing number of historical census datasets and other data repositories, as an Application Programming Interface (API).  

	> An API is a formalized way of accessing complex and/or voluminous data-stores. Making a query to an API is termed 'calling' an API.

Some terminology: 

	> Data Store : Repository (physical of virtual) where data is located.
	> Data Set : An organized collection of related information
	> Data Point : A singular piece of encoded information

The Census API, a *data store*, offers a *data set* on uninsured people in each state, wherein we can find the *data point* for Illinois in 2016, which had an uninsured population of 71,319 people under the age of 18.

Through the use of the Census API, very specific queries can be generated to access information at different geographic resolutions — from whole country tabulations to data bucketed for each state to county- and block-level raw counts.

The [Census API homepage](https://census.gov/developers/) offers a good sense of what is avaiable, though highly specific language is often used.

The [available APIs](https://census.gov/data/developers/data-sets.html) are also enumerated and you can find the ACS there, and [many exploratory tools](https://census.gov/data/data-tools.html) are provided.


#### API Key

The first step to accessing most APIs is to acquire an API cryptographic key.

	> A cryptographic key is a long, alphanumeric, random string that uniquely identifies each user.

Despite census data being fully public, API access is gated to prevent abuse and to anonymously track developers who access the repository. This tracking is benevolent, and helps the maintainers of the API know what is interesting to developers, and where their queries fail unexpectedly.

[Request an API Key](https://api.census.gov/data/key_signup.html), which will be sent to the email address you provide within 15 minutes or so. It may go to your spam folder.

Keep this 40 character string to yourself, and never share it online. Doing so could allow someone else to masquerade as you and hammer the Census API servers. Make sure you copy the string somewhere so that it will be easily accessible. Each API Key is limited to 500 requests per day.


#### Browsing the API Data

The [yearly ACS API](https://census.gov/data/developers/data-sets/acs-1year.html) offers many different way of learning about the data available within. The page lays out four main, confusingly named presentations of the underlying data.

	> Detail Tables : Actual raw datapoints, mostly pure counts, the most data at the highest specificity
	> Subject Tables : The raw data points scaled up and converted into population percentages and averages
	> Data Profile : Percentages and estimations of larger statistical trends derived from the data points
	> Comparison Profile: Shows changes over time for specific data points

The querying format is different for each type of presentation, though the process is similar. 

Underneath each presentation is an set of resources that looks something like this:

##### Detail Tables
- Example Call: api.census.gov/data/2016/acs/acs1?get=NAME,B01001_001E&for=state:*&key=...
- 2016 ACS Detail Table Variables [ html | xml | json ]
- ACS Technical Documentation
- Examples and Supported Geography

Selecting the [html](https://api.census.gov/data/2016/acs/acs1/variables.html) link will display a webpage listing all of the data dimensions available and is the best place to start browsing. On that linked page thousands of rows will display exactly what is avaiable in the ACS.

| Name        | Label           | Concept  | Required | Attributes | Limit | Predicate Type | Group | Valid Value |
| :------     |:-------------   | :-----   | :------- |:---------  | :-----| :------------- |:------| :-----------|
|   C15010_001E   |   Estimate!!Total   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER   |   not required   |   C15010_001M, C15010_001EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_002E   |   Estimate!!Total!!Science and Engineering   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_002M, C15010_002EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_003E   |   Estimate!!Total!!Science and Engineering Related Fields   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_003M, C15010_003EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_004E   |   Estimate!!Total!!Business   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_004M, C15010_004EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_005E   |   Estimate!!Total!!Education   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_005M, C15010_005EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_006E   |   Estimate!!Total!!Arts, Humanities and Other   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_006M, C15010_006EA   |   0   |   int   |   C15010   |   N/A   |


All of the columns hold important information.

- Name : The unique identifier for each data set. This is necessary for calling an API.
- Label : The unique, human readable name for each data set. Most ACS fields will display 'Estimate' since not everyone is being counted.
- Concept : The overall idea of what is being tracked within the dataset, usually understandable.
- Required : If some specific variable was required in order to make a query resolive, it would be marked as such here.
- Attributes : The name + 'M' is an accounting for the margin of error data point for the data set. The name + "EA" is any statement available about how the estimate or average was calculated.
- Limit : If there was a limit on the number of respondents. 0 means 'no limit.'
- Predicate Type : What kind of data is in the data set. Most ACS fields are 'int,' integers, or 'strings,' textual descriptions. 
- Group : Most data sets belong to a group of related datasets.
- Valid Value : Some data sets only permit certain responses. Links here describe those options.

By selecting the [Examples and Supported Geographies](https://api.census.gov/data/2016/acs/acs1.html) link it is possible to find many examples of what you can query the API for, and how. These are great resources! 


-----


#### Calling an API

Let's compose a simple query of the ACS.

For instance, we could attempt to find counts of those who live in the State of Illinois who graduated with STEM Bachelor degrees in 2016. Since we are interested in counts alone, the *Detail Tables* presentation is most appropriate. The "Example Call" descibes how to construct an API call, which looks like a standard web address.

```
api.census.gov/data/2016/acs/acs1?get=NAME,B01001_001E&for=state:*&key=...
```

To complete our call, we need some specific pieces of information.

- Name of the dataset, found above. Many can be called together, separated by commas. 
- Geographic domain of interest. Many can be called together, separated by commas. FIPS encoded.
- ACS year of interest
- API key

```
api.census.gov/data/ |YEAR| /acs/acs1?get= |NAME| &for= |GEOGRAPHY| &key= |KEY|
```

We already have many of the necessary pieces. The unique name of the data set is `C15010_003E` for the year `2016`, and we've requested an API key previously. The only thing missing is the geographic area of interest.


#### Encoded Geographies

Thousands of geographic areas are encoded with the [FIPS encoding system](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standards) and accessible for querying. Every state, county, metropolitan area, congressional district, and census tract has a unique numerical id, which can be [found here](https://www.census.gov/geo/reference/codes/cou.html). A list of [state FIPS codes](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standard_state_code) is also avaiable for easy browsing. The state of Illinois is `17`.

So, our call would look like this, make sure your substitute your API key instead!

```
api.census.gov/data/2016/acs/acs1?get=C15010_003E&for=state:17&key=a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0
```

Entering that into any web browser will return the following text.

```
[["C15010_003E","state"],
["283557","17"]]
```

The API confirms that `C15010_003E` was accessed at the `state` level. `283557` individuals have STEM related degrees in FIPS state `17`, Illinois.

We could similarly ask for all states by using the wildcard operator `*`. Again, ensure you substitute your API key.

```
api.census.gov/data/2016/acs/acs1?get=C15010_003E&for=state:*&key=a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0
```

```
[["C15010_003E","state"],
["84646","01"],
["14711","02"],
["139304","04"],
["50278","05"],
["692652","06"],
["116557","08"],
["76857","09"],
["20035","10"],
["11014","11"],
["432861","12"],
["190302","13"],
["32736","15"],
["33691","16"],
["283557","17"],
["134891","18"],
["59116","19"],
["62926","20"],
["81884","21"],
["90705","22"],
["31382","23"],
["141516","24"],
["168557","25"],
["205751","26"],
["128965","27"],
["50784","28"],
["115480","29"],
["26978","30"],
["45956","31"],
["49666","32"],
["30866","33"],
["204546","34"],
["35635","35"],
["436594","36"],
["205115","37"],
["19826","38"],
["247831","39"],
["69776","40"],
["81895","41"],
["292374","42"],
["24036","44"],
["81485","45"],
["21036","46"],
["129196","47"],
["457374","48"],
["58726","49"],
["12151","50"],
["171380","51"],
["152394","53"],
["34796","54"],
["133290","55"],
["13783","56"],
["60478","72"]]
```

Note that there 52 data points. The District of Columbia and Puerto Rico are included.

-----

#### Complex Variable Queries

We can, as mentioned previously, make a query for many variables at a time — indexed by the same geographic indicator. All named variables are separated by commas, with a total limit on 50 simultaneous requests.

```
api.census.gov/data/2016/acs/acs1/subject?get=S0101_C01_001E,S1301_C01_027E&for=state:*&key=a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0
```

Note how, in the structure of the response, all of the values assosicated with each geographic entity are included within a singular array. This makes data comparison and visualization significantly simpler.

```
[["S0101_C01_001E","S1301_C01_027E","state"],
["4863300","767186","01"],
["741894","126943","02"],
["6931071","1075161","04"],
["2988248","454419","05"],
["39250017","6460931","06"],
["5540545","971927","08"],
["3576452","619189","09"],
["952065","156352","10"],
["681170","159962","11"],
["20612439","3218671","12"],
["10310371","1774843","13"],
["1428557","232595","15"],
["1683140","254534","16"],
["12801539","2244485","17"],
["6633053","1104601","18"],
["3134693","542718","19"],
["2907289","483292","20"],
["4436974","713641","21"],
["4681666","756663","22"],
["1331479","216703","23"],
["6016447","1091523","24"],
["6811779","1252641","25"],
["9928300","1617472","26"],
["5519952","1000123","27"],
["2988726","473194","28"],
["6093000","1021327","29"],
["1042520","166619","30"],
["1907116","331800","31"],
["2940058","493437","32"],
["1334795","228046","33"],
["8944469","1483120","34"],
["2081015","311860","35"],
["19745289","3365361","36"],
["10146788","1707687","37"],
["757953","133009","38"],
["11614373","1930498","39"],
["3923561","614422","40"],
["4093465","680386","41"],
["12784227","2085198","42"],
["1056426","183760","44"],
["4961119","827826","45"],
["865454","142503","46"],
["6651194","1074159","47"],
["27862596","4572528","48"],
["3051217","516699","49"],
["624594","105764","50"],
["8411808","1463542","51"],
["7288000","1204063","53"],
["1831102","260352","54"],
["5778709","1015308","55"],
["585501","95244","56"],
["3411307","464671","72"]]
```

-----


#### Complex Geographic Queries

The same logic applies to multiple geographic entitities. For instance, we can ask for specific states like Illinois, Wisconsin, and Michigan — all separated by commas.

```
api.census.gov/data/2016/acs/acs1/subject?get=S0101_C01_001E,S1301_C01_027E&for=state:17,55,26&key=a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0
```

This will return a subset of the result above.

```
[["S0101_C01_001E","S1301_C01_027E","state"],
["12801539","2244485","17"],
["9928300","1617472","26"],
["5778709","1015308","55"]]
```

We also can use the `in` keyword to include smaller geographic entities constrained by larger ones. For example, perhaps we could ask the API for data for all of the congressional districts in Illinois. Note that `congressional district` has its space replaced by `%20`, which is the HTML encoding for a space. All spaces found in supported geographies will be similarly replaced.

```
api.census.gov/data/2016/acs/acs1/subject?get=S0101_C01_001E,S1301_C01_027E&for=congressional%20district:*&in=state:17&key=a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0
```

The results will include all 18 congressional districts that Illinois currently holds. Illinois will likely lose a congressional seat as a result of the 2020 census, so be careful when comparing this kind of flexible geographic determiner over time.

```
[["S0101_C01_001E","S1301_C01_027E","state","congressional district"],
["691474","123193","17","01"],
["713917","129478","17","02"],
["720930","111337","17","03"],
["695098","129652","17","04"],
["723373","163307","17","05"],
["722323","117611","17","06"],
["723481","155457","17","07"],
["717789","122853","17","08"],
["724071","118239","17","09"],
["710610","117834","17","10"],
["730480","135068","17","11"],
["693736","114221","17","12"],
["704699","124485","17","13"],
["736397","128187","17","14"],
["700549","108530","17","15"],
["687998","116278","17","16"],
["691212","112978","17","17"],
["713402","115777","17","18"]]
```

Be sure to explore the many other examples available in the 'Examples and Supported Geographies' links found under each data presentation on the [ACS page](https://census.gov/data/developers/data-sets/acs-1year.html).

Also, note that all geographies are *FIPS encoded*. Here are some resources to map names to FIPS codes.
- [States and Regions](state-regions-fips.xls)
- [Counties](counties-fips.xls)
- [Metropolitan, Micropolitan, and Combined Statistical Areas](metropolitan-csa-fips.xls)

-----

#### Saving Results and Data Structure

The arrays that result from a census query can be copied and pasted elsehwere from your browser, or alternatively, saved into a file. By default, your browser will likely append the `.json` extension, short for 'Javascript Object Notation.' The census uses a nonstandard form of JSON in order to be compatible with spreadsheet programs like Microsoft Excel or Apple Numbers. Once you have downloaded the file, changing the extension to `.csv`, 'Comma-Separated Values,' will allow spreadsheet programs to read and manipulate the queried data in a more familiar context. However, as you spend time exploring data without the crutch of spreadsheet programs, your ability to identify structures and find flaws and inconsistencies within datasets will quickly improve and ease your modeling work down the line.

As has been visible throughout the queries above, the data that is returned from the census takes on the form of a 2-dimensional matrix.

```
[
	["value","geoID"],
	["value","geoID"],
	["value","geoID"]
]
```

A query returns a single *parent* array (the top and bottom square brackets). Within that array, each geographic *child* entity is its own array in the matrix, and each requested data point is contained in that array. Often, towards computational modeling goals, this data model is too simplistic — and the work of the data modeler often is to match, manipulate, and reconstruct the data in order to serve the intended presentations. This is another reason to avoid using spreadsheet programs, as these software tools only permit 2-dimensional matrices and not the often complex, nested, and multidimensionally hierarchical data structures enabled by a *plain text* approach to data.

-----

#### Looking Forward and Review

You can find more information and details at the many links above, and in [this guide](https://www.census.gov/content/dam/Census/data/developers/api-user-guide/api-guide.pdf) produced by the census development team — it's a highly recommended read.

Continue working on your CTA brief, and use census data as a potential shortcut to some demographic insights. Use [this tool](census_cta.zip) as explored in class to visualize Chicago census tracts.

If that folder is **unzipped to your desktop**, use these commands in Terminal to launch the tool.

```

cd ~/Desktop/census_cta/
python -m SimpleHTTPServer 8080

```

Then, visit `http://localhost:8080` in your browser of choice.
