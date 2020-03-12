ACDI/VOCA R Policy
================
2020-03-12 16:12:33

![](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/R_logo.svg/310px-R_logo.svg.png)

## RStudio

RStudio is an IDE for the R programming language and is the IDE of
choice for ACDI/VOCA for using R. Split into two editions, RStudio for
Desktop and RStudio Server, it is an incredibly empowering tool that
allows one to use R to its full potential. The different panels in
RStudio such as “Environment”, “Files”, “Help”, “History” allows for
easy organization and interacting with all the files and objects in your
analysis.

![](https://d33wubrfki0l68.cloudfront.net/7f83bbb6aae31666477e9125355915ecfa5dd967/90a82/images/2017-08-30-rstudio-dark-theme.png)

Above is what a RStudio set up might look like. You can spend time to
position and size each of the panels however you want along with a great
many other aesthetical aspects like the code font and color.

## R Projects

Using R Projects is a vital part of keeping your work separately
contained with its own working directory, workspace, history, and source
documents. Instead of cluttering up your environment with multiple
objects from multiple projects you can have a number of different
projects opened in its own window so things from one project won’t
interfere with another.

A great resource to get started with R Projects is
[here](https://support.rstudio.com/hc/en-us/articles/200526207-Using-Projects).

For a general overview of a project-oriented workflow in R, consult this
website (still work-in-progress)
[here](https://whattheyforgot.org/project-oriented-workflow.html)
written by Jenny Bryan and Jim Hester from R Studio. For a shorter
blogpost version of the above see
[here](https://www.tidyverse.org/articles/2017/12/workflow-vs-script/).

## GitHub

Now that you have everything organized by using Projects you might want
to backup and version control your code and analyses using GitHub. Using
GitHub will also allow you to coordinate and collaborate with other
analysts on the same project, you will never again have to search your
email for “datafile\_revised.data” or
“final\_final\_final\_draft3.doc”\!

A great introduction to switching over to a version control system for
your projects can be read [here](https://peerj.com/preprints/3159/).

For connecting your RStudio Project to Git and GitHub check out this
[chapter](https://happygitwithr.com/rstudio-git-github.html) of the
book, “Happy Git with R” by Jenny Bryan.

## The Tidyverse and Coding Style

![](https://rviews.rstudio.com/post/2017-06-09-What-is-the-tidyverse_files/tidyverse1.png)

The Tidyverse is a set of R packages specifically designed for doing
data science. The packages are consistent in their underlying design and
structure which makes it very easy to learn. In regards to syntax and
coding style, ACDI VOCA follows the [tidyverse style
guide](https://style.tidyverse.org/) for consistent development of our
code and tools. One of the main strengths of the tidyverse is that the
code is very easily readable by a human compared to base R code. Some
great examples of this can be seen
[here](https://tavareshugo.github.io/data_carpentry_extras/base-r_tidyverse_equivalents/base-r_tidyverse_equivalents.html)
in a great webpage that shows how to do something in both base R and the
tidyverse\!

The best way to get oriented with the tidyverse packages is to go
through Hadley Wickham’s [R for Data Science](https://r4ds.had.co.nz/)
book which is provided online and for free\! You can also go visit the
Slack group for R4DS [here](https://rfordatascience.slack.com/) to get
online help.

Some particular notes on how we write R code:

  - For commenting lines out we use ONE `#` whereas if we want to make a
    comment in the code we use TWO `##`.

<!-- end list -->

``` r
data %>% 
  select(this, that, everything()) %>% 
  ## Calculate values here:
  mutate(nutrition10 = nutrition * 10,
         ham_production = ham + production - costs) %>% 
  ## We need to filter by values over 5000:
  filter(nutrition10 > 5000) %>% 
  ## No need to arrange by DESC order anymore:
  #arrange(desc(nutrition10))
```

  - Avoid using `for` loops or the `apply()` family of functions
    (`sapply`, `lapply`, `vapply`, etc.) and use the `map()` functions
    from the `purrr` package instead. Note that there is nothing wrong
    with using `for` loops in R necessarily but we want to keep
    everything consistent across the codebase.

<!-- end list -->

``` r
library(tidyverse)
```

    ## Registered S3 method overwritten by 'rvest':
    ##   method            from
    ##   read_xml.response xml2

    ## -- Attaching packages ----------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.3.0     v purrr   0.3.3
    ## v tibble  2.1.3     v dplyr   0.8.4
    ## v tidyr   1.0.2     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.3.0

    ## -- Conflicts -------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
## calculate the mean for 10 observations of a randomly generated normal distribution
1:10 %>%
  map(rnorm, n = 10) %>%
  map_dbl(mean)
```

    ##  [1]  0.8123807  2.0213381  2.8889394  3.6325555  4.9214859  5.6274930
    ##  [7]  6.5273035  7.8198616  8.9798907 10.2728308

  - For naming functions and arguments we use snakecase. Ex.
    `ConnectToMEF()` and `fieldToMod = "Indicators"`.

<!-- end list -->

``` r
AddThenSubtract <- function(entryNumber, subtractionNumber) {
  
  sum <- entryNumber + 25 - subtractionNumber
  
  sum
}

AddThenSubtract(entryNumber = 5, subtractionNumber = 3.5)
```

    ## [1] 26.5

## Installing an R Package

There is a tab in R Studio that helps you manage installing packages.

![](https://i.imgur.com/byRVkX7.jpg)

From here you can choose from where you want to install a package,
either from CRAN or a local `.tar.gz` or `.zip` file. Then you just type
the name of the package in, be very careful with the proper
capitalization and underscore/dash in the names\! Then, make sure you
are installing the package into the proper directory, check whether you
want to install dependencies or not and click install. Remember that
after installing you need to load the package with a `library()` call
every new session.

If you want to install from the console or in a script:

``` r
## Take off thehashtag in the line below to "uncomment" it:
# install.packages("nycflights13")
library(nycflights13)

## let's look at one of the datasets from our newly installed package!
glimpse(airlines)
```

    ## Observations: 16
    ## Variables: 2
    ## $ carrier <chr> "9E", "AA", "AS", "B6", "DL", "EV", "F9", "FL", "HA", ...
    ## $ name    <chr> "Endeavor Air Inc.", "American Airlines Inc.", "Alaska...

Not all packages are available on CRAN, here’s how you would install a
package from GitHub:

``` r
remotes::install_github("ACDIVOCATech/bulletchartr")

library(bulletchartr)
```

## Where to find inspiration and HELP\!

R is an open-source language with tons of community support online and
in print. A great way to read up on the latest R news is to go on
Twitter and search for `#rstats`. You should also subscribe to [R
Bloggers](https://www.r-bloggers.com/) to get a curated stream of news,
blog posts, and tutorials provided by thousands of R users from across
the world.

For getting help, go over to [StackOverflow](https://stackoverflow.com)
and look up questions tagged with `[r]` and any other R package you
might be having trouble with. Another good resource that is solely
dedicated to R problems and discussion is [RStudio
Community](https://community.rstudio.com/). One can also try asking for
help on Twitter via the `#rstats` hashtag as well\!

## Create/develop R packages

At ACDI/VOCA we have a whole suite of packages that helps us gather and
process data from our databases and output them into a variety of
dashboards and visualizations. The [R Packages]() book by Hadley Wickham
is a great and free online resource for those who want to create their
own package to help with their analysis or contribute to one of our own
existing packages.

When collaborating on packages (this can apply to projects too) it is
important to create your own branch alongside the main stream of
development. This allows you and others to work in parallel without
overwriting eachother’s work. A good way to organize things is that you
leave the main “master” branch alone while everybody

When using an ACDI/VOCA R package, please remember to PULL the latest
version from Github from the Github panel in RStudio. When contributing
to any of our R packages please create your own branch and when you’re
done submit a Pull Request to the package authors. For an introduction
on contributing to open-source using GitHub, see
[here](https://github.com/firstcontributions/first-contributions).

## Misc

Here are some general best practices that should be followed:

  - Inputs into complex functions should be stated explicitly, example:
    DO THIS: `function(arg1 = x, arg2 = y)` instead of this:
    `function(x,y)`. It’s a bit more annoying, but does help resolve
    conflicts, especially when working with packages like
    `ACDIVOCAdataGetter` that are continuously in active development.

## For Admins: how to test pull requests:

1.  Git fetch
2.  Switch branch to one to test
3.  Build Package using “BUILD \> MORE \> CLEAN AND REBUILD”.
    Alternatively, use R CMD INSTALL –preclean in console.
4.  Run tests (Ctrl + Shift + T). If this passes, continue
5.  See if Issue has been resolved. If yes, Merge with Master, and
    delete branch.
6.  Pull Master into the server, turn off PT/GQ runners on crontab, run
    Day\_Deleter() and stay vigilant for 30 mins. All should be good.
7.  Delete local branch (run `git gc` or `git prune` or `git branch -D
    branch_name`).
