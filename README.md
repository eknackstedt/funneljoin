
<!-- README.md is generated from README.Rmd. Please edit that file -->
**Disclaimer**: funneljoin is still in active development. While we are using it actively at DataCamp, there are still some bugs, especially with remote tables. If you can, we recommend pulling your data to work with locally.

Overview
========

The goal of funneljoin is to make it easy to analyze behavior funnels. For example, maybe you're interested in finding the people who visit a page and then register. Or you want all the times people click on an item and add it to their cart within 2 days.

You can do this with funneljoin's `after_join()` function. The arguments are:

-   `x`: a dataset with the first set of behaviors.
-   `y`: a dataset with the second set of behaviors.
-   `by_time`: a character vector to specify the time columns in x and y. Must be a single column in each tbl. Note that this column is used to filter for time y &gt;= time x.
-   `by_user`: a character vector to specify the user or identity columns in x and y. Must be a single column in each tbl.
-   `mode`: the method used to join: "inner", "full", "anti", "semi", "right", "left".
-   `type`: the type of funnel used to distinguish between event pairs, such as "first-first", "last-first", "any-firstafter". See types of funnels.
-   `dt` (optional): if doing a gap join, either the number of seconds between events or a difftime object.

As funneljoin uses dplyr, it can also work with remote tables, **but has only been tried on postgres**. Some of the functionality, especially the gap joins, use SQL code that may not work with different versions.

Installation
------------

You can install the development version from [GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("robinsones/funneljoin")
```

Types of funnels
----------------

Paired funneljoins can be any combination of `first`, `last`, `any`, and `lastbefore`, combined with `first`, `last`, `any`, and `firstafter`. Some common ones you may use include:

-   **first-first**: Take the earliest x and y for each user **before** joining. For example, you want the first time someone entered an experiment, followed by the first time someone **ever** registered. If they registered, entered the experiment, and registered again, you do not want to include that person.
-   **first-firstafter**: Take the first x, then the first y after that. For example, you want when someone first entered an experiment and the first course they started afterwards. You don't care if they started courses before entering the experiment.
-   **lastbefore-firstafter**: First x that's followed by a y before the next x. For example, in last click paid ad attribution, you want the last time someone clicked an ad before the first subscription they did afterward.
-   **any-firstafter**: Take all Xs followed by the first Y after it. For example, you want all the times someone visited a homepage and their first product page they visited afterwards.
-   **any-any**: Take all Xs followed by all Ys. For example, you want all the times someone visited a homepage and **all** the product pages they saw afterward.

The other main types of funneljoins are gap joins. These take into account the time difference between events. There are currently two types:

-   smallestgap: Take the smallest x-y gap (in tie, earliest).
-   withingap: Take all x-y pairs with a gap of less than dt (all ad clicks followed by a course start within an hour).

Rules
-----

Some rules to keep in mind:

-   If type\_x is "last" or "first", then a right join has the same number of rows as y.
-   If type\_y is "last", "first", or "firstafter", then a left join has the same number of rows as x.

Reporting bugs and adding features
----------------------------------

If you find any bugs or have a feature request or question, please [create an issue](https://github.com/datacamp/funneljoin/issues/new). If you'd like to add a feature, tests, or other functionality, please make an issue first and let's discuss!
