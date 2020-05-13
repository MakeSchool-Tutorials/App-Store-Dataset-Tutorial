---
title: "See Games With Us"
slug: see-games-with-us
---

Part 3: Exploratory Data Science (finale)

# See Games With Us

Before we were Make School, we were *Make Games With Us*.

So before we continue, let's take a look at our mobile game market!

First, let's visualize the distribution of apps based on genre to see how many games we're contending with! Please put this in a new cell.

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

Let's slice our data to grab only the mobile games and take a look at our data before moving forward. New cell.

```py
games = df.loc[df["prime_genre"] == "Games"]
games.head()
```

You should see the following data sliced:

![screenshot14_mobgames](../media/screenshot14_mobgames.png?raw=true)

By the way, one nice feature of Jupyter data representation with Pandas is seeing sliced data by index.

Check the far left side of the DataFrame above. Notice anything?

You may note that those bolded numbers aren't exactly in arithmetic incrementing order. Rather, they are *the entries from the original data that match our slicing criteria*!

# Exploring Deeper

Let's explore price a little deeper by **exploring the distribution of mobile games by price**.

First things first: let's set up our object in a new cell to hold mobile game data by price.

```py
prices = (games["price"].value_counts()) / (games["price"].shape[0]) * 100
prices.sort_values(ascending=False, inplace=True)
```

Now, let's initialize our plotting space in a new cell and create a barplot to visualize our data.

```py
plt.subplots(figsize=(20, 10))
ax = sns.barplot(y=prices.values, x=prices.index, order=prices.index)
ax.set(xlabel="USD", ylabel="percent (%)")
```

You should see the following visualization:

![screenshot15_bargraph](../media/screenshot15_bargraph.png?raw=true)

As you may have suspected, most mobile games are free of charge and the majority of mobile games are under $4.

Not too many popular mobile games out there that charge you more than a coffee!

# Visualizations

Speaking of price, let's close off our tutorial with some final visualizations looking at mobile game popularity.

Let's first look into the top rated mobile games by price category.

(This should look very familiar.)

First things first: let's grab our categorical objects and sort them by total ratings. (Let's do this in a new cell!)

```py
free_games = games.loc[games["price_categories"] == "FREE"]
paid_games = games.loc[games["price_categories"] == "PAID"]

free_games_rated = free_games.sort_values(by=["rating_count_tot"], ascending=False)
paid_games_rated = paid_games.sort_values(by=["rating_count_tot"], ascending=False)
```

Then, let's initialize our plotting space (in a new cell), this time using subplots eloquently to display two plots dynamically.

```py
fig = plt.figure(figsize=(20, 10))
ax1 = fig.add_subplot(211)
ax2 = fig.add_subplot(212)
```

Finally, let's create two barplots for top rated free and paid mobile apps. In a new cell.

```py
sns.barplot(x=free_games_rated["rating_count_tot"][:10], y=free_games_rated["track_name"][:10], ax=ax1)
sns.barplot(x=paid_games_rated["rating_count_tot"][:10], y=paid_games_rated["track_name"][:10], ax=ax2)
```

You should see a visualization like the following:

![screenshot16_doublebargraphs](../media/screenshot16_doublebargraphs.png?raw=true)

While this is fine and dandy, keep in mind that currently we are only looking at top rated apps in both categories by *total rating*.

For mobile developers, it may also be important to look at top rated apps in both categories by **current rating of the latest released version**!

# Let's Explore That

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

Finally, let's create our barplots in a new cell and display the top ten highest rated apps by current version.

```py
sns.barplot(x=free_games_rated_curr["rating_count_ver"][:10], y=free_games_rated_curr["track_name"][:10], ax=ax1)
sns.barplot(x=paid_games_rated_curr["rating_count_ver"][:10], y=paid_games_rated_curr["track_name"][:10], ax=ax2)
```

You should see a visualization like the following:

![screenshot17_doublebargraphs](../media/screenshot17_doublebargraphs.png?raw=true)

How interesting that the data displayed in the top ten categorical apps by current rating are not at all the same as the top ten by total rating?

Perhaps this is due to many of the most popular apps on the app store having many more releases.

*How would you go about trying to visualize the number of releases each app has?*

# Final Thoughts

Data Science is a continuous, iterative, and complex process – often involving formulating and reformulating critical questions surrounding obscure bodies of data in order to derive an easy-to-communicate result.

Think about some of the questions you ended up answering in this tutorial.

- What's the **average byte size** of mobile apps?
- How many apps are **free vs. paid**?
- How many mobile apps are games and what is the general categorical distribution of **apps by genre**?
- What are the **most popular mobile games** by total popular rating?


# Challenges

Answer these questions in a Markdown or text file and be prepared to share your responses with others.

# Stretch Challenges

- Come up with at least **five more questions** that could be answered with the dataset. *(You do not have to answer them!)*
- Explore the SeaBorn documentation and construct at least **one additional unique visualization**.
