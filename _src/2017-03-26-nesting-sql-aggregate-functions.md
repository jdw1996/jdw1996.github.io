<!-- Nesting SQL Aggregate Functions -->
<!-- 2017-03-26 -->

This co-op term, I'm working for the
[Jonah Group](https://www.jonahgroup.com/).  My team is building a Big Data
application for a corporate client, and I have spent a lot of my time writing
the queries for canned reports that the client wishes to receive on their data.
In the case of our project, we are storing all data using
[Hadoop](http://hadoop.apache.org/) and querying it using
[Hive Query Language](https://hive.apache.org/), which closely resembles SQL.

Before I started at the Jonah Group in January, I didn't have any experience
writing database queries, so there were a few patterns in our codebase that
took some getting used to. One construct in particular that I had trouble with
was nesting aggregate functions (like `SUM` and `COUNT`). I didn't find many
resources discussing these, so I thought I'd write my own.

Like I said, I first came across these ideas while using Hive, but the same
principles apply in other SQL implementations. As I write this, I'll be testing
my code in [PostgreSQL](https://www.postgresql.org/).

First things first, we'll need some example data to practice with. Here's the
data I'll use:

 name | amount | year
:----:|-------:|----:
 Gene | 234.04 | 2015
 Paul |  56.99 | 2015
Peter |  23.95 | 2015
Peter | 102.76 | 2015
  Ace |   5.49 | 2016
  Ace |  19.41 | 2016
  Ace |  22.77 | 2016
 Gene | 166.30 | 2016
 Paul |  65.99 | 2016
Peter |  89.90 | 2016

You can get the code to create this table, along with all of the queries
discussed below,
[here](https://gist.github.com/jdw1996/036d10e4a5a32fac218fda6a4a864b29).

## Overview of Aggregate Functions

Let's start by looking at the result of using `COUNT` and `SUM` over `amount`
when we group by `year`. We use this query:

```
SELECT year,
    COUNT(*) AS the_count,
    SUM(amount) AS the_sum
FROM purchases
GROUP BY year;
```

And we get this data:

year | the_count | the_sum
:---:|----------:|-------:
2015 |         4 |  417.74
2016 |         6 |  369.86

By using the `GROUP BY` statement, we squash all the records with matching
values in the `year` column together, and count or sum them as the case may be.
This gives us the number of purchases and total amount spent in each year.

We could instead group by both `year` and `name`. Then the query becomes:

```
SELECT year,
    name,
    COUNT(*) AS the_count,
    SUM(amount) AS the_sum
FROM purchases
GROUP BY year, name;
```

And we get this data:

year | name  | the_count | the_sum
----:|:-----:|----------:|-------:
2015 | Gene  |         1 |  234.04
2015 | Paul  |         1 |   56.99
2015 | Peter |         2 |  126.71
2016 | Ace   |         3 |   47.67
2016 | Gene  |         1 |  166.30
2016 | Paul  |         1 |   65.99
2016 | Peter |         1 |   89.90

Now we are squashing together all the records with matching `year`s *and*
matching `name`s. This means that we get the number of purchases each person
made in each year, along with the total amount they spent in each year.

## Aggregating the Results

Let's suppose that we now want to find the percentage of the total money spent
in a year that is spent by each person. You may have to read that sentence a
couple of times to make sense of it. It may also help to explain the basic
procedure we want to follow. It will be something like this:

1.  Find the total amount spent by each person, in each year.
2.  Find the total amount spent by everyone in each year.
3.  Divide the first number by the second, for each person and each year. (And
    multiply by 100 to get a percentage.)

We already have the numbers needed for the first two steps in our queries
above. The tricky part is combining them to finish of the third step. You could
store information in some intermediate table, but there is a simpler way.

The first thing to note is that we want our result to have exactly one row for
every combination of `year` and `name`. This indicates that we should put both
of these in a `GROUP BY` clause.

We know how to get the total amount spent by each person each year when
grouping by `year` and `name`, from the second query we looked at. In our
percentage calculation, this will be the numerator, and the denominator will be
the sum of all those amounts, across the year we are looking at. To achieve
this sum of sums, so to speak, we can use the `OVER` clause. With it, we can
calculate the two values we need like this:

```
SELECT year,
    name,
    SUM(amount) AS numerator,
    SUM(SUM(amount))
      OVER (PARTITION BY year) AS denominator
FROM purchases
GROUP BY year, name;
```

This gives us:

year | name  | numerator | denominator
----:|:-----:|----------:|-----------:
2015 | Gene  |    234.04 |      417.74
2015 | Paul  |     56.99 |      417.74
2015 | Peter |    126.71 |      417.74
2016 | Ace   |     47.67 |      369.86
2016 | Gene  |    166.30 |      369.86
2016 | Paul  |     65.99 |      369.86
2016 | Peter |     89.90 |      369.86

Looking at the nested `SUM` calls, we can think of the inner one as being
affected by the `GROUP BY` clause, calculating the sum for each combination of
person and year. Then, the outer one sums up all the resulting amounts for each
year, because only `year` is included in the `PARTITION BY` condition.

Putting it all together, we can use this query to calculate the percentages we
are looking for:

```
SELECT year,
    name,
    SUM(amount)
      * 100.0
      / SUM(SUM(amount)) OVER (PARTITION BY year)
      AS pct_of_spending
FROM purchases
GROUP BY year, name;
```

The result is:

year | name  | pct_of_spending
----:|:-----:|-------------------:
2015 | Gene  | 56.0252788816009958
2015 | Paul  | 13.6424570306889453
2015 | Peter | 30.3322640877100589
2016 | Ace   | 12.8886605742713459
2016 | Paul  | 17.8418861190720813
2016 | Peter | 24.3064943492132158
2016 | Gene  | 44.9629589574433569

## A Note on MySQL

When I started writing this post, I was using [MySQL](https://www.mysql.com/),
but then I realized that it does not support the `OVER` clause. At that point,
I switched over to PostgreSQL.

I don't have any special insight into *why* MySQL doesn't support this feature
now, but hopefully they implement it in the near future, because it is very
useful.
