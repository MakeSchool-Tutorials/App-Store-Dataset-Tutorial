### Welcome to Part 1 of the tutorial series on basic exploratory data science! 

> NOTE: Add realistic scenario to connect to students. 

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

We can see the information from the very first app using either of the following two commands in new Jupyter cells:

```py
df.iloc[0]
```

```py
df.loc[1]
```

Both commands should return data for the app *PAC-MAN Premium*. 

`.iloc[]` and `.loc[]` are powerful selector tools in Pandas that allow you to view either a single or multiple rows, columns, and/or cells in a dataset. 

You may be wondering what the difference is between `.iloc[]` and `.loc[]`? 

Let's explore that using the example of the number **3**. 

When I run the following command in a new cell: 

```py
df.iloc[3]
```

Pandas returns the data for the app *eBay: Best App to Buy, Sell, Save! Online Shopping*. 

However, if you notice carefully, the *eBay* app appears in the **fourth position** in the original dataframe. 

**Why is this the case?**

Well, it turns out that `.iloc[]` is useful for selecting *data by index*. 

Since Python (like most languages), has zero-indexed arrays, the input value **3** refers to the **fourth** row of the dataset! 

Let's contrast that with the following command:

```py
df.loc[3]
```

In this case, Pandas returns the data for the app *WeatherBug - Local Weather, Radar, Apps, Alerts*. 

**Why is this the case?**

Simply put, `.loc[]` is useful for selecting *data by label*. 

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

Before we go any further, let's take a quick peek at the top of our DataFrame to check the data for any obvious red flags. 

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

Doesn't this look familiar to some of the original commands we played with, including `.describe()`? 

---

Let's try something new - let's use our Pythonic powers and attempt to calculate the average price across all apps. 

In this example, we don't care about which apps are free vs. paid or the general distribution of apps by payment; we only want to calculate the exact average price. 

> (For those of you who are more statistically inclined out there, you may be raising an eyebrow. <br><br>Good! <br><br>That intuition is important for answering more realistic data science questions involving descriptive statistics like the population average. <br><br>For now however, it is important to focus on the example at hand and understand the data science process.)

To calculate the exact average price for all apps, we can run the following command in a new cell:

```py
df.price.mean()
```

This should return the value ``` 1.7262178685562626 ```, suggesting that the average price of an app is about $1.73. 

---

By the way, those selectors seem great, but imagine trying to get general information about **your entire dataset** that way. 

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

---

Now imagine we want to know how large our apps are. 

How can we go about solving that question? 

In the last few cells, ever take a look at that 'size_bytes' column? 

Pretty nasty, huh? 

In fact, on our `.describe( )` cell above, we see that most data contains 'size_bytes' attributes ranging in the millions. 

Well, in the real world, we call those Megabytes and can do some quick reformatting to our DataFrame to clean that up a little.

```py
def _byte_resizer(data):
    return np.around(data / 1000000, decimals=2)

df["size_Mb"] = df["size_bytes"].apply(_byte_resizer)
df.drop("size_bytes", axis="columns", inplace=True)
```

Notice how in the previous cell, we wrote a custom helper function ```_byte_resizer()``` that contained our logic for grabbing ```size_bytes``` data and dividing it effectively to give us our new ```size_Mb``` column. 

Pandas is full of little gems: tools and tricks that optimize certain tasks, like looking at the beginning few elements of a dataset. 

However, the best Python developers and data science experts maintain their flexibility with Python to be able to write custom functions and commands at will based on what they're doing. 

For instance, take a look at the following command. It does the exact same thing as the previous cell, but is written quite differently using more advanced Python syntax that may not be as nice for readability. 

```py
df["size_Mb"] = df["size_bytes"].apply(lambda num: np.around(num / 1000000, decimals=2))
df.drop("size_bytes", axis="columns", inplace=True)
```

Which looks cleaner to you? 

Use your skills wisely! 