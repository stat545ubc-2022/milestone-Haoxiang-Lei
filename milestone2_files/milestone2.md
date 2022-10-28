milestone2
================
HaoxiangLei
2022-10-24

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

``` r
head(cancer_sample)
```

    ## # A tibble: 6 × 32
    ##       ID diagn…¹ radiu…² textu…³ perim…⁴ area_…⁵ smoot…⁶ compa…⁷ conca…⁸ conca…⁹
    ##    <dbl> <chr>     <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ## 1 8.42e5 M          18.0    10.4   123.    1001   0.118   0.278   0.300   0.147 
    ## 2 8.43e5 M          20.6    17.8   133.    1326   0.0847  0.0786  0.0869  0.0702
    ## 3 8.43e7 M          19.7    21.2   130     1203   0.110   0.160   0.197   0.128 
    ## 4 8.43e7 M          11.4    20.4    77.6    386.  0.142   0.284   0.241   0.105 
    ## 5 8.44e7 M          20.3    14.3   135.    1297   0.100   0.133   0.198   0.104 
    ## 6 8.44e5 M          12.4    15.7    82.6    477.  0.128   0.17    0.158   0.0809
    ## # … with 22 more variables: symmetry_mean <dbl>, fractal_dimension_mean <dbl>,
    ## #   radius_se <dbl>, texture_se <dbl>, perimeter_se <dbl>, area_se <dbl>,
    ## #   smoothness_se <dbl>, compactness_se <dbl>, concavity_se <dbl>,
    ## #   concave_points_se <dbl>, symmetry_se <dbl>, fractal_dimension_se <dbl>,
    ## #   radius_worst <dbl>, texture_worst <dbl>, perimeter_worst <dbl>,
    ## #   area_worst <dbl>, smoothness_worst <dbl>, compactness_worst <dbl>,
    ## #   concavity_worst <dbl>, concave_points_worst <dbl>, symmetry_worst <dbl>, …

<!--------------------------- Start your work below --------------------------->

From the table, we can find : it contains more than 8 varibles And the 8
varibles I would like to choose are: ID, diagnosis area_mean,
area_worst, smoothness_mean, smoothness_worst, compactness_mean,
compactness_worst

cancer_ms2 \<- filter(cancer_sample, col = c(“ID”, “diagnosis”,
“area_mean”, “area_worst”, “smoothness_mean”, “smoothness_worst”,
“compactness_mean”, “compactness_worst” ))

``` r
cancer_ms2 <- select(cancer_sample, c("ID", "diagnosis", "area_mean", "area_worst", "smoothness_mean", "smoothness_worst", "compactness_mean", "compactness_worst" ))
head(cancer_ms2)
```

    ## # A tibble: 6 × 8
    ##         ID diagnosis area_mean area_worst smoothness_m…¹ smoot…² compa…³ compa…⁴
    ##      <dbl> <chr>         <dbl>      <dbl>          <dbl>   <dbl>   <dbl>   <dbl>
    ## 1   842302 M             1001       2019          0.118    0.162  0.278    0.666
    ## 2   842517 M             1326       1956          0.0847   0.124  0.0786   0.187
    ## 3 84300903 M             1203       1709          0.110    0.144  0.160    0.424
    ## 4 84348301 M              386.       568.         0.142    0.210  0.284    0.866
    ## 5 84358402 M             1297       1575          0.100    0.137  0.133    0.205
    ## 6   843786 M              477.       742.         0.128    0.179  0.17     0.525
    ## # … with abbreviated variable names ¹​smoothness_mean, ²​smoothness_worst,
    ## #   ³​compactness_mean, ⁴​compactness_worst

The data is tidy, since: Each variable forms a column. Each observation
forms a row. Each type of observational unit forms a table

<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

To untidy the data, since all the values in this data are float or
integer, and it mainly specify the characterisc of each sample of
cancer. So the meaning of this untidy process is : we try to spread all
the values of the area_worst value according to the diagnoisis of
cancer.

``` r
untidy_data <- cancer_ms2 %>%
  pivot_wider(id_cols = c(ID, area_mean,smoothness_mean, smoothness_worst,
compactness_mean, compactness_worst ), 
              names_from = diagnosis, 
              values_from = area_worst)
head(untidy_data)
```

    ## # A tibble: 6 × 8
    ##         ID area_mean smoothness_mean smoothness_wo…¹ compa…² compa…³     M     B
    ##      <dbl>     <dbl>           <dbl>           <dbl>   <dbl>   <dbl> <dbl> <dbl>
    ## 1   842302     1001           0.118            0.162  0.278    0.666 2019     NA
    ## 2   842517     1326           0.0847           0.124  0.0786   0.187 1956     NA
    ## 3 84300903     1203           0.110            0.144  0.160    0.424 1709     NA
    ## 4 84348301      386.          0.142            0.210  0.284    0.866  568.    NA
    ## 5 84358402     1297           0.100            0.137  0.133    0.205 1575     NA
    ## 6   843786      477.          0.128            0.179  0.17     0.525  742.    NA
    ## # … with abbreviated variable names ¹​smoothness_worst, ²​compactness_mean,
    ## #   ³​compactness_worst

Now, we need to tidy the untidy_data back. After tidy the data, it has
been changed back.

``` r
tidy_data <- untidy_data %>%
  pivot_longer(cols = c(-ID, -area_mean, -smoothness_mean, -smoothness_worst,
-compactness_mean, -compactness_worst),
   names_to = "diagnosis", 
   values_to = "area_worst"
)

head(tidy_data)
```

    ## # A tibble: 6 × 8
    ##         ID area_mean smoothness_mean smoothnes…¹ compa…² compa…³ diagn…⁴ area_…⁵
    ##      <dbl>     <dbl>           <dbl>       <dbl>   <dbl>   <dbl> <chr>     <dbl>
    ## 1   842302      1001          0.118        0.162  0.278    0.666 M          2019
    ## 2   842302      1001          0.118        0.162  0.278    0.666 B            NA
    ## 3   842517      1326          0.0847       0.124  0.0786   0.187 M          1956
    ## 4   842517      1326          0.0847       0.124  0.0786   0.187 B            NA
    ## 5 84300903      1203          0.110        0.144  0.160    0.424 M          1709
    ## 6 84300903      1203          0.110        0.144  0.160    0.424 B            NA
    ## # … with abbreviated variable names ¹​smoothness_worst, ²​compactness_mean,
    ## #   ³​compactness_worst, ⁴​diagnosis, ⁵​area_worst

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

<!-------------------------- Start your work below ---------------------------->

1.  Is there any relationship between diagnosis of the cancer with
    area_worst?

2.  Is there any relationship between area_worst with compactness_worst?

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

Based on the milestone1 investigation of the data, I have found there
might exist some relationship between the diagnosis and area_worst, so I
would like to use the model to check it. As for the compactness_worst
with diagonsis, based on my understanding, there should be relationship,
but I need to do more research to find out.

<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

<!--------------------------- Start your work below --------------------------->

So the data I will use in the futher for doing my research, I will store
it in “cancer”

``` r
cancer <- select(cancer_ms2, ID, diagnosis, area_worst, compactness_worst) %>%
filter(area_worst > 0)%>%
arrange(area_worst)
na.omit(cancer)
```

    ## # A tibble: 569 × 4
    ##           ID diagnosis area_worst compactness_worst
    ##        <dbl> <chr>          <dbl>             <dbl>
    ##  1    862722 B               185.            0.120 
    ##  2    921362 B               224.            0.306 
    ##  3    894047 B               240.            0.0777
    ##  4  85713702 B               242.            0.136 
    ##  5    921092 B               248             0.0834
    ##  6 871001502 B               250.            0.431 
    ##  7    872113 B               259.            0.0706
    ##  8     92751 B               269.            0.0644
    ##  9    864726 B               270             0.188 
    ## 10    858981 B               274.            0.170 
    ## # … with 559 more rows

<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

``` r
ggplot(cancer, aes(diagnosis, area_worst)) + 
    geom_boxplot(width = 0.2)
```

![](milestone2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
ggplot(cancer, aes(x = area_worst)) + 
    geom_density(aes(color = diagnosis))
```

![](milestone2_files/figure-gfm/unnamed-chunk-7-2.png)<!-- -->

<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        -   Note that you might first have to *make* a time-based column
            using a function like `ymd()`, but this doesn’t count.
        -   Examples of something you might do here: extract the day of
            the year from a date, or extract the weekday, or let 24
            hours elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        -   For example, you could say something like “Investigating the
            day of the week might be insightful because penguins don’t
            work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**Task 1**:

<!----------------------------------------------------------------------------->

I used fct_recode function in “forcats” package, since B and M are not
intuitive for people who first check this research, I change the name.
However, I really can not find varibles that could be reordered.

``` r
cancer <- cancer %>%
mutate(diagnosis = fct_recode(diagnosis, "Maligant" = "M",
                               "Benign" = "B"))
ggplot(cancer, aes(diagnosis, area_worst)) + 
    geom_boxplot(width = 0.2)
```

![](milestone2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

<!-------------------------- Start your work below ---------------------------->

**Task 2**:

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: Is there any relationship between area_worst with
compactness_worst?

**Variable of interest**: Y(area_worst), and X(compactness_worst)
<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

``` r
model <-  lm(area_worst~ compactness_worst, cancer)
print(model)
```

    ## 
    ## Call:
    ## lm(formula = area_worst ~ compactness_worst, data = cancer)
    ## 
    ## Coefficients:
    ##       (Intercept)  compactness_worst  
    ##             477.3             1586.1

``` r
unclass(model)
```

    ## $coefficients
    ##       (Intercept) compactness_worst 
    ##          477.3004         1586.0723 
    ## 
    ## $residuals
    ##             1             2             3             4             5 
    ## -4.827463e+02 -7.396729e+02 -3.603906e+02 -4.503304e+02 -3.615788e+02 
    ##             6             7             8             9            10 
    ## -9.110976e+02 -3.300295e+02 -3.109069e+02 -5.053234e+02 -4.727155e+02 
    ##            11            12            13            14            15 
    ## -3.989147e+02 -5.561875e+02 -5.792676e+02 -5.678479e+02 -2.747837e+02 
    ##            16            17            18            19            20 
    ## -3.131127e+02 -7.746603e+02 -7.228679e+02 -4.755922e+02 -3.732422e+02 
    ##            21            22            23            24            25 
    ## -2.167843e+02 -4.333433e+02 -3.444815e+02 -3.680759e+02 -8.449210e+02 
    ##            26            27            28            29            30 
    ## -4.832997e+02 -7.301787e+02 -2.602314e+02 -4.105228e+02 -3.434836e+02 
    ##            31            32            33            34            35 
    ## -2.923152e+02 -2.486610e+02 -3.203291e+02 -3.032958e+02 -6.994000e+02 
    ##            36            37            38            39            40 
    ## -6.655027e+02 -2.707154e+02 -3.796818e+02 -4.761322e+02 -3.909187e+02 
    ##            41            42            43            44            45 
    ## -3.071188e+02 -4.812142e+02 -2.512246e+02 -2.645068e+02 -2.614490e+02 
    ##            46            47            48            49            50 
    ## -2.147612e+02 -3.902421e+02 -2.651823e+02 -4.278866e+02 -2.674863e+02 
    ##            51            52            53            54            55 
    ## -3.300948e+02 -2.135163e+02 -4.565564e+02 -2.961525e+02 -5.364596e+02 
    ##            56            57            58            59            60 
    ## -3.108374e+02 -3.156677e+02 -2.326409e+02 -3.366727e+02 -5.549917e+02 
    ##            61            62            63            64            65 
    ## -4.612134e+02 -2.041510e+02 -3.445023e+02 -2.647745e+02 -2.114927e+02 
    ##            66            67            68            69            70 
    ## -3.101907e+02 -2.372314e+02 -2.951747e+02 -4.679045e+02 -1.767105e+02 
    ##            71            72            73            74            75 
    ## -4.713488e+02 -3.883317e+02 -1.815450e+02 -2.626697e+02 -1.548382e+02 
    ##            76            77            78            79            80 
    ## -4.513976e+02 -6.624514e+02 -3.322828e+02 -1.638927e+02 -1.945577e+02 
    ##            81            82            83            84            85 
    ## -3.233385e+02 -2.019211e+02 -3.320135e+02 -2.040794e+02 -1.248392e+02 
    ##            86            87            88            89            90 
    ## -3.878224e+02 -5.120876e+02 -1.787869e+02 -2.300770e+02 -3.445599e+02 
    ##            91            92            93            94            95 
    ## -4.728281e+02 -2.521259e+02 -2.230111e+02 -2.433333e+02 -2.588215e+02 
    ##            96            97            98            99           100 
    ## -3.554305e+02 -1.358450e+02 -1.950372e+02 -1.719763e+02 -2.766851e+02 
    ##           101           102           103           104           105 
    ## -1.556943e+02 -2.654580e+02 -3.285009e+02 -2.673957e+02 -1.388492e+02 
    ##           106           107           108           109           110 
    ## -1.156015e+02 -2.409563e+02 -1.376229e+02 -2.656507e+02 -7.733282e+02 
    ##           111           112           113           114           115 
    ## -4.464904e+02 -4.023148e+02 -3.457781e+02 -1.791405e+02 -3.864573e+02 
    ##           116           117           118           119           120 
    ## -6.483351e+02 -2.166648e+02 -2.483482e+02 -1.666064e+02 -5.052325e+02 
    ##           121           122           123           124           125 
    ## -4.986640e+02 -1.464076e+02 -2.567982e+02 -2.712487e+02 -2.364170e+02 
    ##           126           127           128           129           130 
    ## -1.399403e+02 -2.689623e+02 -1.305250e+02 -2.800750e+02 -2.786234e+02 
    ##           131           132           133           134           135 
    ## -2.585245e+02 -2.307060e+02 -1.094059e+02 -5.092064e+02 -1.456778e+03 
    ##           136           137           138           139           140 
    ## -1.347794e+02 -3.669148e+02 -3.458717e+02 -2.583824e+02 -4.876975e+01 
    ##           141           142           143           144           145 
    ## -2.523828e+02 -3.912227e+02 -3.291761e+02 -3.784788e+02 -3.613610e+01 
    ##           146           147           148           149           150 
    ## -1.377474e+02 -9.744051e+01 -2.383319e+02 -3.075019e+02 -2.640226e+02 
    ##           151           152           153           154           155 
    ## -2.132613e+02 -1.207519e+02 -3.774956e+02 -3.012744e+02 -7.111112e+01 
    ##           156           157           158           159           160 
    ## -2.479754e+02 -2.180261e+02 -9.135598e+01 -3.067667e+02 -2.716317e+02 
    ##           161           162           163           164           165 
    ## -4.056720e+02 -3.329746e+02 -4.292180e+02 -1.399045e+02 -5.116509e+01 
    ##           166           167           168           169           170 
    ## -7.841652e+01 -4.603028e+01 -3.751442e+01 -5.723336e+01 -2.334853e+02 
    ##           171           172           173           174           175 
    ## -2.445464e+02 -1.165854e+01 -7.657164e+01 -3.421001e+02 -4.661055e+00 
    ##           176           177           178           179           180 
    ## -3.773625e+02 -2.291820e+02 -9.364861e+01 -1.923023e+02 -2.474321e+02 
    ##           181           182           183           184           185 
    ## -2.913663e+02 -7.005810e+01 -1.289971e+02 -1.617633e+02 -1.862474e+02 
    ##           186           187           188           189           190 
    ## -3.968122e+02 -2.682748e+02 -2.030216e+02 -2.190582e+02 -2.951241e+02 
    ##           191           192           193           194           195 
    ## -5.017378e+01 -1.570702e+02 -3.439294e+01 -1.082325e+00 -1.990110e+02 
    ##           196           197           198           199           200 
    ## -1.890565e+02 -1.291374e+02 -1.826181e+01 -1.966901e+02 -2.413518e+02 
    ##           201           202           203           204           205 
    ## -1.652822e+02 -6.396177e+01 -1.283615e+03 -2.270907e+02  7.037987e+00 
    ##           206           207           208           209           210 
    ## -9.513234e+01 -2.932500e+02 -2.145497e+02 -8.222287e+01 -4.019795e+01 
    ##           211           212           213           214           215 
    ## -1.892791e+02 -5.408044e+02 -2.069533e+02 -2.366576e+02 -2.747991e+02 
    ##           216           217           218           219           220 
    ## -1.999951e+02 -2.199001e+02  3.977028e+01 -4.284262e+01 -1.376068e+02 
    ##           221           222           223           224           225 
    ## -1.736967e+02 -3.076784e+02 -5.346212e+02 -5.135847e+01 -2.491933e+02 
    ##           226           227           228           229           230 
    ## -1.385441e+02 -1.665623e+02 -2.316531e+02 -4.466762e+02 -1.155629e+02 
    ##           231           232           233           234           235 
    ## -1.144801e+02 -9.051975e+01 -2.854653e+02 -2.674701e+02  7.706562e+01 
    ##           236           237           238           239           240 
    ## -1.518004e+02 -1.585135e+02 -6.222876e+02 -3.586137e+01 -2.087051e+02 
    ##           241           242           243           244           245 
    ## -1.204986e+02 -2.230519e+02 -1.947785e+02 -1.405278e+02 -2.794608e+02 
    ##           246           247           248           249           250 
    ## -4.011298e+02  4.860589e+01 -1.670841e+02 -1.968781e+02 -1.124195e+02 
    ##           251           252           253           254           255 
    ## -2.215808e+01 -6.673696e+01 -2.870941e+02 -2.466353e+02 -1.299986e+01 
    ##           256           257           258           259           260 
    ## -8.440755e+01 -4.529694e+02 -3.589497e+02 -5.388571e+01 -1.627763e+02 
    ##           261           262           263           264           265 
    ## -7.658941e+02 -5.361894e+02 -2.376316e+02 -1.385640e+02 -9.006788e+01 
    ##           266           267           268           269           270 
    ## -4.034064e+01 -6.525919e+01 -7.151702e+01 -3.126172e+02 -2.134599e+02 
    ##           271           272           273           274           275 
    ##  1.377267e+01 -9.019089e+01 -7.962610e+01 -8.073925e+01 -1.391411e+02 
    ##           276           277           278           279           280 
    ##  1.023620e+02 -2.722364e+02 -2.097210e+02 -1.578183e+02 -1.765684e+02 
    ##           281           282           283           284           285 
    ## -2.779111e+01  1.337687e+00  1.195412e+01  1.274250e+02 -4.285711e+00 
    ##           286           287           288           289           290 
    ## -2.186227e+02 -1.013636e+02 -1.117833e+02  5.029607e+01 -3.510974e+02 
    ##           291           292           293           294           295 
    ##  5.722353e+01 -1.004841e+03  1.288364e+02  1.401183e+02 -4.987735e+01 
    ##           296           297           298           299           300 
    ## -5.562164e+01 -1.823865e+02  2.918746e+00 -6.187388e+02 -1.200588e-02 
    ##           301           302           303           304           305 
    ## -1.634947e+02  5.943128e+01 -1.296872e+02  2.356554e+01 -2.708095e+02 
    ##           306           307           308           309           310 
    ## -4.333405e+02 -4.731100e+01 -1.443965e+03 -1.773120e+00 -7.656649e+01 
    ##           311           312           313           314           315 
    ## -2.529894e+02  3.447986e+01 -4.069997e+01 -1.824739e+02  1.189245e+01 
    ##           316           317           318           319           320 
    ## -6.392968e+02 -2.467439e+02  2.573306e+01  1.409092e+02 -5.946380e+02 
    ##           321           322           323           324           325 
    ## -4.070159e+02 -3.558030e+02 -5.682297e+02  7.243691e+01 -1.539011e+01 
    ##           326           327           328           329           330 
    ##  9.082877e+01  9.670420e+01  3.817813e+01  7.412337e+01 -4.203140e+02 
    ##           331           332           333           334           335 
    ##  3.014159e+01 -3.786581e+02  1.225477e+02 -1.194230e+03 -1.719650e+02 
    ##           336           337           338           339           340 
    ## -9.808152e+01 -1.441912e+02 -1.020775e+02 -4.548060e+02  4.803791e+01 
    ##           341           342           343           344           345 
    ##  5.435841e+01  1.462717e+02  8.615278e+01 -4.298068e+02 -1.343495e+01 
    ##           346           347           348           349           350 
    ## -3.183367e+02  5.675958e+01 -3.498778e+02 -1.636381e+02  4.629925e+01 
    ##           351           352           353           354           355 
    ## -1.655722e+02 -2.443688e+02  1.819173e+01 -3.348680e+02 -2.446377e+02 
    ##           356           357           358           359           360 
    ## -6.744921e+01 -1.480803e+02 -3.097871e+02 -2.266872e+02  6.629886e+01 
    ##           361           362           363           364           365 
    ## -1.302251e+00 -1.686772e+02 -1.308459e+02  1.308561e+02 -3.002314e+02 
    ##           366           367           368           369           370 
    ## -1.926646e+02  5.385942e+01 -6.695094e+01  1.287704e+02  1.569439e+02 
    ##           371           372           373           374           375 
    ##  3.686098e+01  2.538265e+01 -7.691257e+02  2.510947e+02 -2.444899e+02 
    ##           376           377           378           379           380 
    ## -8.602364e+01 -9.549133e+01  3.447028e+01 -2.024882e+01  7.890488e+01 
    ##           381           382           383           384           385 
    ## -1.498654e+02 -1.407416e+02 -1.739110e+02  1.540236e+02  9.403931e+01 
    ##           386           387           388           389           390 
    ##  8.485769e+01 -2.331044e+02 -5.045572e+02 -6.340282e+02 -1.642922e+02 
    ##           391           392           393           394           395 
    ##  1.210258e+02 -1.337317e+01 -1.607537e+02 -3.557025e+02 -8.179938e+02 
    ##           396           397           398           399           400 
    ## -3.623325e+02  1.363741e+02 -9.400269e+01  2.350352e+02 -2.627088e+02 
    ##           401           402           403           404           405 
    ##  2.976177e+02  1.086842e+02 -2.453059e+02 -5.772602e+02  1.974780e+02 
    ##           406           407           408           409           410 
    ## -5.086198e+02  1.564658e+02  1.508736e+02 -2.744564e+01  1.622867e+02 
    ##           411           412           413           414           415 
    ## -4.407478e+02 -2.363203e+02 -1.722254e+02  2.246964e+02 -1.612107e+01 
    ##           416           417           418           419           420 
    ##  2.710928e+02  2.653981e+02  2.593169e+02 -1.189266e+02  2.264445e+02 
    ##           421           422           423           424           425 
    ## -1.927060e+02  2.834813e+02 -3.247910e+02 -7.661132e+01  1.990210e+02 
    ##           426           427           428           429           430 
    ## -1.175436e+02  1.435665e+02 -9.986078e+01 -9.286078e+01  5.640990e+01 
    ##           431           432           433           434           435 
    ##  1.762841e+02  1.559688e+02  3.639455e+02  4.266998e+02 -1.504855e+02 
    ##           436           437           438           439           440 
    ##  2.989939e+02 -5.220312e+01  2.131545e+02  8.773877e+01  4.970093e+02 
    ##           441           442           443           444           445 
    ##  5.744730e+02  5.050093e+02  3.632492e+02  4.213478e+02 -7.161699e+01 
    ##           446           447           448           449           450 
    ##  3.036961e+02  4.393536e+02  1.434409e+02  4.010538e+02  5.747152e+02 
    ##           451           452           453           454           455 
    ##  2.192629e+02  4.466109e+02 -1.368568e+02  5.930537e+02 -1.773906e+02 
    ##           456           457           458           459           460 
    ##  2.291198e+02  1.355900e+01  1.529206e+02  3.918527e+02  3.629727e+02 
    ##           461           462           463           464           465 
    ##  3.739030e+02 -6.792835e+01  2.698915e+02 -1.117794e+02  4.528218e+02 
    ##           466           467           468           469           470 
    ##  1.663152e+02  5.562550e+02  4.793788e+02  2.356556e+02  3.022165e+02 
    ##           471           472           473           474           475 
    ##  2.793016e+02  2.610695e+02  5.590790e+02  5.899281e+02  3.383016e+02 
    ##           476           477           478           479           480 
    ##  5.449049e+02  4.689534e+02  6.510693e+02  5.796206e+02  5.515549e+02 
    ##           481           482           483           484           485 
    ##  3.630192e+02  2.007640e+02  3.573597e+02  5.350771e+02  5.694678e+02 
    ##           486           487           488           489           490 
    ##  6.134717e+02  6.979474e+02  6.072551e+02  7.016128e+02  5.142358e+02 
    ##           491           492           493           494           495 
    ##  7.083305e+02  2.823888e+02  7.725548e+02  4.993171e+02  7.518682e+02 
    ##           496           497           498           499           500 
    ##  6.327619e+02  7.943691e+02  5.764428e+02  7.201274e+02  2.391065e+02 
    ##           501           502           503           504           505 
    ##  1.680446e+02  6.372048e+02  5.560811e+02  7.657889e+02  6.438683e+02 
    ##           506           507           508           509           510 
    ##  7.830635e+02 -6.972628e+00  7.155665e+02  9.359203e+02  5.752339e+02 
    ##           511           512           513           514           515 
    ##  5.175086e+02  6.518973e+02  3.095773e+01  3.223405e+02  6.808625e+02 
    ##           516           517           518           519           520 
    ##  5.584119e+02  6.374892e+02  9.488565e+02  6.718877e+02  6.359535e+02 
    ##           521           522           523           524           525 
    ##  7.113886e+02  9.061583e+02  7.121894e+02  9.258488e+02  8.344659e+02 
    ##           526           527           528           529           530 
    ##  2.904780e+02  6.777697e+02 -3.316978e+01  8.996013e+02  1.018193e+03 
    ##           531           532           533           534           535 
    ##  9.567851e+02  6.301025e+02  6.034817e+02  5.111935e+02  7.482359e+02 
    ##           536           537           538           539           540 
    ##  9.841545e+02  1.182739e+03  9.933422e+02  8.477833e+02  1.018605e+03 
    ##           541           542           543           544           545 
    ##  1.029597e+03  4.860099e+02  1.052859e+03  1.214563e+03  9.228722e+02 
    ##           546           547           548           549           550 
    ##  9.225705e+02  6.969421e+02  4.088224e+02  1.110830e+03  1.170203e+03 
    ##           551           552           553           554           555 
    ##  1.133511e+03  1.144696e+03  7.729248e+02  1.157280e+03  1.421087e+03 
    ##           556           557           558           559           560 
    ##  1.357410e+03  1.349886e+03  1.781251e+03  1.121002e+03  1.725321e+03 
    ##           561           562           563           564           565 
    ##  1.500453e+03  1.729748e+03  2.003949e+03  1.812286e+03  2.407487e+03 
    ##           566           567           568           569 
    ##  2.098878e+03  1.815048e+03  2.535342e+03  3.101667e+03 
    ## 
    ## $effects
    ##       (Intercept) compactness_worst                                     
    ##     -2.100518e+04     -5.947393e+03     -3.733543e+02     -4.523161e+02 
    ##                                                                         
    ##     -3.734585e+02     -8.572195e+02     -3.443363e+02     -3.263733e+02 
    ##                                                                         
    ##     -4.974341e+02     -4.682503e+02     -4.031328e+02     -5.415636e+02 
    ##                                                                         
    ##     -5.608412e+02     -5.507836e+02     -2.925430e+02     -3.250718e+02 
    ##                                                                         
    ##     -7.314140e+02     -6.854293e+02     -4.675516e+02     -3.772900e+02 
    ##                                                                         
    ##     -2.392787e+02     -4.292565e+02     -3.504210e+02     -3.709507e+02 
    ##                                                                         
    ##     -7.900025e+02     -4.712864e+02     -6.885404e+02     -2.743472e+02 
    ##                                                                         
    ##     -4.067009e+02     -3.475503e+02     -3.019720e+02     -2.626898e+02 
    ##                                                                         
    ##     -3.252849e+02     -3.099731e+02     -6.585940e+02     -6.285370e+02 
    ##                                                                         
    ##     -2.806465e+02     -3.763895e+02     -4.613002e+02     -3.862266e+02 
    ##                                                                         
    ##     -3.122071e+02     -4.653228e+02     -2.625860e+02     -2.741051e+02 
    ##                                                                         
    ##     -2.711853e+02     -2.300423e+02     -3.845095e+02     -2.741751e+02 
    ##                                                                         
    ##     -4.167813e+02     -2.753933e+02     -3.305104e+02     -2.277532e+02 
    ##                                                                         
    ##     -4.417811e+02     -3.000678e+02     -5.116769e+02     -3.129177e+02 
    ##                                                                         
    ##     -3.168022e+02     -2.435709e+02     -3.351587e+02     -5.268417e+02 
    ##                                                                         
    ##     -4.442248e+02     -2.176084e+02     -3.409452e+02     -2.706195e+02 
    ##                                                                         
    ##     -2.235616e+02     -3.097361e+02     -2.454789e+02     -2.964037e+02 
    ##                                                                         
    ##     -4.480783e+02     -1.915206e+02     -4.507658e+02     -3.775669e+02 
    ##                                                                         
    ##     -1.953013e+02     -2.665472e+02     -1.707303e+02     -4.314767e+02 
    ##                                                                         
    ##     -6.163674e+02     -3.254718e+02     -1.769396e+02     -2.039232e+02 
    ##                                                                         
    ##     -3.172842e+02     -2.103011e+02     -3.248053e+02     -2.118918e+02 
    ##                                                                         
    ##     -1.421028e+02     -3.736714e+02     -4.830674e+02     -1.894729e+02 
    ##                                                                         
    ##     -2.345977e+02     -3.347221e+02     -4.473454e+02     -2.526929e+02 
    ##                                                                         
    ##     -2.269075e+02     -2.445434e+02     -2.579696e+02     -3.430199e+02 
    ##                                                                         
    ##     -1.496013e+02     -2.017334e+02     -1.812680e+02     -2.731658e+02 
    ##                                                                         
    ##     -1.660454e+02     -2.626576e+02     -3.181335e+02     -2.642926e+02 
    ##                                                                         
    ##     -1.508159e+02     -1.302337e+02     -2.405773e+02     -1.494496e+02 
    ##                                                                         
    ##     -2.622071e+02     -7.092725e+02     -4.213104e+02     -3.823561e+02 
    ##                                                                         
    ##     -3.323838e+02     -1.856097e+02     -3.681634e+02     -5.987703e+02 
    ##                                                                         
    ##     -2.185749e+02     -2.462288e+02     -1.737000e+02     -4.713882e+02 
    ##                                                                         
    ##     -4.655197e+02     -1.551470e+02     -2.523709e+02     -2.650621e+02 
    ##                                                                         
    ##     -2.341463e+02     -1.490694e+02     -2.624162e+02     -1.404789e+02 
    ##                                                                         
    ##     -2.720723e+02     -2.707341e+02     -2.522811e+02     -2.274705e+02 
    ##                                                                         
    ##     -1.205667e+02     -4.725623e+02     -1.307006e+03     -1.425918e+02 
    ##                                                                         
    ##     -3.469561e+02     -3.283155e+02     -2.510228e+02     -6.633986e+01 
    ##                                                                         
    ##     -2.455718e+02     -3.678399e+02     -3.130388e+02     -3.564013e+02 
    ##                                                                         
    ##     -5.487910e+01     -1.443113e+02     -1.088000e+02     -2.327317e+02 
    ##                                                                         
    ##     -2.936157e+02     -2.550361e+02     -2.102339e+02     -1.287345e+02 
    ##                                                                         
    ##     -3.548317e+02     -2.875585e+02     -8.478798e+01     -2.405213e+02 
    ##                                                                         
    ##     -2.139771e+02     -1.021649e+02     -2.918590e+02     -2.608669e+02 
    ##                                                                         
    ##     -3.788840e+02     -3.146428e+02     -3.992519e+02     -1.442549e+02 
    ##                                                                         
    ##     -6.606403e+01     -8.999352e+01     -6.143432e+01     -5.292035e+01 
    ##                                                                         
    ##     -7.018000e+01     -2.253501e+02     -2.350680e+02     -2.994562e+01 
    ##                                                                         
    ##     -8.710439e+01     -3.208550e+02     -2.357999e+01     -3.518042e+02 
    ##                                                                         
    ##     -2.212738e+02     -1.018583e+02     -1.887452e+02     -2.372159e+02 
    ##                                                                         
    ##     -2.759100e+02     -8.095029e+01     -1.327044e+02     -1.614789e+02 
    ##                                                                         
    ##     -1.830308e+02     -3.683973e+02     -2.551075e+02     -1.975538e+02 
    ##                                                                         
    ##     -2.116419e+02     -2.785516e+02     -6.274585e+01     -1.568805e+02 
    ##                                                                         
    ##     -4.869219e+01     -1.887755e+01     -1.931270e+02     -1.840616e+02 
    ##                                                                         
    ##     -1.312177e+02     -3.338775e+01     -1.905224e+02     -2.297736e+02 
    ##                                                                         
    ##     -1.625385e+02     -7.321947e+01     -1.147388e+03     -2.168556e+02 
    ##                                                                         
    ##     -1.031835e+01     -9.986116e+01     -2.743128e+02     -2.048443e+02 
    ##                                                                         
    ##     -8.818133e+01     -5.087069e+01     -1.821465e+02     -4.916368e+02 
    ##                                                                         
    ##     -1.975696e+02     -2.235281e+02     -2.570727e+02     -1.911789e+02 
    ##                                                                         
    ##     -2.084355e+02      2.029895e+01     -5.232922e+01     -1.354685e+02 
    ##                                                                         
    ##     -1.670749e+02     -2.850523e+02     -4.848672e+02     -5.888710e+01 
    ##                                                                         
    ##     -2.330182e+02     -1.355545e+02     -1.600162e+02     -2.171049e+02 
    ##                                                                         
    ##     -4.063621e+02     -1.147299e+02     -1.136093e+02     -9.208932e+01 
    ##                                                                         
    ##     -2.637472e+02     -2.477195e+02      5.590116e+01     -1.455002e+02 
    ##                                                                         
    ##     -1.513053e+02     -5.593102e+02     -4.261437e+01     -1.945920e+02 
    ##                                                                         
    ##     -1.166199e+02     -2.068578e+02     -1.819328e+02     -1.340574e+02 
    ##                                                                         
    ##     -2.563239e+02     -3.634452e+02      3.266080e+01     -1.573031e+02 
    ##                                                                         
    ##     -1.834838e+02     -1.088246e+02     -2.913809e+01     -6.826870e+01 
    ##                                                                         
    ##     -2.622357e+02     -2.264117e+02     -2.054741e+01     -8.336648e+01 
    ##                                                                         
    ##     -4.079448e+02     -3.250676e+02     -5.607956e+01     -1.518035e+02 
    ##                                                                         
    ##     -6.829018e+02     -4.805710e+02     -2.170865e+02     -1.295964e+02 
    ##                                                                         
    ##     -8.658644e+01     -4.219398e+01     -6.410462e+01     -6.924631e+01 
    ##                                                                         
    ##     -2.815539e+02     -1.938417e+02      6.395387e+00     -8.512037e+01 
    ##                                                                         
    ##     -7.557713e+01     -7.578223e+01     -1.271468e+02      8.576613e+01 
    ##                                                                         
    ##     -2.438783e+02     -1.887596e+02     -1.427971e+02     -1.592392e+02 
    ##                                                                         
    ##     -2.788511e+01     -1.896635e+00      7.584741e+00      1.092950e+02 
    ##                                                                         
    ##     -6.479563e+00     -1.952399e+02     -9.184742e+01     -1.009051e+02 
    ##                                                                         
    ##      4.187831e+01     -3.116157e+02      4.863552e+01     -8.863595e+02 
    ##                                                                         
    ##      1.122198e+02      1.221679e+02     -4.516627e+01     -5.015385e+01 
    ##                                                                         
    ##     -1.615009e+02      2.143718e+00     -5.453189e+02     -3.897637e-01 
    ##                                                                         
    ##     -1.443494e+02      5.203508e+01     -1.145146e+02      2.070957e+01 
    ##                                                                         
    ##     -2.385543e+02     -3.816758e+02     -4.142702e+01     -1.271473e+03 
    ##                                                                         
    ##     -8.077251e-01     -6.627470e+01     -2.215477e+02      3.236168e+01 
    ##                                                                         
    ##     -3.385119e+01     -1.584290e+02      1.293352e+01     -5.601448e+02 
    ##                                                                         
    ##     -2.144131e+02      2.569582e+01      1.276712e+02     -5.201209e+02 
    ##                                                                         
    ##     -3.547459e+02     -3.096055e+02     -4.965881e+02      6.810536e+01 
    ##                                                                         
    ##     -9.222364e+00      8.475680e+01      9.002687e+01      3.848141e+01 
    ##                                                                         
    ##      7.015126e+01     -3.653009e+02      3.239338e+01     -3.275042e+02 
    ##                                                                         
    ##      1.140165e+02     -1.045442e+03     -1.450825e+02     -7.984433e+01 
    ##                                                                         
    ##     -1.201084e+02     -8.297005e+01     -3.934366e+02      4.996811e+01 
    ##                                                                         
    ##      5.602376e+01      1.375229e+02      8.475347e+01     -3.695346e+02 
    ##                                                                         
    ##     -2.443211e+00     -2.709663e+02      6.007077e+01     -2.973619e+02 
    ##                                                                         
    ##     -1.328585e+02      5.205081e+01     -1.341683e+02     -2.034115e+02 
    ##                                                                         
    ##      2.791599e+01     -2.830330e+02     -2.035290e+02     -4.741484e+01 
    ##                                                                         
    ##     -1.184168e+02     -2.606574e+02     -1.873379e+02      7.150181e+01 
    ##                                                                         
    ##      1.203528e+01     -1.346248e+02     -1.012581e+02      1.292297e+02 
    ##                                                                         
    ##     -2.503450e+02     -1.554530e+02      6.178650e+01     -4.449511e+01 
    ##                                                                         
    ##      1.278819e+02      1.527069e+02      4.698251e+01      3.688517e+01 
    ##                                                                         
    ##     -6.626566e+02      2.368446e+02     -1.992004e+02     -5.912216e+01 
    ##                                                                         
    ##     -6.679267e+01      4.797807e+01      3.341639e-01      8.772109e+01 
    ##                                                                         
    ##     -1.137510e+02     -1.051191e+02     -1.340698e+02      1.552160e+02 
    ##                                                                         
    ##      1.027799e+02      9.520624e+01     -1.839368e+02     -4.229648e+02 
    ##                                                                         
    ##     -5.360154e+02     -1.222945e+02      1.301257e+02      1.176897e+01 
    ##                                                                         
    ##     -1.179614e+02     -2.893956e+02     -6.958799e+02     -2.945311e+02 
    ##                                                                         
    ##      1.455876e+02     -5.703703e+01      2.331251e+02     -2.051796e+02 
    ##                                                                         
    ##      2.886249e+02      1.223812e+02     -1.885523e+02     -4.804958e+02 
    ##                                                                         
    ##      2.023971e+02     -4.180983e+02      1.685548e+02      1.641543e+02 
    ##                                                                         
    ##      7.306656e+00      1.746594e+02     -3.557692e+02     -1.756887e+02 
    ##                                                                         
    ##     -1.187257e+02      2.312235e+02      1.923659e+01      2.726825e+02 
    ##                                                                         
    ##      2.695038e+02      2.655792e+02     -6.707274e+01      2.377011e+02 
    ##                                                                         
    ##     -1.313366e+02      2.881734e+02     -2.471903e+02     -2.753837e+01 
    ##                                                                         
    ##      2.159339e+02     -6.048738e+01      1.711490e+02     -4.276676e+01 
    ##                                                                         
    ##     -3.576676e+01      9.653481e+01      2.043774e+02      1.868430e+02 
    ##                                                                         
    ##      3.716834e+02      4.283841e+02     -7.960047e+01      3.166257e+02 
    ##                                                                         
    ##      8.390668e+00      2.432909e+02      1.345038e+02      4.974639e+02 
    ##                                                                         
    ##      5.656883e+02      5.054639e+02      3.810892e+02      4.323774e+02 
    ##                                                                         
    ##     -1.431957e+00      3.292355e+02      4.488320e+02      1.883330e+02 
    ##                                                                         
    ##      4.156967e+02      5.690027e+02      2.563042e+02      4.590404e+02 
    ##                                                                         
    ##     -5.471578e+01      5.881357e+02     -8.946080e+01      2.685636e+02 
    ##                                                                         
    ##      7.907137e+01      2.032421e+02      4.146302e+02      3.895526e+02 
    ##                                                                         
    ##      3.995370e+02      1.052369e+01      3.084084e+02     -2.750071e+01 
    ##                                                                         
    ##      4.708319e+02      2.187366e+02      5.627632e+02      4.951756e+02 
    ##                                                                         
    ##      2.818341e+02      3.418874e+02      3.223020e+02      3.074372e+02 
    ##                                                                         
    ##      5.702599e+02      6.023198e+02      3.813020e+02      5.635016e+02 
    ##                                                                         
    ##      4.974439e+02      6.583153e+02      5.959849e+02      5.724594e+02 
    ##                                                                         
    ##      4.065304e+02      2.636279e+02      4.044084e+02      5.630758e+02 
    ##                                                                         
    ##      5.940803e+02      6.337898e+02      7.084284e+02      6.333242e+02 
    ##                                                                         
    ##      7.167854e+02      5.519960e+02      7.240138e+02      3.510226e+02 
    ##                                                                         
    ##      7.836790e+02      5.447011e+02      7.672488e+02      6.635415e+02 
    ##                                                                         
    ##      8.062311e+02      6.142976e+02      7.412022e+02      3.180314e+02 
    ##                                                                         
    ##      2.569960e+02      6.701978e+02      5.993463e+02      7.860692e+02 
    ##                                                                         
    ##      6.788098e+02      8.016413e+02      1.061933e+02      7.431490e+02 
    ##                                                                         
    ##      9.373397e+02      6.200315e+02      5.703840e+02      6.888630e+02 
    ##                                                                         
    ##      1.431778e+02      4.006412e+02      7.173552e+02      6.110603e+02 
    ##                                                                         
    ##      6.824949e+02      9.575592e+02      7.146989e+02      6.840049e+02 
    ##                                                                         
    ##      7.506812e+02      9.222200e+02      7.525792e+02      9.431402e+02 
    ##                                                                         
    ##      8.661158e+02      3.874882e+02      7.293020e+02      1.033973e+02 
    ##                                                                         
    ##      9.276568e+02      1.034728e+03      9.813597e+02      6.937610e+02 
    ##                                                                         
    ##      6.766368e+02      5.961911e+02      8.055570e+02      1.014291e+03 
    ##                                                                         
    ##      1.190382e+03      1.025484e+03      8.989561e+02      1.052147e+03 
    ##                                                                         
    ##      1.061947e+03      5.842687e+02      1.083865e+03      1.226879e+03 
    ##                                                                         
    ##      9.730803e+02      9.752000e+02      7.774372e+02      5.246367e+02 
    ##                                                                         
    ##      1.149592e+03      1.210233e+03      1.179349e+03      1.189796e+03 
    ##                                                                         
    ##      8.776345e+02      1.219009e+03      1.453020e+03      1.397535e+03 
    ##                                                                         
    ##      1.399734e+03      1.782273e+03      1.208289e+03      1.746850e+03 
    ##                                                                         
    ##      1.552023e+03      1.770668e+03      2.026954e+03      1.862683e+03 
    ##                                                                         
    ##      2.410628e+03      2.147535e+03      1.899706e+03      2.557703e+03 
    ##                   
    ##      3.154524e+03 
    ## 
    ## $rank
    ## [1] 2
    ## 
    ## $fitted.values
    ##         1         2         3         4         5         6         7         8 
    ##  667.9463  963.2729  600.4906  692.5304  609.5788 1160.8976  589.2295  579.5069 
    ##         9        10        11        12        13        14        15        16 
    ##  775.3234  746.6155  673.8147  831.7875  863.6676  852.2479  560.2837  608.9127 
    ##        17        18        19        20        21        22        23        24 
    ## 1071.7603 1023.0679  776.5922  675.2422  520.5843  743.4433  659.3815  685.0759 
    ##        25        26        27        28        29        30        31        32 
    ## 1169.6210  809.8997 1058.2787  590.8314  741.2228  675.0836  628.2152  591.5610 
    ##        33        34        35        36        37        38        39        40 
    ##  667.6291  653.1958 1051.3000 1019.1027  625.9154  736.7818  833.5322  748.5187 
    ##        41        42        43        44        45        46        47        48 
    ##  666.5188  842.4142  613.9246  628.7068  627.5490  581.0612  757.2421  633.7823 
    ##        49        50        51        52        53        54        55        56 
    ##  802.2866  642.8863  705.6948  589.8163  833.0564  676.3525  916.9596  691.7374 
    ##        57        58        59        60        61        62        63        64 
    ##  699.6677  617.5409  721.8727  945.1917  851.6134  596.3510  739.0023  660.1745 
    ##        65        66        67        68        69        70        71        72 
    ##  607.9927  712.9907  640.0314  698.8747  875.4045  585.0105  881.7488  799.4317 
    ##        73        74        75        76        77        78        79        80 
    ##  593.8450  676.6697  575.9382  876.1976 1095.5514  766.2828  599.7927  630.6577 
    ##        81        82        83        84        85        86        87        88 
    ##  759.9385  638.9211  769.6135  643.6794  564.4392  827.8224  952.4876  619.5869 
    ##        89        90        91        92        93        94        95        96 
    ##  671.2770  791.6599  922.8281  704.4259  676.5111  699.0333  716.3215  813.2305 
    ##        97        98        99       100       101       102       103       104 
    ##  593.8450  653.0372  631.2763  738.6851  622.3943  732.6580  796.1009  735.1957 
    ##       105       106       107       108       109       110       111       112 
    ##  608.8492  586.5015  712.3563  610.0229  738.0507 1246.2282  920.2904  876.5148 
    ##       113       114       115       116       117       118       119       120 
    ##  821.4781  654.9405  862.5573 1124.7351  693.1648  726.9482  649.7064  992.9325 
    ##       121       122       123       124       125       126       127       128 
    ##  987.0640  635.9076  746.2982  761.0487  728.2170  632.6403  764.0623  625.7250 
    ##       129       130       131       132       133       134       135       136 
    ##  776.2750  775.3234  761.5245  736.3060  615.6059 1016.4064 1964.8776  643.6794 
    ##       137       138       139       140       141       142       143       144 
    ##  876.5148  856.3717  770.8824  561.8698  766.2828  905.2227  844.4761  894.2788 
    ##       145       146       147       148       149       150       151       152 
    ##  552.0361  654.1474  613.9405  756.1319  825.6019  784.5226  734.5613  642.2519 
    ##       153       154       155       156       157       158       159       160 
    ##  899.1956  824.1744  594.5111  771.6754  743.1261  618.5560  834.1667  799.4317 
    ##       161       162       163       164       165       166       167       168 
    ##  933.7720  862.8746  960.4180  672.7045  584.2651  612.1165  580.0303  580.0144 
    ##       169       170       171       172       173       174       175       176 
    ##  600.6334  777.3853  788.6464  555.8585  620.8716  887.3001  550.5611  923.4625 
    ##       177       178       179       180       181       182       183       184 
    ##  775.4820  640.3486  739.0023  794.8321  838.7663  617.8581  678.0971  711.5633 
    ##       185       186       187       188       189       190       191       192 
    ##  736.1474  947.4122  819.5748  755.0216  771.3582  848.1241  603.7738  710.7702 
    ##       193       194       195       196       197       198       199       200 
    ##  589.2929  559.9823  758.5110  751.0565  691.7374  582.3618  760.8901  806.2518 
    ##       201       202       203       204       205       206       207       208 
    ##  732.1822  631.5618 1851.3148  794.9907  563.6620  669.5323  867.9500  790.5497 
    ##       209       210       211       212       213       214       215       216 
    ##  659.2229  619.6979  768.9791 1121.4044  787.8533  819.2576  857.7991  783.0951 
    ##       217       218       219       220       221       222       223       224 
    ##  805.3001  545.9297  629.6426  727.1068  764.6967  898.8784 1126.3212  646.0585 
    ##       225       226       227       228       229       230       231       232 
    ##  844.7933  734.2441  764.0623  831.1531 1047.1762  716.1629  716.4801  696.0198 
    ##       233       234       235       236       237       238       239       240 
    ##  891.2653  874.7701  531.7344  762.0004  769.6135 1237.1876  652.5614  827.5051 
    ##       241       242       243       244       245       246       247       248 
    ##  741.6986  844.9519  816.8785  763.4278  903.1608 1025.1298  575.4941  791.1841 
    ##       249       250       251       252       253       254       255       256 
    ##  821.4781  739.3195  650.6581  696.3370  917.5941  878.7353  645.8999  717.9075 
    ##       257       258       259       260       261       262       263       264 
    ## 1086.6694  993.2497  690.7857  801.1763 1404.9941 1175.4894  881.4316  784.3640 
    ##       265       266       267       268       269       270       271       272 
    ##  738.3679  693.6406  718.8592  728.2170  969.6172  873.6599  647.3273  751.6909 
    ##       273       274       275       276       277       278       279       280 
    ##  743.1261  750.7392  809.7411  570.0380  946.9364  884.9210  835.1183  854.4684 
    ##       281       282       283       284       285       286       287       288 
    ##  708.3911  682.0623  672.5459  557.1750  690.7857  905.2227  788.9636  800.3833 
    ##       289       290       291       292       293       294       295       296 
    ##  638.6039 1040.1974  637.1765 1702.5413  569.8636  558.6817  748.6774  755.0216 
    ##       297       298       299       300       301       302       303       304 
    ##  884.2865  702.6813 1324.7388  706.0120  869.6947  647.1687  836.3872  685.2345 
    ##       305       306       307       308       309       310       311       312 
    ##  979.6095 1142.3405  758.5110 2155.3649  717.2731  795.4665  972.7894  691.4201 
    ##       313       314       315       316       317       318       319       320 
    ##  766.6000  910.7739  717.9075 1372.7968  980.2439  708.8669  598.1908 1333.9380 
    ##       321       322       323       324       325       326       327       328 
    ## 1147.4159 1096.5030 1309.8297  672.8631  760.8901  658.2712  653.1958  711.7219 
    ##       329       330       331       332       333       334       335       336 
    ##  675.8766 1170.4140  728.0584 1138.0581  637.6523 1956.6301  934.5650  862.0815 
    ##       337       338       339       340       341       342       343       344 
    ##  911.0912  869.3775 1223.7060  725.3621  723.1416  635.8283  697.4472 1214.5068 
    ##       345       346       347       348       349       350       351       352 
    ##  801.3350 1106.3367  736.9404 1149.4778  967.2381  757.4007  972.4722 1052.5688 
    ##       353       354       355       356       357       358       359       360 
    ##  790.7083 1143.7680 1053.8377  877.1492  957.8803 1121.0871 1039.0872  752.8011 
    ##       361       362       363       364       365       366       367       368 
    ##  821.0023  994.6772  957.2459  695.5439 1127.4314 1021.1646  775.6406  897.4509 
    ##       369       370       371       372       373       374       375       376 
    ##  701.7296  673.6561  794.0390  805.6173 1601.8257  589.7053 1088.8899  934.7236 
    ##       377       378       379       380       381       382       383       384 
    ##  949.7913  822.4297  881.7488  783.0951 1011.9654 1007.8416 1043.2110  719.1764 
    ##       385       386       387       388       389       390       391       392 
    ##  782.4607  795.9423 1121.4044 1393.2572 1530.9282 1061.2922  785.4742  919.9732 
    ##       393       394       395       396       397       398       399       400 
    ## 1067.9537 1265.1025 1732.9938 1277.6325  786.4259 1019.1027  693.1648 1191.5088 
    ##       401       402       403       404       405       406       407       408 
    ##  633.7823  824.0158 1185.0059 1520.4602  750.4220 1468.1198  810.5342  820.5264 
    ##       409       410       411       412       413       414       415       416 
    ## 1000.5456  812.9133 1421.6478 1217.5203 1157.7254  763.9036 1005.6211  722.5072 
    ##       417       418       419       420       421       422       423       424 
    ##  743.6019  761.6831 1143.9266  803.5555 1223.7060  748.5187 1359.7910 1120.6113 
    ##       425       426       427       428       429       430       431       432 
    ##  850.9790 1187.5436  940.4335 1187.8608 1187.8608 1045.5901  944.7159  968.0312 
    ##       433       434       435       436       437       438       439       440 
    ##  774.0545  723.3002 1303.4855  857.0061 1217.2031  961.8455 1101.2612  712.9907 
    ##       441       442       443       444       445       446       447       448 
    ##  635.5270  712.9907  858.7508  801.6522 1297.6170  923.3039  788.6464 1085.5591 
    ##       449       450       451       452       453       454       455       456 
    ##  831.9462  661.2848 1019.7371  813.3891 1397.8568  667.9463 1446.3906 1039.8802 
    ##       457       458       459       460       461       462       463       464 
    ## 1258.4410 1131.0794  900.1473  932.0273  924.0970 1366.9284 1032.1085 1415.7794 
    ##       465       466       467       468       469       470       471       472 
    ##  860.1782 1148.6848  763.7450  841.6212 1096.3444 1041.7835 1069.6984 1097.9305 
    ##       473       474       475       476       477       478       479       480 
    ##  802.9210  813.0719 1069.6984  865.0951  948.0466  769.9307  846.3794  884.4451 
    ##       481       482       483       484       485       486       487       488 
    ## 1073.9808 1236.2360 1103.6403  943.9229  915.5322  879.5283  797.0526  927.7449 
    ##       489       490       491       492       493       494       495       496 
    ##  836.3872 1025.7642  840.6695 1284.6112  802.4452 1089.6829  838.1318  967.2381 
    ##       497       498       499       500       501       502       503       504 
    ##  808.6309 1026.5572  885.8726 1370.8935 1454.9554  985.7952 1071.9189  879.2111 
    ##       505       506       507       508       509       510       511       512 
    ## 1002.1317  864.9365 1657.9726  940.4335  721.0797 1084.7661 1152.4914 1019.1027 
    ##       513       514       515       516       517       518       519       520 
    ## 1650.0423 1365.6595 1015.1375 1150.5881 1086.5108  782.1435 1068.1123 1112.0465 
    ##       521       522       523       524       525       526       527       528 
    ## 1038.6114  843.8417 1047.8106  854.1512  974.5341 1522.5220 1141.2303 1854.1698 
    ##       529       530       531       532       533       534       535       536 
    ##  944.3987  847.8069  915.2149 1242.8975 1322.5183 1421.8065 1189.7641  961.8455 
    ##       537       538       539       540       541       542       543       544 
    ##  773.2615  978.6578 1138.2167  990.3948  980.4025 1532.9901  969.1414  812.4375 
    ##       545       546       547       548       549       550       551       552 
    ## 1130.1278 1150.4295 1384.0579 1680.1776 1034.1704 1044.7971 1093.4895 1087.3038 
    ##       553       554       555       556       557       558       559       560 
    ## 1587.0752 1226.7196  976.9132 1045.5901 1127.1142  717.7489 1440.9979  889.6792 
    ##       561       562       563       564       565       566       567       568 
    ## 1141.5475 1052.2516  902.0505 1131.7138  735.5130 1117.1220 1418.9515  896.6579 
    ##       569 
    ## 1152.3328 
    ## 
    ## $assign
    ## [1] 0 1
    ## 
    ## $qr
    ## $qr
    ##      (Intercept) compactness_worst
    ## 1   -23.85372088     -6.0651673886
    ## 2     0.04192218     -3.7497612967
    ## 3     0.04192218     -0.0456564742
    ## 4     0.04192218     -0.0301808225
    ## 5     0.04192218     -0.0441283770
    ## 6     0.04192218      0.0485708571
    ## 7     0.04192218     -0.0475499281
    ## 8     0.04192218     -0.0491846988
    ## 9     0.04192218     -0.0162599364
    ## 10    0.04192218     -0.0210869103
    ## 11    0.04192218     -0.0333276895
    ## 12    0.04192218     -0.0067659987
    ## 13    0.04192218     -0.0014056575
    ## 14    0.04192218     -0.0033257797
    ## 15    0.04192218     -0.0524169046
    ## 16    0.04192218     -0.0442403841
    ## 17    0.04192218      0.0335832364
    ## 18    0.04192218      0.0253960486
    ## 19    0.04192218     -0.0160465894
    ## 20    0.04192218     -0.0330876742
    ## 21    0.04192218     -0.0590919961
    ## 22    0.04192218     -0.0216202776
    ## 23    0.04192218     -0.0357545106
    ## 24    0.04192218     -0.0314342356
    ## 25    0.04192218      0.0500376171
    ## 26    0.04192218     -0.0104462330
    ## 27    0.04192218      0.0313164254
    ## 28    0.04192218     -0.0472805776
    ## 29    0.04192218     -0.0219936347
    ## 30    0.04192218     -0.0331143425
    ## 31    0.04192218     -0.0409948442
    ## 32    0.04192218     -0.0471579031
    ## 33    0.04192218     -0.0343677557
    ## 34    0.04192218     -0.0367945768
    ## 35    0.04192218      0.0301430174
    ## 36    0.04192218      0.0247293394
    ## 37    0.04192218     -0.0413815355
    ## 38    0.04192218     -0.0227403489
    ## 39    0.04192218     -0.0064726467
    ## 40    0.04192218     -0.0207668899
    ## 41    0.04192218     -0.0345544342
    ## 42    0.04192218     -0.0049792183
    ## 43    0.04192218     -0.0433976638
    ## 44    0.04192218     -0.0409121722
    ## 45    0.04192218     -0.0411068513
    ## 46    0.04192218     -0.0489233489
    ## 47    0.04192218     -0.0193001299
    ## 48    0.04192218     -0.0400587846
    ## 49    0.04192218     -0.0117263144
    ## 50    0.04192218     -0.0385280205
    ## 51    0.04192218     -0.0279673482
    ## 52    0.04192218     -0.0474512551
    ## 53    0.04192218     -0.0065526518
    ## 54    0.04192218     -0.0329009956
    ## 55    0.04192218      0.0075549129
    ## 56    0.04192218     -0.0303141643
    ## 57    0.04192218     -0.0289807461
    ## 58    0.04192218     -0.0427896251
    ## 59    0.04192218     -0.0252471751
    ## 60    0.04192218      0.0123018817
    ## 61    0.04192218     -0.0034324532
    ## 62    0.04192218     -0.0463525185
    ## 63    0.04192218     -0.0223669918
    ## 64    0.04192218     -0.0356211688
    ## 65    0.04192218     -0.0443950606
    ## 66    0.04192218     -0.0267406035
    ## 67    0.04192218     -0.0390080510
    ## 68    0.04192218     -0.0291140879
    ## 69    0.04192218      0.0005678015
    ## 70    0.04192218     -0.0482593066
    ## 71    0.04192218      0.0016345360
    ## 72    0.04192218     -0.0122063450
    ## 73    0.04192218     -0.0467738787
    ## 74    0.04192218     -0.0328476589
    ## 75    0.04192218     -0.0497847370
    ## 76    0.04192218      0.0007011433
    ## 77    0.04192218      0.0375834910
    ## 78    0.04192218     -0.0177800331
    ## 79    0.04192218     -0.0457738150
    ## 80    0.04192218     -0.0405841514
    ## 81    0.04192218     -0.0188467677
    ## 82    0.04192218     -0.0391947296
    ## 83    0.04192218     -0.0172199975
    ## 84    0.04192218     -0.0383946787
    ## 85    0.04192218     -0.0517181934
    ## 86    0.04192218     -0.0074327078
    ## 87    0.04192218      0.0135286265
    ## 88    0.04192218     -0.0424456032
    ## 89    0.04192218     -0.0337543833
    ## 90    0.04192218     -0.0135130948
    ## 91    0.04192218      0.0085416424
    ## 92    0.04192218     -0.0281806952
    ## 93    0.04192218     -0.0328743273
    ## 94    0.04192218     -0.0290874195
    ## 95    0.04192218     -0.0261805678
    ## 96    0.04192218     -0.0098861973
    ## 97    0.04192218     -0.0467738787
    ## 98    0.04192218     -0.0368212452
    ## 99    0.04192218     -0.0404801447
    ## 100   0.04192218     -0.0224203285
    ## 101   0.04192218     -0.0419735731
    ## 102   0.04192218     -0.0234337263
    ## 103   0.04192218     -0.0127663806
    ## 104   0.04192218     -0.0230070325
    ## 105   0.04192218     -0.0442510514
    ## 106   0.04192218     -0.0480086240
    ## 107   0.04192218     -0.0268472770
    ## 108   0.04192218     -0.0440537055
    ## 109   0.04192218     -0.0225270019
    ## 110   0.04192218      0.0629184370
    ## 111   0.04192218      0.0081149485
    ## 112   0.04192218      0.0007544800
    ## 113   0.04192218     -0.0084994424
    ## 114   0.04192218     -0.0365012248
    ## 115   0.04192218     -0.0015923360
    ## 116   0.04192218      0.0424904700
    ## 117   0.04192218     -0.0300741490
    ## 118   0.04192218     -0.0243937874
    ## 119   0.04192218     -0.0373812808
    ## 120   0.04192218      0.0203290594
    ## 121   0.04192218      0.0193423299
    ## 122   0.04192218     -0.0397014285
    ## 123   0.04192218     -0.0211402470
    ## 124   0.04192218     -0.0186600891
    ## 125   0.04192218     -0.0241804405
    ## 126   0.04192218     -0.0402507968
    ## 127   0.04192218     -0.0181533902
    ## 128   0.04192218     -0.0414135375
    ## 129   0.04192218     -0.0160999262
    ## 130   0.04192218     -0.0162599364
    ## 131   0.04192218     -0.0185800840
    ## 132   0.04192218     -0.0228203540
    ## 133   0.04192218     -0.0431149791
    ## 134   0.04192218      0.0242759773
    ## 135   0.04192218      0.1837527953
    ## 136   0.04192218     -0.0383946787
    ## 137   0.04192218      0.0007544800
    ## 138   0.04192218     -0.0026324022
    ## 139   0.04192218     -0.0170066506
    ## 140   0.04192218     -0.0521502209
    ## 141   0.04192218     -0.0177800331
    ## 142   0.04192218      0.0055814539
    ## 143   0.04192218     -0.0046325296
    ## 144   0.04192218      0.0037413368
    ## 145   0.04192218     -0.0538036595
    ## 146   0.04192218     -0.0366345666
    ## 147   0.04192218     -0.0433949970
    ## 148   0.04192218     -0.0194868084
    ## 149   0.04192218     -0.0078060649
    ## 150   0.04192218     -0.0147131712
    ## 151   0.04192218     -0.0231137060
    ## 152   0.04192218     -0.0386346939
    ## 153   0.04192218      0.0045680561
    ## 154   0.04192218     -0.0080460802
    ## 155   0.04192218     -0.0466618716
    ## 156   0.04192218     -0.0168733087
    ## 157   0.04192218     -0.0216736143
    ## 158   0.04192218     -0.0426189476
    ## 159   0.04192218     -0.0063659732
    ## 160   0.04192218     -0.0122063450
    ## 161   0.04192218      0.0103817595
    ## 162   0.04192218     -0.0015389993
    ## 163   0.04192218      0.0148620447
    ## 164   0.04192218     -0.0335143680
    ## 165   0.04192218     -0.0483846479
    ## 166   0.04192218     -0.0437016831
    ## 167   0.04192218     -0.0490966932
    ## 168   0.04192218     -0.0490993601
    ## 169   0.04192218     -0.0456324727
    ## 170   0.04192218     -0.0159132476
    ## 171   0.04192218     -0.0140197938
    ## 172   0.04192218     -0.0531609519
    ## 173   0.04192218     -0.0422295894
    ## 174   0.04192218      0.0025679288
    ## 175   0.04192218     -0.0540516753
    ## 176   0.04192218      0.0086483158
    ## 177   0.04192218     -0.0162332680
    ## 178   0.04192218     -0.0389547143
    ## 179   0.04192218     -0.0223669918
    ## 180   0.04192218     -0.0129797276
    ## 181   0.04192218     -0.0055925907
    ## 182   0.04192218     -0.0427362884
    ## 183   0.04192218     -0.0326076436
    ## 184   0.04192218     -0.0269806188
    ## 185   0.04192218     -0.0228470223
    ## 186   0.04192218      0.0126752388
    ## 187   0.04192218     -0.0088194627
    ## 188   0.04192218     -0.0196734870
    ## 189   0.04192218     -0.0169266455
    ## 190   0.04192218     -0.0040191572
    ## 191   0.04192218     -0.0451044391
    ## 192   0.04192218     -0.0271139606
    ## 193   0.04192218     -0.0475392608
    ## 194   0.04192218     -0.0524675745
    ## 195   0.04192218     -0.0190867830
    ## 196   0.04192218     -0.0203401961
    ## 197   0.04192218     -0.0303141643
    ## 198   0.04192218     -0.0487046683
    ## 199   0.04192218     -0.0186867575
    ## 200   0.04192218     -0.0110596053
    ## 201   0.04192218     -0.0235137314
    ## 202   0.04192218     -0.0404321417
    ## 203   0.04192218      0.1646582465
    ## 204   0.04192218     -0.0129530592
    ## 205   0.04192218     -0.0518488684
    ## 206   0.04192218     -0.0340477353
    ## 207   0.04192218     -0.0006856117
    ## 208   0.04192218     -0.0136997734
    ## 209   0.04192218     -0.0357811790
    ## 210   0.04192218     -0.0424269353
    ## 211   0.04192218     -0.0173266709
    ## 212   0.04192218      0.0419304344
    ## 213   0.04192218     -0.0141531356
    ## 214   0.04192218     -0.0088727995
    ## 215   0.04192218     -0.0023923870
    ## 216   0.04192218     -0.0149531865
    ## 217   0.04192218     -0.0112196155
    ## 218   0.04192218     -0.0548303915
    ## 219   0.04192218     -0.0407548289
    ## 220   0.04192218     -0.0243671191
    ## 221   0.04192218     -0.0180467168
    ## 222   0.04192218      0.0045147194
    ## 223   0.04192218      0.0427571537
    ## 224   0.04192218     -0.0379946532
    ## 225   0.04192218     -0.0045791928
    ## 226   0.04192218     -0.0231670427
    ## 227   0.04192218     -0.0181533902
    ## 228   0.04192218     -0.0068726722
    ## 229   0.04192218      0.0294496399
    ## 230   0.04192218     -0.0262072362
    ## 231   0.04192218     -0.0261538995
    ## 232   0.04192218     -0.0295941185
    ## 233   0.04192218      0.0032346379
    ## 234   0.04192218      0.0004611280
    ## 235   0.04192218     -0.0572172101
    ## 236   0.04192218     -0.0185000790
    ## 237   0.04192218     -0.0172199975
    ## 238   0.04192218      0.0613983403
    ## 239   0.04192218     -0.0369012503
    ## 240   0.04192218     -0.0074860445
    ## 241   0.04192218     -0.0219136296
    ## 242   0.04192218     -0.0045525245
    ## 243   0.04192218     -0.0092728249
    ## 244   0.04192218     -0.0182600637
    ## 245   0.04192218      0.0052347652
    ## 246   0.04192218      0.0257427373
    ## 247   0.04192218     -0.0498594084
    ## 248   0.04192218     -0.0135930999
    ## 249   0.04192218     -0.0084994424
    ## 250   0.04192218     -0.0223136550
    ## 251   0.04192218     -0.0372212706
    ## 252   0.04192218     -0.0295407817
    ## 253   0.04192218      0.0076615863
    ## 254   0.04192218      0.0011278371
    ## 255   0.04192218     -0.0380213216
    ## 256   0.04192218     -0.0259138842
    ## 257   0.04192218      0.0360900626
    ## 258   0.04192218      0.0203823961
    ## 259   0.04192218     -0.0304741745
    ## 260   0.04192218     -0.0119129930
    ## 261   0.04192218      0.0896134696
    ## 262   0.04192218      0.0510243466
    ## 263   0.04192218      0.0015811993
    ## 264   0.04192218     -0.0147398396
    ## 265   0.04192218     -0.0224736652
    ## 266   0.04192218     -0.0299941439
    ## 267   0.04192218     -0.0257538740
    ## 268   0.04192218     -0.0241804405
    ## 269   0.04192218      0.0164088098
    ## 270   0.04192218      0.0002744495
    ## 271   0.04192218     -0.0377813063
    ## 272   0.04192218     -0.0202335226
    ## 273   0.04192218     -0.0216736143
    ## 274   0.04192218     -0.0203935328
    ## 275   0.04192218     -0.0104729013
    ## 276   0.04192218     -0.0507768002
    ## 277   0.04192218      0.0125952337
    ## 278   0.04192218      0.0021679033
    ## 279   0.04192218     -0.0062059630
    ## 280   0.04192218     -0.0029524226
    ## 281   0.04192218     -0.0275139861
    ## 282   0.04192218     -0.0319409345
    ## 283   0.04192218     -0.0335410364
    ## 284   0.04192218     -0.0529396045
    ## 285   0.04192218     -0.0304741745
    ## 286   0.04192218      0.0055814539
    ## 287   0.04192218     -0.0139664570
    ## 288   0.04192218     -0.0120463348
    ## 289   0.04192218     -0.0392480663
    ## 290   0.04192218      0.0282762319
    ## 291   0.04192218     -0.0394880816
    ## 292   0.04192218      0.1396433209
    ## 293   0.04192218     -0.0508061354
    ## 294   0.04192218     -0.0526862550
    ## 295   0.04192218     -0.0207402215
    ## 296   0.04192218     -0.0196734870
    ## 297   0.04192218      0.0020612299
    ## 298   0.04192218     -0.0284740472
    ## 299   0.04192218      0.0761192773
    ## 300   0.04192218     -0.0279140115
    ## 301   0.04192218     -0.0003922596
    ## 302   0.04192218     -0.0378079746
    ## 303   0.04192218     -0.0059926161
    ## 304   0.04192218     -0.0314075672
    ## 305   0.04192218      0.0180889168
    ## 306   0.04192218      0.0454506584
    ## 307   0.04192218     -0.0190867830
    ## 308   0.04192218      0.2157815007
    ## 309   0.04192218     -0.0260205577
    ## 310   0.04192218     -0.0128730541
    ## 311   0.04192218      0.0169421771
    ## 312   0.04192218     -0.0303675010
    ## 313   0.04192218     -0.0177266964
    ## 314   0.04192218      0.0065148467
    ## 315   0.04192218     -0.0259138842
    ## 316   0.04192218      0.0841997917
    ## 317   0.04192218      0.0181955902
    ## 318   0.04192218     -0.0274339810
    ## 319   0.04192218     -0.0460431655
    ## 320   0.04192218      0.0776660424
    ## 321   0.04192218      0.0463040461
    ## 322   0.04192218      0.0377435012
    ## 323   0.04192218      0.0736124511
    ## 324   0.04192218     -0.0334876996
    ## 325   0.04192218     -0.0186867575
    ## 326   0.04192218     -0.0359411892
    ## 327   0.04192218     -0.0367945768
    ## 328   0.04192218     -0.0269539504
    ## 329   0.04192218     -0.0329810007
    ## 330   0.04192218      0.0501709589
    ## 331   0.04192218     -0.0242071089
    ## 332   0.04192218      0.0447306126
    ## 333   0.04192218     -0.0394080765
    ## 334   0.04192218      0.1823660404
    ## 335   0.04192218      0.0105151013
    ## 336   0.04192218     -0.0016723411
    ## 337   0.04192218      0.0065681834
    ## 338   0.04192218     -0.0004455964
    ## 339   0.04192218      0.0591315293
    ## 340   0.04192218     -0.0246604711
    ## 341   0.04192218     -0.0250338282
    ## 342   0.04192218     -0.0397147627
    ## 343   0.04192218     -0.0293541032
    ## 344   0.04192218      0.0575847642
    ## 345   0.04192218     -0.0118863246
    ## 346   0.04192218      0.0393969398
    ## 347   0.04192218     -0.0227136805
    ## 348   0.04192218      0.0466507348
    ## 349   0.04192218      0.0160087843
    ## 350   0.04192218     -0.0192734615
    ## 351   0.04192218      0.0168888404
    ## 352   0.04192218      0.0303563643
    ## 353   0.04192218     -0.0136731050
    ## 354   0.04192218      0.0456906737
    ## 355   0.04192218      0.0305697112
    ## 356   0.04192218      0.0008611535
    ## 357   0.04192218      0.0144353509
    ## 358   0.04192218      0.0418770976
    ## 359   0.04192218      0.0280895533
    ## 360   0.04192218     -0.0200468441
    ## 361   0.04192218     -0.0085794475
    ## 362   0.04192218      0.0206224114
    ## 363   0.04192218      0.0143286774
    ## 364   0.04192218     -0.0296741236
    ## 365   0.04192218      0.0429438322
    ## 366   0.04192218      0.0250760282
    ## 367   0.04192218     -0.0162065996
    ## 368   0.04192218      0.0042747041
    ## 369   0.04192218     -0.0286340574
    ## 370   0.04192218     -0.0333543578
    ## 371   0.04192218     -0.0131130694
    ## 372   0.04192218     -0.0111662788
    ## 373   0.04192218      0.1227089096
    ## 374   0.04192218     -0.0474699230
    ## 375   0.04192218      0.0364634197
    ## 376   0.04192218      0.0105417697
    ## 377   0.04192218      0.0130752643
    ## 378   0.04192218     -0.0083394322
    ## 379   0.04192218      0.0016345360
    ## 380   0.04192218     -0.0149531865
    ## 381   0.04192218      0.0235292631
    ## 382   0.04192218      0.0228358856
    ## 383   0.04192218      0.0287829308
    ## 384   0.04192218     -0.0257005373
    ## 385   0.04192218     -0.0150598600
    ## 386   0.04192218     -0.0127930490
    ## 387   0.04192218      0.0419304344
    ## 388   0.04192218      0.0876400106
    ## 389   0.04192218      0.1107881508
    ## 390   0.04192218      0.0318231243
    ## 391   0.04192218     -0.0145531610
    ## 392   0.04192218      0.0080616118
    ## 393   0.04192218      0.0329431956
    ## 394   0.04192218      0.0660919724
    ## 395   0.04192218      0.1447636468
    ## 396   0.04192218      0.0681987731
    ## 397   0.04192218     -0.0143931509
    ## 398   0.04192218      0.0247293394
    ## 399   0.04192218     -0.0300741490
    ## 400   0.04192218      0.0537178514
    ## 401   0.04192218     -0.0400587846
    ## 402   0.04192218     -0.0080727485
    ## 403   0.04192218      0.0526244484
    ## 404   0.04192218      0.1090280387
    ## 405   0.04192218     -0.0204468695
    ## 406   0.04192218      0.1002274786
    ## 407   0.04192218     -0.0103395595
    ## 408   0.04192218     -0.0086594526
    ## 409   0.04192218      0.0216091408
    ## 410   0.04192218     -0.0099395340
    ## 411   0.04192218      0.0924136478
    ## 412   0.04192218      0.0580914631
    ## 413   0.04192218      0.0480374898
    ## 414   0.04192218     -0.0181800586
    ## 415   0.04192218      0.0224625285
    ## 416   0.04192218     -0.0251405016
    ## 417   0.04192218     -0.0215936092
    ## 418   0.04192218     -0.0185534157
    ## 419   0.04192218      0.0457173421
    ## 420   0.04192218     -0.0115129675
    ## 421   0.04192218      0.0591315293
    ## 422   0.04192218     -0.0207668899
    ## 423   0.04192218      0.0820129858
    ## 424   0.04192218      0.0417970926
    ## 425   0.04192218     -0.0035391266
    ## 426   0.04192218      0.0530511423
    ## 427   0.04192218      0.0115018308
    ## 428   0.04192218      0.0531044790
    ## 429   0.04192218      0.0531044790
    ## 430   0.04192218      0.0291829563
    ## 431   0.04192218      0.0122218766
    ## 432   0.04192218      0.0161421262
    ## 433   0.04192218     -0.0164732833
    ## 434   0.04192218     -0.0250071598
    ## 435   0.04192218      0.0725457165
    ## 436   0.04192218     -0.0025257288
    ## 437   0.04192218      0.0580381264
    ## 438   0.04192218      0.0151020600
    ## 439   0.04192218      0.0385435521
    ## 440   0.04192218     -0.0267406035
    ## 441   0.04192218     -0.0397654326
    ## 442   0.04192218     -0.0267406035
    ## 443   0.04192218     -0.0022323768
    ## 444   0.04192218     -0.0118329879
    ## 445   0.04192218      0.0715589870
    ## 446   0.04192218      0.0086216475
    ## 447   0.04192218     -0.0140197938
    ## 448   0.04192218      0.0359033841
    ## 449   0.04192218     -0.0067393303
    ## 450   0.04192218     -0.0354344902
    ## 451   0.04192218      0.0248360129
    ## 452   0.04192218     -0.0098595289
    ## 453   0.04192218      0.0884133932
    ## 454   0.04192218     -0.0343144189
    ## 455   0.04192218      0.0965739127
    ## 456   0.04192218      0.0282228952
    ## 457   0.04192218      0.0649719011
    ## 458   0.04192218      0.0435572046
    ## 459   0.04192218      0.0047280663
    ## 460   0.04192218      0.0100884075
    ## 461   0.04192218      0.0087549893
    ## 462   0.04192218      0.0832130622
    ## 463   0.04192218      0.0269161453
    ## 464   0.04192218      0.0914269184
    ## 465   0.04192218     -0.0019923615
    ## 466   0.04192218      0.0465173930
    ## 467   0.04192218     -0.0182067269
    ## 468   0.04192218     -0.0051125601
    ## 469   0.04192218      0.0377168328
    ## 470   0.04192218      0.0285429155
    ## 471   0.04192218      0.0332365476
    ## 472   0.04192218      0.0379835165
    ## 473   0.04192218     -0.0116196410
    ## 474   0.04192218     -0.0099128657
    ## 475   0.04192218      0.0332365476
    ## 476   0.04192218     -0.0011656422
    ## 477   0.04192218      0.0127819123
    ## 478   0.04192218     -0.0171666607
    ## 479   0.04192218     -0.0043125092
    ## 480   0.04192218      0.0020878982
    ## 481   0.04192218      0.0339565935
    ## 482   0.04192218      0.0612383301
    ## 483   0.04192218      0.0389435776
    ## 484   0.04192218      0.0120885348
    ## 485   0.04192218      0.0073148976
    ## 486   0.04192218      0.0012611789
    ## 487   0.04192218     -0.0126063705
    ## 488   0.04192218      0.0093683617
    ## 489   0.04192218     -0.0059926161
    ## 490   0.04192218      0.0258494107
    ## 491   0.04192218     -0.0052725703
    ## 492   0.04192218      0.0693721812
    ## 493   0.04192218     -0.0116996461
    ## 494   0.04192218      0.0365967615
    ## 495   0.04192218     -0.0056992641
    ## 496   0.04192218      0.0160087843
    ## 497   0.04192218     -0.0106595799
    ## 498   0.04192218      0.0259827526
    ## 499   0.04192218      0.0023279135
    ## 500   0.04192218      0.0838797713
    ## 501   0.04192218      0.0980140043
    ## 502   0.04192218      0.0191289830
    ## 503   0.04192218      0.0336099047
    ## 504   0.04192218      0.0012078422
    ## 505   0.04192218      0.0218758245
    ## 506   0.04192218     -0.0011923106
    ## 507   0.04192218      0.1321495105
    ## 508   0.04192218      0.0115018308
    ## 509   0.04192218     -0.0253805169
    ## 510   0.04192218      0.0357700422
    ## 511   0.04192218      0.0471574338
    ## 512   0.04192218      0.0247293394
    ## 513   0.04192218      0.1308160923
    ## 514   0.04192218      0.0829997153
    ## 515   0.04192218      0.0240626303
    ## 516   0.04192218      0.0468374134
    ## 517   0.04192218      0.0360633942
    ## 518   0.04192218     -0.0151131967
    ## 519   0.04192218      0.0329698640
    ## 520   0.04192218      0.0403570009
    ## 521   0.04192218      0.0280095482
    ## 522   0.04192218     -0.0047392030
    ## 523   0.04192218      0.0295563134
    ## 524   0.04192218     -0.0030057593
    ## 525   0.04192218      0.0172355291
    ## 526   0.04192218      0.1093747275
    ## 527   0.04192218      0.0452639799
    ## 528   0.04192218      0.1651382771
    ## 529   0.04192218      0.0121685399
    ## 530   0.04192218     -0.0040724939
    ## 531   0.04192218      0.0072615609
    ## 532   0.04192218      0.0623584014
    ## 533   0.04192218      0.0757459202
    ## 534   0.04192218      0.0924403162
    ## 535   0.04192218      0.0534244994
    ## 536   0.04192218      0.0151020600
    ## 537   0.04192218     -0.0166066251
    ## 538   0.04192218      0.0179289066
    ## 539   0.04192218      0.0447572810
    ## 540   0.04192218      0.0199023655
    ## 541   0.04192218      0.0182222586
    ## 542   0.04192218      0.1111348395
    ## 543   0.04192218      0.0163288047
    ## 544   0.04192218     -0.0100195391
    ## 545   0.04192218      0.0433971944
    ## 546   0.04192218      0.0468107450
    ## 547   0.04192218      0.0860932455
    ## 548   0.04192218      0.1358830815
    ## 549   0.04192218      0.0272628341
    ## 550   0.04192218      0.0290496145
    ## 551   0.04192218      0.0372368023
    ## 552   0.04192218      0.0361967361
    ## 553   0.04192218      0.1202287517
    ## 554   0.04192218      0.0596382282
    ## 555   0.04192218      0.0176355546
    ## 556   0.04192218      0.0291829563
    ## 557   0.04192218      0.0428904955
    ## 558   0.04192218     -0.0259405526
    ## 559   0.04192218      0.0956671883
    ## 560   0.04192218      0.0029679542
    ## 561   0.04192218      0.0453173166
    ## 562   0.04192218      0.0303030276
    ## 563   0.04192218      0.0050480867
    ## 564   0.04192218      0.0436638780
    ## 565   0.04192218     -0.0229536958
    ## 566   0.04192218      0.0412103885
    ## 567   0.04192218      0.0919602856
    ## 568   0.04192218      0.0041413623
    ## 569   0.04192218      0.0471307654
    ## attr(,"assign")
    ## [1] 0 1
    ## 
    ## $qraux
    ## [1] 1.041922 1.015342
    ## 
    ## $pivot
    ## [1] 1 2
    ## 
    ## $tol
    ## [1] 1e-07
    ## 
    ## $rank
    ## [1] 2
    ## 
    ## attr(,"class")
    ## [1] "qr"
    ## 
    ## $df.residual
    ## [1] 567
    ## 
    ## $xlevels
    ## named list()
    ## 
    ## $call
    ## lm(formula = area_worst ~ compactness_worst, data = cancer)
    ## 
    ## $terms
    ## area_worst ~ compactness_worst
    ## attr(,"variables")
    ## list(area_worst, compactness_worst)
    ## attr(,"factors")
    ##                   compactness_worst
    ## area_worst                        0
    ## compactness_worst                 1
    ## attr(,"term.labels")
    ## [1] "compactness_worst"
    ## attr(,"order")
    ## [1] 1
    ## attr(,"intercept")
    ## [1] 1
    ## attr(,"response")
    ## [1] 1
    ## attr(,".Environment")
    ## <environment: R_GlobalEnv>
    ## attr(,"predvars")
    ## list(area_worst, compactness_worst)
    ## attr(,"dataClasses")
    ##        area_worst compactness_worst 
    ##         "numeric"         "numeric" 
    ## 
    ## $model
    ##     area_worst compactness_worst
    ## 1        185.2           0.12020
    ## 2        223.6           0.30640
    ## 3        240.1           0.07767
    ## 4        242.2           0.13570
    ## 5        248.0           0.08340
    ## 6        249.8           0.43100
    ## 7        259.2           0.07057
    ## 8        268.6           0.06444
    ## 9        270.0           0.18790
    ## 10       273.9           0.16980
    ## 11       274.9           0.12390
    ## 12       275.6           0.22350
    ## 13       284.4           0.24360
    ## 14       284.4           0.23640
    ## 15       285.5           0.05232
    ## 16       295.8           0.08298
    ## 17       297.1           0.37480
    ## 18       300.2           0.34410
    ## 19       301.0           0.18870
    ## 20       302.0           0.12480
    ## 21       303.8           0.02729
    ## 22       310.1           0.16780
    ## 23       314.9           0.11480
    ## 24       317.0           0.13100
    ## 25       324.7           0.43650
    ## 26       326.6           0.20970
    ## 27       328.1           0.36630
    ## 28       330.6           0.07158
    ## 29       330.7           0.16640
    ## 30       331.6           0.12470
    ## 31       335.9           0.09515
    ## 32       342.9           0.07204
    ## 33       347.3           0.12000
    ## 34       349.9           0.11090
    ## 35       351.9           0.36190
    ## 36       353.6           0.34160
    ## 37       355.2           0.09370
    ## 38       357.1           0.16360
    ## 39       357.4           0.22460
    ## 40       357.6           0.17100
    ## 41       359.4           0.11930
    ## 42       361.2           0.23020
    ## 43       362.7           0.08614
    ## 44       364.2           0.09546
    ## 45       366.1           0.09473
    ## 46       366.3           0.06542
    ## 47       367.0           0.17650
    ## 48       368.6           0.09866
    ## 49       374.4           0.20490
    ## 50       375.4           0.10440
    ## 51       375.6           0.14400
    ## 52       376.3           0.07094
    ## 53       376.5           0.22430
    ## 54       380.2           0.12550
    ## 55       380.5           0.27720
    ## 56       380.9           0.13520
    ## 57       384.0           0.14020
    ## 58       384.9           0.08842
    ## 59       385.2           0.15420
    ## 60       390.2           0.29500
    ## 61       390.4           0.23600
    ## 62       392.2           0.07506
    ## 63       394.5           0.16500
    ## 64       395.4           0.11530
    ## 65       396.5           0.08240
    ## 66       402.8           0.14860
    ## 67       402.8           0.10260
    ## 68       403.7           0.13970
    ## 69       407.5           0.25100
    ## 70       408.3           0.06791
    ## 71       410.4           0.25500
    ## 72       411.1           0.20310
    ## 73       412.3           0.07348
    ## 74       414.0           0.12570
    ## 75       421.1           0.06219
    ## 76       424.8           0.25150
    ## 77       433.1           0.38980
    ## 78       434.0           0.18220
    ## 79       435.9           0.07723
    ## 80       436.1           0.09669
    ## 81       436.6           0.17820
    ## 82       437.0           0.10190
    ## 83       437.6           0.18430
    ## 84       439.6           0.10490
    ## 85       439.6           0.05494
    ## 86       440.0           0.22100
    ## 87       440.4           0.29960
    ## 88       440.8           0.08971
    ## 89       441.2           0.12230
    ## 90       447.1           0.19820
    ## 91       450.0           0.28090
    ## 92       452.3           0.14320
    ## 93       453.5           0.12560
    ## 94       455.7           0.13980
    ## 95       457.5           0.15070
    ## 96       457.8           0.21180
    ## 97       458.0           0.07348
    ## 98       458.0           0.11080
    ## 99       459.3           0.09708
    ## 100      462.0           0.16480
    ## 101      466.7           0.09148
    ## 102      467.2           0.16100
    ## 103      467.6           0.20100
    ## 104      467.8           0.16260
    ## 105      470.0           0.08294
    ## 106      470.9           0.06885
    ## 107      471.4           0.14820
    ## 108      472.4           0.08368
    ## 109      472.4           0.16440
    ## 110      472.9           0.48480
    ## 111      473.8           0.27930
    ## 112      474.2           0.25170
    ## 113      475.7           0.21700
    ## 114      475.8           0.11200
    ## 115      476.1           0.24290
    ## 116      476.4           0.40820
    ## 117      476.5           0.13610
    ## 118      478.6           0.15740
    ## 119      483.1           0.10870
    ## 120      487.7           0.32510
    ## 121      488.4           0.32140
    ## 122      489.5           0.10000
    ## 123      489.5           0.16960
    ## 124      489.8           0.17890
    ## 125      491.8           0.15820
    ## 126      492.7           0.09794
    ## 127      495.1           0.18080
    ## 128      495.2           0.09358
    ## 129      496.2           0.18850
    ## 130      496.7           0.18790
    ## 131      503.0           0.17920
    ## 132      505.6           0.16330
    ## 133      506.2           0.08720
    ## 134      507.2           0.33990
    ## 135      508.1           0.93790
    ## 136      508.9           0.10490
    ## 137      509.6           0.25170
    ## 138      510.5           0.23900
    ## 139      512.5           0.18510
    ## 140      513.1           0.05332
    ## 141      513.9           0.18220
    ## 142      514.0           0.26980
    ## 143      515.3           0.23150
    ## 144      515.8           0.26290
    ## 145      515.9           0.04712
    ## 146      516.4           0.11150
    ## 147      516.5           0.08615
    ## 148      517.8           0.17580
    ## 149      518.1           0.21960
    ## 150      520.5           0.19370
    ## 151      521.3           0.16220
    ## 152      521.5           0.10400
    ## 153      521.7           0.26600
    ## 154      522.9           0.21870
    ## 155      523.4           0.07390
    ## 156      523.7           0.18560
    ## 157      525.1           0.16760
    ## 158      527.2           0.08906
    ## 159      527.4           0.22500
    ## 160      527.8           0.20310
    ## 161      528.1           0.28780
    ## 162      529.9           0.24310
    ## 163      531.2           0.30460
    ## 164      532.8           0.12320
    ## 165      533.1           0.06744
    ## 166      533.7           0.08500
    ## 167      534.0           0.06477
    ## 168      542.5           0.06476
    ## 169      543.4           0.07776
    ## 170      543.9           0.18920
    ## 171      544.1           0.19630
    ## 172      544.2           0.04953
    ## 173      544.3           0.09052
    ## 174      545.2           0.25850
    ## 175      545.9           0.04619
    ## 176      546.1           0.28130
    ## 177      546.3           0.18800
    ## 178      546.7           0.10280
    ## 179      546.7           0.16500
    ## 180      547.4           0.20020
    ## 181      547.4           0.22790
    ## 182      547.8           0.08862
    ## 183      549.1           0.12660
    ## 184      549.8           0.14770
    ## 185      549.9           0.16320
    ## 186      550.6           0.29640
    ## 187      551.3           0.21580
    ## 188      552.0           0.17510
    ## 189      552.3           0.18540
    ## 190      553.0           0.23380
    ## 191      553.6           0.07974
    ## 192      553.7           0.14720
    ## 193      554.9           0.07061
    ## 194      558.9           0.05213
    ## 195      559.5           0.17730
    ## 196      562.0           0.17260
    ## 197      562.6           0.13520
    ## 198      564.1           0.06624
    ## 199      564.2           0.17880
    ## 200      564.9           0.20740
    ## 201      566.9           0.16070
    ## 202      567.6           0.09726
    ## 203      567.7           0.86630
    ## 204      567.9           0.20030
    ## 205      570.7           0.05445
    ## 206      574.4           0.12120
    ## 207      574.7           0.24630
    ## 208      576.0           0.19750
    ## 209      577.0           0.11470
    ## 210      579.5           0.08978
    ## 211      579.7           0.18390
    ## 212      580.6           0.40610
    ## 213      580.9           0.19580
    ## 214      582.6           0.21560
    ## 215      583.0           0.23990
    ## 216      583.1           0.19280
    ## 217      585.4           0.20680
    ## 218      585.7           0.04327
    ## 219      586.8           0.09605
    ## 220      589.5           0.15750
    ## 221      591.0           0.18120
    ## 222      591.2           0.26580
    ## 223      591.7           0.40920
    ## 224      594.7           0.10640
    ## 225      595.6           0.23170
    ## 226      595.7           0.16200
    ## 227      597.5           0.18080
    ## 228      599.5           0.22310
    ## 229      600.5           0.35930
    ## 230      600.6           0.15060
    ## 231      602.0           0.15080
    ## 232      605.5           0.13790
    ## 233      605.8           0.26100
    ## 234      607.3           0.25060
    ## 235      608.8           0.03432
    ## 236      610.2           0.17950
    ## 237      611.1           0.18430
    ## 238      614.9           0.47910
    ## 239      616.7           0.11050
    ## 240      618.8           0.22080
    ## 241      621.2           0.16670
    ## 242      621.9           0.23180
    ## 243      622.1           0.21410
    ## 244      622.9           0.18040
    ## 245      623.7           0.26850
    ## 246      624.0           0.34540
    ## 247      624.1           0.06191
    ## 248      624.1           0.19790
    ## 249      624.6           0.21700
    ## 250      626.9           0.16520
    ## 251      628.5           0.10930
    ## 252      629.6           0.13810
    ## 253      630.5           0.27760
    ## 254      632.1           0.25310
    ## 255      632.9           0.10630
    ## 256      633.5           0.15170
    ## 257      633.7           0.38420
    ## 258      634.3           0.32530
    ## 259      636.9           0.13460
    ## 260      638.4           0.20420
    ## 261      639.1           0.58490
    ## 262      639.3           0.44020
    ## 263      643.8           0.25480
    ## 264      645.8           0.19360
    ## 265      648.3           0.16460
    ## 266      653.3           0.13640
    ## 267      653.6           0.15230
    ## 268      656.7           0.15820
    ## 269      657.0           0.31040
    ## 270      660.2           0.24990
    ## 271      661.1           0.10720
    ## 272      661.5           0.17300
    ## 273      663.5           0.16760
    ## 274      670.0           0.17240
    ## 275      670.6           0.20960
    ## 276      672.4           0.05847
    ## 277      674.7           0.29610
    ## 278      675.2           0.25700
    ## 279      677.3           0.22560
    ## 280      677.9           0.23780
    ## 281      680.6           0.14570
    ## 282      683.4           0.12910
    ## 283      684.5           0.12310
    ## 284      684.6           0.05036
    ## 285      686.5           0.13460
    ## 286      686.6           0.26980
    ## 287      687.6           0.19650
    ## 288      688.6           0.20370
    ## 289      688.9           0.10170
    ## 290      689.1           0.35490
    ## 291      694.4           0.10080
    ## 292      697.7           0.77250
    ## 293      698.7           0.05836
    ## 294      698.8           0.05131
    ## 295      698.8           0.17110
    ## 296      699.4           0.17510
    ## 297      701.9           0.25660
    ## 298      705.6           0.14210
    ## 299      706.0           0.53430
    ## 300      706.0           0.14420
    ## 301      706.2           0.24740
    ## 302      706.6           0.10710
    ## 303      706.7           0.22640
    ## 304      708.8           0.13110
    ## 305      708.8           0.31670
    ## 306      709.0           0.41930
    ## 307      711.2           0.17730
    ## 308      711.4           1.05800
    ## 309      715.5           0.15130
    ## 310      718.9           0.20060
    ## 311      719.8           0.31240
    ## 312      725.9           0.13500
    ## 313      725.9           0.18240
    ## 314      728.3           0.27330
    ## 315      729.8           0.15170
    ## 316      733.5           0.56460
    ## 317      733.5           0.31710
    ## 318      734.6           0.14600
    ## 319      739.1           0.07622
    ## 320      739.3           0.54010
    ## 321      740.4           0.42250
    ## 322      740.7           0.39040
    ## 323      741.6           0.52490
    ## 324      745.3           0.12330
    ## 325      745.5           0.17880
    ## 326      749.1           0.11410
    ## 327      749.9           0.11090
    ## 328      749.9           0.14780
    ## 329      750.0           0.12520
    ## 330      750.1           0.43700
    ## 331      758.2           0.15810
    ## 332      759.4           0.41660
    ## 333      760.2           0.10110
    ## 334      762.4           0.93270
    ## 335      762.6           0.28830
    ## 336      764.0           0.24260
    ## 337      766.9           0.27350
    ## 338      767.3           0.24720
    ## 339      768.9           0.47060
    ## 340      773.4           0.15640
    ## 341      777.5           0.15500
    ## 342      782.1           0.09995
    ## 343      783.6           0.13880
    ## 344      784.7           0.46480
    ## 345      787.9           0.20430
    ## 346      788.0           0.39660
    ## 347      793.7           0.16370
    ## 348      799.6           0.42380
    ## 349      803.6           0.30890
    ## 350      803.7           0.17660
    ## 351      806.9           0.31220
    ## 352      808.2           0.36270
    ## 353      808.9           0.19760
    ## 354      808.9           0.42020
    ## 355      809.2           0.36350
    ## 356      809.7           0.25210
    ## 357      809.8           0.30300
    ## 358      811.3           0.40590
    ## 359      812.4           0.35420
    ## 360      819.1           0.17370
    ## 361      819.7           0.21670
    ## 362      826.0           0.32620
    ## 363      826.4           0.30260
    ## 364      826.4           0.13760
    ## 365      827.2           0.40990
    ## 366      828.5           0.34290
    ## 367      829.5           0.18810
    ## 368      830.5           0.26490
    ## 369      830.5           0.14150
    ## 370      830.6           0.12380
    ## 371      830.9           0.19970
    ## 372      831.0           0.20700
    ## 373      832.7           0.70900
    ## 374      840.8           0.07087
    ## 375      844.4           0.38560
    ## 376      848.7           0.28840
    ## 377      854.3           0.29790
    ## 378      856.9           0.21760
    ## 379      861.5           0.25500
    ## 380      862.0           0.19280
    ## 381      862.1           0.33710
    ## 382      867.1           0.33450
    ## 383      869.3           0.35680
    ## 384      873.2           0.15250
    ## 385      876.5           0.19240
    ## 386      880.8           0.20090
    ## 387      888.3           0.40610
    ## 388      888.7           0.57750
    ## 389      896.9           0.66430
    ## 390      897.0           0.36820
    ## 391      906.5           0.19430
    ## 392      906.6           0.27910
    ## 393      907.2           0.37240
    ## 394      909.4           0.49670
    ## 395      915.0           0.79170
    ## 396      915.3           0.50460
    ## 397      922.8           0.19490
    ## 398      925.1           0.34160
    ## 399      928.2           0.13610
    ## 400      928.8           0.45030
    ## 401      931.4           0.09866
    ## 402      932.7           0.21860
    ## 403      939.7           0.44620
    ## 404      943.2           0.65770
    ## 405      947.9           0.17220
    ## 406      959.5           0.62470
    ## 407      967.0           0.21010
    ## 408      971.4           0.21640
    ## 409      973.1           0.32990
    ## 410      975.2           0.21160
    ## 411      980.9           0.59540
    ## 412      981.2           0.46670
    ## 413      985.5           0.42900
    ## 414      988.6           0.18070
    ## 415      989.5           0.33310
    ## 416      993.6           0.15460
    ## 417     1009.0           0.16790
    ## 418     1021.0           0.17930
    ## 419     1025.0           0.42030
    ## 420     1030.0           0.20570
    ## 421     1031.0           0.47060
    ## 422     1032.0           0.17100
    ## 423     1035.0           0.55640
    ## 424     1044.0           0.40560
    ## 425     1050.0           0.23560
    ## 426     1070.0           0.44780
    ## 427     1084.0           0.29200
    ## 428     1088.0           0.44800
    ## 429     1095.0           0.44800
    ## 430     1102.0           0.35830
    ## 431     1121.0           0.29470
    ## 432     1124.0           0.30940
    ## 433     1138.0           0.18710
    ## 434     1150.0           0.15510
    ## 435     1153.0           0.52090
    ## 436     1156.0           0.23940
    ## 437     1165.0           0.46650
    ## 438     1175.0           0.30550
    ## 439     1189.0           0.39340
    ## 440     1210.0           0.14860
    ## 441     1210.0           0.09976
    ## 442     1218.0           0.14860
    ## 443     1222.0           0.24050
    ## 444     1223.0           0.20450
    ## 445     1226.0           0.51720
    ## 446     1227.0           0.28120
    ## 447     1228.0           0.19630
    ## 448     1229.0           0.38350
    ## 449     1233.0           0.22360
    ## 450     1236.0           0.11600
    ## 451     1239.0           0.34200
    ## 452     1260.0           0.21190
    ## 453     1261.0           0.58040
    ## 454     1261.0           0.12020
    ## 455     1269.0           0.61100
    ## 456     1269.0           0.35470
    ## 457     1272.0           0.49250
    ## 458     1284.0           0.41220
    ## 459     1292.0           0.26660
    ## 460     1295.0           0.28670
    ## 461     1298.0           0.28170
    ## 462     1299.0           0.56090
    ## 463     1302.0           0.34980
    ## 464     1304.0           0.59170
    ## 465     1313.0           0.24140
    ## 466     1315.0           0.42330
    ## 467     1320.0           0.18060
    ## 468     1321.0           0.22970
    ## 469     1332.0           0.39030
    ## 470     1344.0           0.35590
    ## 471     1349.0           0.37350
    ## 472     1359.0           0.39130
    ## 473     1362.0           0.20530
    ## 474     1403.0           0.21170
    ## 475     1408.0           0.37350
    ## 476     1410.0           0.24450
    ## 477     1417.0           0.29680
    ## 478     1421.0           0.18450
    ## 479     1426.0           0.23270
    ## 480     1436.0           0.25670
    ## 481     1437.0           0.37620
    ## 482     1437.0           0.47850
    ## 483     1461.0           0.39490
    ## 484     1479.0           0.29420
    ## 485     1485.0           0.27630
    ## 486     1493.0           0.25360
    ## 487     1495.0           0.20160
    ## 488     1535.0           0.28400
    ## 489     1538.0           0.22640
    ## 490     1540.0           0.34580
    ## 491     1549.0           0.22910
    ## 492     1567.0           0.50900
    ## 493     1575.0           0.20500
    ## 494     1589.0           0.38610
    ## 495     1590.0           0.22750
    ## 496     1600.0           0.30890
    ## 497     1603.0           0.20890
    ## 498     1603.0           0.34630
    ## 499     1606.0           0.25760
    ## 500     1610.0           0.56340
    ## 501     1623.0           0.61640
    ## 502     1623.0           0.32060
    ## 503     1628.0           0.37490
    ## 504     1645.0           0.25340
    ## 505     1646.0           0.33090
    ## 506     1648.0           0.24440
    ## 507     1651.0           0.74440
    ## 508     1656.0           0.29200
    ## 509     1657.0           0.15370
    ## 510     1660.0           0.38300
    ## 511     1670.0           0.42570
    ## 512     1671.0           0.34160
    ## 513     1681.0           0.73940
    ## 514     1688.0           0.56010
    ## 515     1696.0           0.33910
    ## 516     1709.0           0.42450
    ## 517     1724.0           0.38410
    ## 518     1731.0           0.19220
    ## 519     1740.0           0.37250
    ## 520     1748.0           0.40020
    ## 521     1750.0           0.35390
    ## 522     1750.0           0.23110
    ## 523     1760.0           0.35970
    ## 524     1780.0           0.23760
    ## 525     1809.0           0.31350
    ## 526     1813.0           0.65900
    ## 527     1819.0           0.41860
    ## 528     1821.0           0.86810
    ## 529     1844.0           0.29450
    ## 530     1866.0           0.23360
    ## 531     1872.0           0.27610
    ## 532     1873.0           0.48270
    ## 533     1926.0           0.53290
    ## 534     1933.0           0.59550
    ## 535     1938.0           0.44920
    ## 536     1946.0           0.30550
    ## 537     1956.0           0.18660
    ## 538     1972.0           0.31610
    ## 539     1986.0           0.41670
    ## 540     2009.0           0.32350
    ## 541     2010.0           0.31720
    ## 542     2019.0           0.66560
    ## 543     2022.0           0.31010
    ## 544     2027.0           0.21130
    ## 545     2053.0           0.41160
    ## 546     2073.0           0.42440
    ## 547     2081.0           0.57170
    ## 548     2089.0           0.75840
    ## 549     2145.0           0.35110
    ## 550     2215.0           0.35780
    ## 551     2227.0           0.38850
    ## 552     2232.0           0.38460
    ## 553     2360.0           0.69970
    ## 554     2384.0           0.47250
    ## 555     2398.0           0.31500
    ## 556     2403.0           0.35830
    ## 557     2477.0           0.40970
    ## 558     2499.0           0.15160
    ## 559     2562.0           0.60760
    ## 560     2615.0           0.26000
    ## 561     2642.0           0.41880
    ## 562     2782.0           0.36250
    ## 563     2906.0           0.26780
    ## 564     2944.0           0.41260
    ## 565     3143.0           0.16280
    ## 566     3216.0           0.40340
    ## 567     3234.0           0.59370
    ## 568     3432.0           0.26440
    ## 569     4254.0           0.42560

model \<- lm(area_worst\~)

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

Predictions on Y Given the compactness_worst and predict the area_worst
value: eg: we set the compactness_worst =1, the area_worst should be
2063.373

``` r
newdata<-data.frame(compactness_worst = 1)
predict(model, newdata)
```

    ##        1 
    ## 2063.373

``` r
#Using broom package 
(tidyed_lm <- broom::tidy(model))
```

    ## # A tibble: 2 × 5
    ##   term              estimate std.error statistic  p.value
    ##   <chr>                <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)           477.      40.8      11.7 1.98e-28
    ## 2 compactness_worst    1586.     137.       11.6 4.12e-28

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
here::here()
```

    ## [1] "/Users/haoxianglei/STAT545/milestone1-Haoxiang-Lei"

``` r
dir.create(here::here("output"))
```

    ## Warning in dir.create(here::here("output")): '/Users/haoxianglei/STAT545/
    ## milestone1-Haoxiang-Lei/output' already exists

``` r
# the summary_table is from Milestone1 4.2
mean_value <-cancer_sample %>%
  group_by(diagnosis) %>%
  summarise(area_worst_mean = mean(area_worst, na.rm = TRUE))

### median of area_worst in each group of diagnosis ###
median_value <- cancer_sample %>%
  group_by(diagnosis) %>%
  summarise(area_worst_median = median(area_worst, na.rm = TRUE))

### sum of area_worst in each group of diagnosis ###
sum_value <- cancer_sample %>%
  group_by(diagnosis) %>%
  summarise(area_worst_sum = sum(area_worst, na.rm = TRUE))

temp <- merge(mean_value, median_value, by = "diagnosis")
summary_table <- merge(temp, sum_value, by = "diagnosis")
Task4.2 <- summary_table

write_csv(Task4.2, here::here("output", "Task4.2.cvs"))
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 3.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

``` r
saveRDS(model, here::here("output", "model.rds"))
readRDS(here::here("output","model.rds"))
```

    ## 
    ## Call:
    ## lm(formula = area_worst ~ compactness_worst, data = cancer)
    ## 
    ## Coefficients:
    ##       (Intercept)  compactness_worst  
    ##             477.3             1586.1

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

-   In a sentence or two, explains what this repository is, so that
    future-you or someone else stumbling on your repository can be
    oriented to the repository.
-   In a sentence or two (or more??), briefly explains how to engage
    with the repository. You can assume the person reading knows the
    material from STAT 545A. Basically, if a visitor to your repository
    wants to explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least four folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output, and all data
    files saved from Task 4 above appear in the `output` folder.
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
