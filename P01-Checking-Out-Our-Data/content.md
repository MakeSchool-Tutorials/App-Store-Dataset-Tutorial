### Welcome to Part 1 of the tutorial series on basic exploratory data science! 

> NOTE: Explicitly point out the difference between data science tutorials and other web/app tutorials. 

This tutorial is geared towards introductory data science students at the Make School Product College. 

You should have the following prerequisite skills:

- Basic Python Development Skills
- Basic/Intermediate App Development Skills
- Basic Communication and Presentation Skills

You may realize as this course goes on that it does not have the same continuous project-based approach as many other Product College courses.

That's because this course is best taught by working through dataset after dataset. 

While you'll be able to apply data science skills into any app you build, it is not directly project-focused. 

---

For this tutorial, navigate to the `notebooks/ ` folder and create a new Jupyter notebook file.

In this course, you'll be working primarily with **Jupyter Notebooks**, a type of interactive Python simulations where you'll be able to easily manipulate, visualize, and model data. 

<br>

### **EDA: Mobile App Data**

In this tutorial, we'll start you off with a nod towards our mobile developers and roots as a mobile game academy by exploring a **curated dataset on over 10k mobile apps** on the app store. 

Consider the following questions as you work through the tutorial:
- What's the average byte size of mobile apps?
- How many apps are free vs. paid?
- How many mobile apps are games and what is the general categorical distribution of apps by genre?
- What are the most popular mobile games by total popular rating?

As you complete the tutorial, **write down answers you find to these questions** and be sure to ask your own! 

A strong part of data science is being able to formulate critical questions based off of observations you see in unknown data. 


---

## <center>Introduction</center>

Welcome to the first tutorial on Exploratory Data Analysis! 

In this tutorial, we'll quickly explore a simple dataset designed to cultivate your interest in extracting unique insights from data. 

As many of you are mobile developers, we'll be exploring the [App Store Mobile Apps](https://www.kaggle.com/ramamet4/app-store-apple-data-set-10k-apps/version/2) dataset from Kaggle! No need to download or set anything up; if you followed the setup instructions correctly, you should have the dataset already downloaded in your local directory! 

We'll explore trends among mobile app data, including **most popular mobile apps/games**, **top rated games**, and **free vs. paid mobile apps**. 

### Before moving any further, ensure you have the following prerequisites met:
- Anaconda Navigator installed and running successfully.
- Jupyter works successfully.
- Python 3.6+ installed and working.


Open up a Jupyter Notebook (See Tutorial 0 (link here) for setup instructions.) and <u>enter the following into the first cell</u> to set up our dependencies needed for basic data analysis and visualization.

```py
# Pandas is a library for basic data analysis
import pandas as pd

# NumPy is a library for advanced mathematical computation
import numpy as np

# MatPlotLib is a library for basic data visualization
import matplotlib.pyplot as plt

# SeaBorn is a library for advanced data visualization
import seaborn as sb
```

Here, we run some basic setup for some visualization work in SeaBorn and MatPlotLib.

Don't worry about what this all means now: simply <u>plug this into your next cell</u> for some data visualization setup.

```py
sns.set(style="white", context="notebook", palette="deep")

COLOR_COLUMNS = ["#66C2FF", "#5CD6D6", "#00CC99", "#85E085", "#FFD966", "#FFB366", "#FFB3B3", "#DAB3FF", "#C2C2D6"]

sns.set_palette(palette=COLOR_COLUMNS, n_colors=4)
```

## Setting up our DataFrame

### NOTE: We'll be quickly reviewing some basic Bash commands in Jupyter.

Here, we can use Bash commands preceded by a percentage sign (%) to check our current directory location.

```py
% ls
```

Next, we can run the following Bash command preceded by '%' to navigate to our raw data file. 

After running the next cell, run the previous cell to <i>see reflected changes in the current directory location</i>! 

```py
# NOTE: BASH COMMANDS IN JUPYTER (cont'd)

% cd ../datasets/
```

Sweet! Let's reset now.

Run the following command to navigate to the last location: the <strong>starting directory from where this notebook was launched</strong>. 

```py
% cd -
```

Here, we'll grab our file (`AppleStore.csv`) and call it as a <strong>DataFrame</strong> – a Pandas-specific array-like object that's very useful for visually representing and interpreting data in a programmatic manner.

```py
# NOTE: Must be in the /tutorials/ directory. (Not the /datasets/ directory.)

FILEPATH = "../datasets/AppleStore.csv"

df = pd.read_csv(FILEPATH, index_col="Unnamed: 0")
df
```

Your output in Jupyter should be the following:

![screenshot01-dfhead](../media/screenshot01_dfhead.png?raw=true)

Look over the data as a whole and see if you can detect any interesting trends or patterns across the data. 

Some good questions to keep in mind: 

- Any *convoluted* data?

- Any *repeated* data?

- Any *redundant* data?

- Any data that's *difficult to understand*?

All these realizations from a quick glance at the information is crucial to establish a good relationship with your data at the start. 

Your intuition will lead you to deeper questions and important revelations as you sift through more and more data in attempts to answer said questions. 

That being said, time for a quick side note...

---

### **Quick Note on Data Cleaning**:

Before we go any further, it's important to make one very important assumption regarding the cleanliness of data in this practice tutorial. 

Right now, the majority of your data science work focuses on *data analysis* and *data visualization* - not easy work, especially if your data is **unclean**. 

Unclean data is indicative of an important factor now at play: important trends or patterns that exist among your data may be *obscured* by lack of data clarity. 

This could be for a multitude of reasons, including irregularities in data types, inconsistencies in how data was recorded, or the presence of inappropriate data values such as *null values*. (Remember that term - it'll come up again next tutorial!)

It is crucial as a data scientist to always **question the data in front of you** from as many angles possible, including its validity and cleanliness. 

In this tutorial, we simply want to instill that sense of critical thinking in you, but do not wish to throw some complex data cleaning challenges at you just yet. 

However, keep in mind ways that the data in this tutorial could be potentially obscured to make your job tougher; you'll find that industry data science often spends a lot of time tackling the issue of data cleaning and processing. 

---

Now that we're done rambling, let's get back into checking out the values in our data! 

Let's take a look at the very first app in our dataset and see some of its information. 

We can see the information from the very first app using either of the following two commands:

> NOTE: Explicitly state to run this in Jupyter for each of the next few cells. 

```py
df.iloc[0]
```

```py
df.loc[1]
```

Both commands should return data for the app *PAC-MAN Premium*. 

`.iloc[ ]` and `.loc[ ]` are powerful selector tools in Pandas that allow you to view either a single or multiple rows, columns, and/or cells in a dataset. 

You may be wondering what the difference is between `.iloc[ ]` and `.loc[ ]`? 

Let's explore that using the example of the number **3**. 

When I run the following command: 

```py
df.iloc[3]
```

Pandas returns the data for the app *eBay: Best App to Buy, Sell, Save! Online Shopping*. 

> NOTE: Include image sample for .iloc[3] returning eBay data.

However, if you notice carefully, the *eBay* app appears in the **fourth position** in the original dataframe. 

**Why is this the case?**

Well, it turns out that `.iloc[ ]` is useful for selecting *data by index*. 

Since Python (like most languages), has zero-indexed arrays, the input value **3** refers to the **fourth** row of the dataset! 

Let's contrast that with the following command:

```py
df.loc[3]
```

In this case, Pandas returns the data for the app *WeatherBug - Local Weather, Radar, Apps, Alerts*. 

> NOTE: Include image sample for loc[3] returning WeatherBug data.

**Why is this the case?**

Simply put, `.loc[ ]` is useful for selecting *data by label*. 

This means that the original label of the data as defined by its leftmost column becomes the selection value. 

Therefore, when we ran `df.loc[3]`, rather than selecting the row defined by the third index, it selected the row defined by the number **3** occurring in the leftmost column, which happens to be the second index!

If this seems tricky, don't worry.

Even the most advanced Python developers and data scientists can trip up these commands. Practice makes perfect! 

These selector methods also support *slicing*. 

Try running either of the following:

```py
df.iloc[:3]
```

```py
df.loc[:3]
```

Either one should return you the first three rows appearing in the dataset - accurately sliced from the first data value, as your slice parameter indicates. 

Ask yourself the following: *why would both slicing index and label selectors return the same output*? 

---

> NOTE: Calculate average price/statistic manually to feed into *.describe( )*. 

---

By the way, those selectors seem great, but imagine trying to get general information about your entire dataset that way. 

Seems impractical, right?

Pandas agrees - that's why it offers a far better way to grab immediate descriptive statistics on *all columns* across your dataset in a highly visual manner. 

Run the following in a new cell to quickly show general column-formatted information on the dataset.

```py
df.describe()
```

Describing the dataset should display the following:

![screenshot04_dfdescribe](../media/screenshot04_dfdescribe.png?raw=true)

The `.describe()` method in Pandas is powerful, but one restriction is that without any specific keyword arguments, it describes and returns descriptive statistics on columns that **only** contain numerical data (ints, floats).

If we want to know about trends across our non-numeric data, we can revise the prior `.describe()` statement to the following:

```py
df.describe(include="O")
```

...which should display the following result:

![screenshot05_advdfdescribe](../media/screenshot05_advdfdescribe.png?raw=true)

### Notice anything interesting? 

If you look carefully, the CURRENCY column has a 'count' value of 7000 but also has a 'unique' value of 1. 

This means that every value in the CURRENCY column **is the same**! 

Looking at the data shows that the value under observation is the type of currency of all data: "USD". 

This isn't that important to us, so we can make the executive judgment to **drop that column** to reduce the size of the data. 

```py
# NOTE: Running this multiple times will throw errors.
df = df.drop("currency", axis="columns")
```

> NOTE: Build story around this with introductory question. 

In the last few cells, ever take a look at that 'size_bytes' column? 

Pretty nasty, huh? 

In fact, on our `.describe( )` cell above, we see that most data contains 'size_bytes' attributes ranging in the millions. 

Well, in the real world, we call those Megabytes and can do some quick reformatting to our DataFrame to clean that up a little.

> NOTE: Replace lambda with better (more Pythonic) operation. 

```py
df["size_Mb"] = df["size_bytes"].apply(lambda num: np.around(num / 1000000, decimals=2))
df.drop("size_bytes", axis="columns", inplace=True)
```

> NOTE: Place up near slicing. 

Before we go any further, let's take a quick peek at the top of our DataFrame to check the data for any other obvious red flags. 

You can peek at the top of a DataFrame with the following line:

```py
df.head()
```

You should see this following:

![screenshot06_dfhead](../media/screenshot06_dfhead.png?raw=true)

As you may have guessed, you can peek at the bottom of a DataFrame with the following line:

```py
df.tail()
```

You should see the following:

![screenshot07_dftail](../media/screenshot07_dftail.png?raw=true)

Doesn't this look familiar to some of the original commands we played with, including `.describe( )`? 

Pandas is full of these little gems: tools and tricks that optimize certain tasks, like looking at the beginning few elements of a dataset. Use them wisely! 

## Basic Visualizations

Let's get to the fun part! 

We're going to rely primarily on SeaBorn visualizations to illustrate its authenticity and aptitude for beautiful and simple visualizations. 

Each of these can also be replicated through MatPlotLib, though it would take much longer. 

### Let's explore the distribution of apps based on file size.

Remember that reformat we did to change 'size_bytes' data to 'size_Mb' data? 

That's going to come in handy, as we're going to generate a histogram of frequency occurrences (counts) based on general data byte size. 

First things first: let's set up the plotting space.

```py
plt.subplots(figsize=(10, 8))
```

Nice! 

Next, let's initialize the bins by which our histogram data will fall. 

Take note that for this example, the bins are not evenly sized but grow exponentially. 

Why do you think this is the case?

```py
BINS = [0.00, 10.00, 20.00, 50.00, 100.00, 200.00, 500.00, 1000.00, 2000.00, np.inf]
LABELS = ["<10m", "10-20m", "20-50m", "50-100m", "100-200m", "200-500m", "500-1000m", "1-2G", ">2G"]
```

Finally, let's produce our plotting object of frequency counts and use SeaBorn to visualize it for us.

```py
freqs = pd.cut(df["size_Mb"], BINS, include_lowest=True, labels=LABELS)
sns.barplot(y=freqs.value_counts().values, x=freqs.value_counts().index)
```

If all is well, you should see your very first tutorial visualization with SeaBorn here: 

![screenshot08_bargraph](../media/screenshot08_bargraph.png?raw=true)

### Looks great! Let's take a further look into priced vs. unpriced apps on the mobile store. 

We'll generate a doughnut plot using basic MatPlotLib to get a basic sense as to the distribution of paid vs. free apps on the app store.

First things first, let's set up our parameters in a new cell.

We want to look at two main categories: whether or not an app is free or paid.

```py
BINS = [-np.inf, 0.00, np.inf]
LABELS = ["FREE", "PAID"]
colors = ['lightcoral', 'yellowgreen']
```

When we look back at the data, our data contains numerical price data. 

Let's run a quick script to add a column containing free vs. paid price attributes for ease of use.

```py
df["price_categories"] = pd.cut(df["price"], BINS, include_lowest=True, labels=LABELS)
```

Now, let's initialize our plotting space in MatPlotLib. 

You'll often see this syntax `fig, ax` being used. 

```py
fig, axs = plt.subplots(figsize=(10, 5))
```

For easier reference, we'll slice our DataFrame to easily grab the data we want to look at. 

You can do this with ANY column or feature in your data.

```py
price_df = df["price_categories"].value_counts()
```

Finally, we'll generate a nice looking doughnut plot in MatPlotLib by plotting our data using a standard pie chart, then modifying the chart with a superimposed white circle to give the visualization some pizzazz. 

```py
plt.pie(price_df.values, labels=LABELS, colors=colors, autopct='%1.1f%%', shadow=True)
centre_circle = plt.Circle((0,0),0.75,color='black', fc='white',linewidth=1.25)

fig = plt.gcf()
fig.gca().add_artist(centre_circle)

plt.axis('equal')
```

A little bit of convoluted code, but if you followed it carefully in the same cell, you should see the following: 

![screenshot09_donutgraph](../media/screenshot09_donutgraph.png?raw=true)

Awesome! More paid apps than I expected, at least. 

### Let's keep moving with this idea and check out the highest rated free and paid apps. 

First things first: let's slice our categorical price data into two objects based on category.

```py
free_apps = df.loc[df["price_categories"] == "FREE"]
paid_apps = df.loc[df["price_categories"] == "PAID"]
```

From here, let's sort our sliced categorical price data based on total user ratings to get two new objects.

```py
free_apps_rated = free_apps.sort_values(by=["rating_count_tot"], ascending=False)
paid_apps_rated = paid_apps.sort_values(by=["rating_count_tot"], ascending=False)
```

Now that we have our rated free and rated paid apps objects, let's visualize them one at a time. 

For the sake of clarity, we'll only visualize the top ten highest rated apps in each category. 

First, let's look at free apps. (Feel free to grow this in a new cell.)

```py
sns.barplot(x=free_apps_rated["rating_count_tot"][:10], y=free_apps_rated["track_name"][:10])
```

You should see the following output:

![screenshot10_sidebargraph](../media/screenshot10_sidebargraph.png?raw=true)

Interesting. Personally, I'm shocked that Clash of Clans is more popular than Temple Run. 

Next, let's take a look at paid apps. 

```py
sns.barplot(x=paid_apps_rated["rating_count_tot"][:10], y=paid_apps_rated["track_name"][:10])
```

You should see the following (looks familiar?):

![screenshot11_sidebargraph](../media/screenshot11_sidebargraph.png?raw=true)

Looks like as much as I like Minecraft, it clearly doesn't hold a sword up to Fruit Ninja Classic.

### By the way, what if we just want to know the highest rated apps of all time, regardless of price? 

Not much setup to do here, so let's just dive into it. 

First, let's initialize our plotting space.

```py
plt.subplots(figsize=(20, 20))
```

Next, let's grab our data sorted by ratings as an object.

```py
ratings = df.sort_values(by=["rating_count_tot"], ascending=False)
```

Finally, let's plot it. 

For the sake of clarity, we'll plot the top thirty apps. 

```py
sns.barplot(x=ratings["rating_count_tot"][:30], y=ratings["track_name"][:30])
```

After all this, you should see the following:

![screenshot12_sidebargraph](../media/screenshot12_sidebargraph.png?raw=true)

I don't think anyone's truly surprised that Mark Zuckerberg commands the mobile app market. 

Think of any simple visualization-based questions you have from the basic data.

**How would you go about slicing the DataFrame and creating a visualization based on that?**

## *See Games With Us*

Before we were Make School, we were *Make Games With Us*. 

So before we continue, let's take a look at our mobile game market! 

First, let's visualize the distribution of apps based on genre to see how many games we're contending with!

```py
genres = df["prime_genre"].value_counts()
genres.sort_values(ascending=False, inplace=True)

plt.subplots(figsize=(20, 10))
sns.barplot(x=genres.values, y=genres.index, order=genres.index, orient="h")
```

You should see the following:

![screenshot13_sidebargraph](../media/screenshot13_sidebargraph.png?raw=true)

Great! Over 3000 of our dataset's apps are mobile games! 

This is good for us; generally in data science, the more data we have, the better. 

(Though like everything in data science, there are always exceptions!) 

Let's slice our data to grab only the mobile games and take a look at our data before moving forward.

```py
games = df.loc[df["prime_genre"] == "Games"]
games.head()
```

You should see the following data sliced:

![screenshot14_mobgames](../media/screenshot14_mobgames.png?raw=true)

By the way, one nice feature of Jupyter data representation with Pandas is seeing sliced data by index. 

Check the far left side of the DataFrame above. Notice anything? 

You may note that those bolded numbers aren't exactly in arithmetic incrementing order. Rather, they are *the entries from the original data that match our slicing criteria*!

### Let's explore price a little deeper by **exploring the distribution of mobile games by price**.

First things first: let's set up our object to hold mobile game data by price.

```py
prices = (games["price"].value_counts()) / (games["price"].shape[0]) * 100
prices.sort_values(ascending=False, inplace=True)
```

Now, let's initialize our plotting space and create a barplot to visualize our data.

```py
plt.subplots(figsize=(20, 10))
ax = sns.barplot(y=prices.values, x=prices.index, order=prices.index)
ax.set(xlabel="USD", ylabel="percent (%)")
```

You should see the following visualization:

![screenshot15_bargraph](../media/screenshot15_bargraph.png?raw=true)

As you may have suspected, most mobile games are free of charge and the majority of mobile games are under $4. 

Not too many popular mobile games out there that charge you more than a coffee!

### Speaking of price, let's close off our tutorial with some final visualizations looking at mobile game popularity. 

Let's first look into the top rated mobile games by price category. 

(This should look very familiar.) 

First things first: let's grab our categorical objects and sort them by total ratings. (Let's do this in a new cell!)

```py
free_games = games.loc[games["price_categories"] == "FREE"]
paid_games = games.loc[games["price_categories"] == "PAID"]

free_games_rated = free_games.sort_values(by=["rating_count_tot"], ascending=False)
paid_games_rated = paid_games.sort_values(by=["rating_count_tot"], ascending=False)
```

Then, let's initialize our plotting space, this time using subplots eloquently to display two plots dynamically.

```py
fig = plt.figure(figsize=(20, 10))
ax1 = fig.add_subplot(211)
ax2 = fig.add_subplot(212)
```

Finally, let's create two barplots for top rated free and paid mobile apps.

```py
sns.barplot(x=free_games_rated["rating_count_tot"][:10], y=free_games_rated["track_name"][:10], ax=ax1)
sns.barplot(x=paid_games_rated["rating_count_tot"][:10], y=paid_games_rated["track_name"][:10], ax=ax2)
```

You should see a visualization like the following:

![screenshot16_doublebargraphs](../media/screenshot16_doublebargraphs.png?raw=true)

While this is fine and dandy, keep in mind that currently we are only looking at top rated apps in both categories by *total rating*. 

For mobile developers, it may also be important to look at top rated apps in both categories by **current rating of the latest released version**! 

Let's explore that.

This should be easy peasy by now; in fact, the only significant change is selecting the column 'rating_count_ver' rather than 'rating_count_tot'. 

We save those slices as different objects. 

```py
free_games_rated_curr = free_games.sort_values(by=["rating_count_ver"], ascending=False)
paid_games_rated_curr = paid_games.sort_values(by=["rating_count_ver"], ascending=False)
```

Now let's initialize our plotting space. 

Same as the prior cell!

```py
fig = plt.figure(figsize=(20, 10))
ax1 = fig.add_subplot(211)
ax2 = fig.add_subplot(212)
```

Finally, let's create our barplots and display the top ten highest rated apps by current version. 

```py
sns.barplot(x=free_games_rated_curr["rating_count_ver"][:10], y=free_games_rated_curr["track_name"][:10], ax=ax1)
sns.barplot(x=paid_games_rated_curr["rating_count_ver"][:10], y=paid_games_rated_curr["track_name"][:10], ax=ax2)
```

You should see a visualization like the following:

![screenshot17_doublebargraphs](../media/screenshot17_doublebargraphs.png?raw=true)

How interesting that the data displayed in the top ten categorical apps by current rating are not at all the same as the top ten by total rating? 

Perhaps this is due to many of the most popular apps on the app store having many more releases. 

*How would you go about trying to visualize the number of releases each app has?*

---

## Final Thoughts

Data Science is a continuous, iterative, and complex process – often involving formulating and reformulating critical questions surrounding obscure bodies of data in order to derive an easy-to-communicate result.

Think about some of the questions you ended up answering in this tutorial.

- What's the **average byte size** of mobile apps?
- How many apps are **free vs. paid**?
- How many mobile apps are games and what is the general categorical distribution of **apps by genre**?
- What are the **most popular mobile games** by total popular rating? 


## **Answer these questions in a Markdown or text file and be prepared to share your responses with others.**

---

## Stretch Challenges

> NOTE: Include in tutorial and in stretch how to decide which questions are beyond scope. 

- Come up with at least **five more questions** that could be answered with the dataset. *(You do not have to answer them!)*
- Explore the SeaBorn documentation and construct at least **one additional unique visualization**. 