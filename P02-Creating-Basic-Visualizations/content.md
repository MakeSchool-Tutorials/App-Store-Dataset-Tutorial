---
title: "Creating Basic Visualizations"
slug: creating-basic-visualizations
---

# Basic Visualizations

Part 2: Exploratory Data Science (cont'd)

Now that we've spent enough time playing with our data and assuring ourselves that it's relatively clean, let's get to the fun part!

We're going to rely primarily on SeaBorn visualizations to illustrate its authenticity and aptitude for beautiful and simple visualizations.

Each of these can also be replicated through MatPlotLib, though it would take much longer.

# Let's explore the distribution of apps based on file size.

Remember that reformat we did to change 'size_bytes' data to 'size_Mb' data?

That's going to come in handy, as we're going to generate a histogram of frequency occurrences (counts) based on general data byte size.

First things first: let's set up the plotting space in a new cell.

This should be continued in the same Jupyter notebook as the previous tutorial section.

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

Enter the following into a new cell:

```py
freqs = pd.cut(df["size_Mb"], BINS, include_lowest=True, labels=LABELS)
sns.barplot(y=freqs.value_counts().values, x=freqs.value_counts().index)
```

If all is well, you should see your very first tutorial visualization with SeaBorn here:

![screenshot08_bargraph](../media/screenshot08_bargraph.png?raw=true)

# Priced vs. Unpriced

Looks great! Let's take a further look into priced vs. unpriced apps on the mobile store.

We'll generate a doughnut plot using basic MatPlotLib to get a basic sense as to the distribution of paid vs. free apps on the app store.

First things first, let's set up our parameters in a new cell.

We want to look at two main categories: whether or not an app is free or paid.

Enter the following into a new cell:

```py
BINS = [-np.inf, 0.00, np.inf]
LABELS = ["FREE", "PAID"]
colors = ['lightcoral', 'yellowgreen']
```

When we look back at the data, our data contains numerical price data.

Let's run a quick script to add a column containing free vs. paid price attributes for ease of use.

Enter the following into a new cell:

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

Enter the following into a new cell:

```py
price_df = df["price_categories"].value_counts()
```

Finally, we'll generate a nice looking doughnut plot in MatPlotLib by plotting our data using a standard pie chart, then modifying the chart with a superimposed white circle to give the visualization some pizzazz.

Enter all this into a single cell:

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

# Highest Rated Free and Paid Apps

Let's keep moving with this idea and check out the highest rated free and paid apps.

First things first: let's slice our categorical price data into two objects based on category in a new cell.

```py
free_apps = df.loc[df["price_categories"] == "FREE"]
paid_apps = df.loc[df["price_categories"] == "PAID"]
```

From here, let's sort our sliced categorical price data based on total user ratings to get two new objects in a new cell.

```py
free_apps_rated = free_apps.sort_values(by=["rating_count_tot"], ascending=False)
paid_apps_rated = paid_apps.sort_values(by=["rating_count_tot"], ascending=False)
```

Now that we have our rated free and rated paid apps objects, let's visualize them one at a time.

For the sake of clarity, we'll only visualize the top ten highest rated apps in each category.

First, let's look at free apps. (Feel free to put this in a new cell.)

```py
sns.barplot(x=free_apps_rated["rating_count_tot"][:10], y=free_apps_rated["track_name"][:10])
```

You should see the following output:

![screenshot10_sidebargraph](../media/screenshot10_sidebargraph.png?raw=true)

Interesting. Personally, I'm shocked that Clash of Clans is more popular than Temple Run.

Next, let's take a look at paid apps. Put it in a new cell.

```py
sns.barplot(x=paid_apps_rated["rating_count_tot"][:10], y=paid_apps_rated["track_name"][:10])
```

You should see the following (looks familiar?):

![screenshot11_sidebargraph](../media/screenshot11_sidebargraph.png?raw=true)

Looks like as much as I like Minecraft, it clearly doesn't hold a sword up to Fruit Ninja Classic.

# Highest Rated Apps of All Time

By the way, what if we just want to know the highest rated apps of all time, regardless of price?

Not much setup to do here, so let's just dive into it.

First, let's initialize our plotting space. New cell!

```py
plt.subplots(figsize=(20, 20))
```

Next, let's grab our data sorted by ratings as an object. In a new cell.

```py
ratings = df.sort_values(by=["rating_count_tot"], ascending=False)
```

# Let's Plot it

Finally, let's plot it.

For the sake of clarity, we'll plot the top thirty apps. And guess what? You'll put it in a new cell!

```py
sns.barplot(x=ratings["rating_count_tot"][:30], y=ratings["track_name"][:30])
```

After all this, you should see the following:

![screenshot12_sidebargraph](../media/screenshot12_sidebargraph.png?raw=true)

I don't think anyone's truly surprised that Mark Zuckerberg commands the mobile app market.

Think of any simple visualization-based questions you have from the basic data.

**How would you go about slicing the DataFrame and creating a visualization based on that?**

Ponder these questions and others you may have regarding the newly explored data as we move on to our *third and final section of this tutorial*.
