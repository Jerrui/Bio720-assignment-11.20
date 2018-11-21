Data Visualization with ggplot2: Introduction
=============================================

Exploratory vs Explanatory
--------------------------

-   Exploratory visualizations: Graphical data analysis. Easily
    generated, data-heavy, intended for a small specialist audience.
-   Explanatory visualizations: Communication process. Labor-intensive,
    data-specific, intended for a broader audience.

Exploring ggplot2 part1
-----------------------

    library(ggplot2)
    ggplot(mtcars, aes(x = cyl, y = mpg)) +
      geom_point()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-1-1.png)

-Load the ggplot2 package first.

-Deifine x axis and y axis: x = *, y = *)

-geom\_point(): Scatter plot

-Notice that ggplot2 treats cyl as a continuous variable, which is
wrong.

    ggplot(mtcars, aes(x = factor(cyl), y = mpg)) +
      geom_point()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-2-1.png)

-   Wrapping factor() around cyl: ggplot2 treats cyl as a factor.
    Problem is fixed.

Exploring ggplot2 part2
-----------------------

### Other aesthetics

    ggplot(mtcars, aes(x = wt, y = mpg)) +
      geom_point()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-3-1.png)

    ggplot(mtcars, aes(x = wt, y = mpg, color = disp)) +
      geom_point()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-3-2.png)

    ggplot(mtcars, aes(x = wt, y = mpg, size = disp)) +
      geom_point()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-3-3.png)

-color =/size = : Legend for the color and size scales was
generated(dependent on disp).

### Understanding Variables

ggplot(mtcars, aes(x = wt, y = mpg, shape = disp)) + geom\_point()

-This(shape = disp) gives you an error: only makes sense with
categorical data, and disp is continuous.

geom\_point() and geom\_smooth()
--------------------------------

    ggplot(diamonds, aes(x = carat, y = price))  +
      geom_point()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    ggplot(diamonds, aes(x = carat, y = price)) +
      geom_point() +
      geom_smooth()

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-4-2.png)

-geom\_point(): draw points on the plot.

-geom\_smooth(): draw a smoothed line over the points.

Exploring ggplot2 part4
-----------------------

    ggplot(diamonds, aes(x = carat, y = price, color = clarity)) +
      geom_smooth()

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    ggplot(diamonds, aes(x = carat, y = price, color = clarity)) +
      geom_point(alpha = 0.4)

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-5-2.png)

-color = clarity: clarity mapped to color.

-geom\_point(alpha = 0.4): This will make the points 60% transparent/40%
visible.

Understanding the grammar part1
-------------------------------

    dia_plot <- ggplot(diamonds, aes(x = carat, y = price))

    dia_plot + geom_point()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-6-1.png)

    dia_plot + geom_point(aes(color = clarity))

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-6-2.png)

-   You can store the plot as a ggplot object that you can use later(add
    other layers).

-   You can also modify the functions.

Understanding the grammar part2
-------------------------------

    dia_plot + geom_smooth(aes(col = clarity), se = FALSE)

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-7-1.png)

-se = FALSE: No error shading.

Data Visualization with ggplot2: Data
=====================================

Base package and ggplot2
------------------------

-Base package disadvantages:

1.  Plot doesn't get redrawn.

2.  Plot is drawn as a image.

3.  Need to manually add legend

4.  No unified framework for plotting.

-Using ggplot2

The major advantage of ggplot2 is the ggplot object. You can sign the
base layers to an object, and then recycle the object with different
arguements(such as different plot types).

Plotting the ggplot2 way
------------------------

-   The ggplot2 way:

Rearrange the data to define variables in your dataset according to what
we are really interested in. Should make the function calls as
straightforward as possible.

Tidy Data
---------

### facet\_grid(. ~ \_\_\_) It will generate several plots based on the thing you fill in the blank.

### How to rearrange your data

-'gather()'

It moves information from the columns to the rows. It takes multiple
columns and gathers them into a single column by adding rows.

Say you have a dataset like this:

    head(iris)

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa

Then if you do:

    library(tidyr)
    iris.tidy <- iris %>% 
      gather(key, Value, -Species)

And then have a look:

     head(iris.tidy)

    ##   Species          key Value
    ## 1  setosa Sepal.Length   5.1
    ## 2  setosa Sepal.Length   4.9
    ## 3  setosa Sepal.Length   4.7
    ## 4  setosa Sepal.Length   4.6
    ## 5  setosa Sepal.Length   5.0
    ## 6  setosa Sepal.Length   5.4

So we can see that all the columns are combined except 'species'
column(-species). All the values are now in the 'value' column and the
column names in the previous data frame are now combined in the 'key'
column.

-'separate()‘

It splits one column into two or more columns according to a pattern you
define. So if you add another line to the previous gather command:

     iris.tidy <- iris %>%
      gather(key, Value, -Species) %>%
      separate(key, c("Part", "Measure"), "\\.")

And then you will see:

     head(iris.tidy)

    ##   Species  Part Measure Value
    ## 1  setosa Sepal  Length   5.1
    ## 2  setosa Sepal  Length   4.9
    ## 3  setosa Sepal  Length   4.7
    ## 4  setosa Sepal  Length   4.6
    ## 5  setosa Sepal  Length   5.0
    ## 6  setosa Sepal  Length   5.4

So the 'key' column in the previous data frame is now separated into
"Part" and "Measure". And "\\." lets the 'key' column separate by ".".

-'%&gt;%'

It passes the result of the left-hand side as the first argument of the
function on the right-hand side.

-'spread()'

It takes two columns (key & value), and spreads into multiple columns:
it makes “long” data wider.

Data Visualization with ggplot2: Visible Aesthetics
===================================================

Typical Aesthetics
------------------

X: x axis position

y: Y axis position

colour: Colour of dots, outlines of other shapes

fill: Fill colour

size: Diameter of points, thickness of lines

alpha: Transparency

linetype: Line dash pa!ern

labels: Text on a plot or axes

shape: Shape

### shape

Shapes in R can have a value from 1-25. Shapes 1-20 can only accept a
color aesthetic, but shapes 21-25 have both a color and a fill
aesthetic.

The default geom\_point() uses shape = 19 (a solid circle with an
outline the same colour as the inside). Good alternatives are shape = 1
(hollow) and shape = 16 (solid, no outline). These all use the col
aesthetic.

A really nice alternative is shape = 21 which allows you to use both
fill for the inside and col for the outline.

### hexadecimal colours

Hexadecimal, literally "related to 16", is a base-16 alphanumeric
counting system. Individual values come from the ranges 0-9 and A-F.
This means there are 256 possible two-digit values (i.e. 00 - FF).
Hexadecimal colours use this system to specify a six-digit code for Red,
Green and Blue values ("\#RRGGBB") of a colour (i.e. Pure blue:
"\#0000FF", black: "\#000000", white: "\#FFFFFF"). R can accept hex
codes as valid colours. However, you could use "yellow" as yellow color.
"red" as red.

Modifying Aesthetics
--------------------

### position = "jitter"

Add some random noise on both x and y to see regions of high density. It
can be used as a argument or a function.

### Scale Function

scale\_?*!("*", limits = *, breaks = *, expand = *, labels = *)

?: Defines which scale we want to modify(x,y,color...).

!: Match the type of data we are using(continuous or discrette).

Limits: Scale's limits.

Breaks: controls the breaks in the guide.

Expand: is a numeric vector of two, giving a multiplicative and additive
constant.

labels: label the categories.

### labs(x =*, y =*)

Quick change the axis labels.

Data Visualization with ggplot2: Geometries
===========================================

Scatter Plot
------------

-   Each geom has specific aesthetic mappings

-   geom\_point()

-   Essential: x, y

-   Optional: alpha, colour, fill, shape, size

-example:

     ggplot(iris, aes(x = Sepal.Length, y = Sepal.Width, col = Species)) +
     geom_point()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-13-1.png)

-Add layers by '+geom\_point()'. Can modify aes inside the brackets.

-crosshair: geom\_vline(data = *, linetype = *,aes(*intercept = *, col =
\_))

Bar Plots
---------

### Histogram

-plot of statistical function

-No space between

-x axis labels between the bars

-example:

    ggplot(iris, aes(x = Sepal.Width)) +
    geom_histogram()

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-14-1.png)

-stat\_bin: binwidth defaulted to range/30. Use 'binwidth = x' to adjust
this:

There will always be a message like this. We can change it anytime.

Different types of 'position':

-stack

    ggplot(iris, aes(x = Sepal.Width )) +
    geom_histogram(binwidth = 0.1,position = "stack")

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-15-1.png)

-dodge

    ggplot(iris, aes(x = Sepal.Width, fill = Species)) +
    geom_histogram(binwidth = 0.1, position = "dodge" )

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-16-1.png)

-fill

    ggplot(iris, aes(x = Sepal.Width, fill = Species)) +
    geom_histogram(binwidth = 0.1, position = "fill" )

    ## Warning: Removed 6 rows containing missing values (geom_bar).

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-17-1.png)

### Bar

-geom\_bar()

-All positions from before available

-Two types:

Absolute counts

Distributions

-Absolute counts

    ggplot(sleep, aes(extra)) +
      geom_bar()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-18-1.png)

-Distributions

    ggplot(mtcars, aes(mpg, fill = cyl)) +
      geom_histogram(binwidth = 1, position = "identity")

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-19-1.png)

-geom\_errorbar geom\_errorbar(aes(ymin = *, ymax = *), width = \_)

Line plots
----------

### geom\_line

-example:

    ggplot(economics, aes(x = date, y = unemploy/pop)) +
      geom_line()

![](1111111111111111111111111111111111111_files/figure-markdown_strict/unnamed-chunk-20-1.png)

For geom\_line():

There are several categorical variables:linetype, color, and size are
the common choices.

### fill(geom\_area)

Instead of overlapping time series, they are added together at each
point.

-position = "fill"

For each time point, we have a proportion of the total y value by each
category.

### geom\_ribbon

ggplot(fish, aes(x = *, y = *, fill = *)) + geom\_ribbon(aes(ymax = *,
ymin = 0), alpha = \_)
