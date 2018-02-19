---
title: "Gapminder"
author: "Aaron Shaffer"
date: "February 10, 2018"
output: html_document
---



<h1>Prompt</h1>

<h3>Using the gapminder data set, explore the question "How does life expectancy change over time for each country?"<h3>


<h2>Part 1. Read the Data</h2>


```r
suppressMessages(library(magrittr))
suppressMessages(library(ggplot2))
suppressMessages(library(plotly))
suppressMessages(library(webshot))

gapminder <- read.csv("gapminder-FiveYearData.csv")
```

<h2>Part 2. Visualizing the Data </h2>


```r
gapminder.gg <- ggplot(gapminder) +  
  geom_line(aes(x = year, y = lifeExp, color = country), 
              alpha = .85, stat = "smooth", method = "loess") + 
  facet_grid(~continent, scales = "free") + 
  theme(legend.position = "none",
        axis.text.x = element_text(angle = 60, hjust = 1),
        plot.margin = margin(0,0,50,10))
```

```r
ggplotly(gapminder.gg)
```
<center>

```
## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.
```

```
## Warning in normalizePath(f2): path[1]="./webshot2c9411b4648.png": No such
## file or directory
```

```
## Warning in file(con, "rb"): cannot open file './webshot2c9411b4648.png': No
## such file or directory
```

```
## Error in file(con, "rb"): cannot open the connection
```
</center>
<br>

<h3> Initial Takeaway </h3>

In order to make the GG plot look readable the legend needs to be temporarily hidden away because there are so many different countries in the data set.  However since this is an html document you can use the plotly package to make the graph interactive.  At a quick glance there definately seems to be a large difference in life expectancy over the years on a country to country basis and a quick LM will go on and show this. However printing the full thing will take a  lot of space consdering that there are 142 different countries so here is just a quick glance at the Asian Countries


```r
gapminder.asia.lm <- lm(lifeExp ~ country, 
                        data = gapminder[gapminder$continent == "Asia",])
library(pander)
pander(summary(gapminder.asia.lm))
```


-----------------------------------------------------------------------------
            &nbsp;               Estimate   Std. Error   t value   Pr(>|t|)  
------------------------------- ---------- ------------ --------- -----------
        **(Intercept)**           37.48       2.525       14.84    2.749e-39 

      **countryBahrain**          28.13       3.571       7.877    3.928e-14 

     **countryBangladesh**        12.36       3.571       3.46     0.000604  

      **countryCambodia**         10.42       3.571       2.919    0.003727  

       **countryChina**           24.31       3.571       6.807    4.132e-11 

  **countryHong Kong China**      36.01       3.571       10.09    2.979e-21 

       **countryIndia**           15.69       3.571       4.393    1.467e-05 

     **countryIndonesia**         16.86       3.571       4.721    3.359e-06 

        **countryIran**           21.16       3.571       5.925    7.242e-09 

        **countryIraq**            19.1       3.571       5.35     1.562e-07 

       **countryIsrael**          36.17       3.571       10.13    2.118e-21 

       **countryJapan**           37.35       3.571       10.46    1.481e-22 

       **countryJordan**          22.31       3.571       6.247    1.169e-09 

  **countryKorea Dem. Rep.**      26.13       3.571       7.317    1.636e-12 

     **countryKorea Rep.**        27.52       3.571       7.708    1.239e-13 

       **countryKuwait**          31.44       3.571       8.806    5.398e-17 

      **countryLebanon**          28.39       3.571       7.95     2.385e-14 

      **countryMalaysia**          26.8       3.571       7.506    4.766e-13 

      **countryMongolia**         18.41       3.571       5.156    4.15e-07  

      **countryMyanmar**          15.84       3.571       4.437    1.212e-05 

       **countryNepal**           11.51       3.571       3.223    0.001385  

        **countryOman**           20.96       3.571       5.871    9.779e-09 

      **countryPakistan**          17.4       3.571       4.874    1.638e-06 

    **countryPhilippines**        23.49       3.571       6.578    1.665e-10 

    **countrySaudi Arabia**        21.2       3.571       5.937    6.783e-09 

     **countrySingapore**         33.74       3.571       9.45      4.3e-19  

     **countrySri Lanka**         29.05       3.571       8.135    6.622e-15 

       **countrySyria**           23.87       3.571       6.684    8.768e-11 

       **countryTaiwan**          32.86       3.571       9.202    2.825e-18 

      **countryThailand**         24.72       3.571       6.923    2.01e-11  

      **countryVietnam**            20        3.571       5.601    4.208e-08 

 **countryWest Bank and Gaza**    22.85       3.571       6.399    4.822e-10 

     **countryYemen Rep.**        9.302       3.571       2.605    0.009566  
-----------------------------------------------------------------------------


--------------------------------------------------------------
 Observations   Residual Std. Error   $R^2$    Adjusted $R^2$ 
-------------- --------------------- -------- ----------------
     396               8.746          0.5006       0.4566     
--------------------------------------------------------------

Table: Fitting linear model: lifeExp ~ country

Lets take another look at life expectancy by continent per year.


```r
continent.gg <- ggplot(gapminder) + 
  geom_boxplot(aes(x = continent, y = lifeExp), width = 1) + 
  facet_wrap(~year, ncol = 3) + 
  theme(axis.text.x = element_text(angle = 60, hjust = 1),
        plot.margin = margin(0,0,45,10))
```

```r
ggplotly(continent.gg)
```
<center>


```
## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.
```

```
## Warning in normalizePath(f2): path[1]="./webshot2c948df35a1.png": No such
## file or directory
```

```
## Warning in file(con, "rb"): cannot open file './webshot2c948df35a1.png': No
## such file or directory
```

```
## Error in file(con, "rb"): cannot open the connection
```

</center>
<br>
<br>

On a Continent by continental basis its very clear that life expectancy differs greatly.  Lets investigate just a bit more closely For Asia.


```r
asian.lifeExp.gg <- ggplot(gapminder[gapminder$continent == "Asia",]) + 
  geom_boxplot(aes(x = country, y = lifeExp), width = 1) + 
  theme(axis.text.x = element_text(angle = 80, hjust = 1),
        plot.margin = margin(0,0,45,10))
```

```r
ggplotly(asian.lifeExp.gg)
```
<center>

```
## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.
```

```
## Warning in normalizePath(f2): path[1]="./webshot2c9428e78944.png": No such
## file or directory
```

```
## Warning in file(con, "rb"): cannot open file './webshot2c9428e78944.png':
## No such file or directory
```

```
## Error in file(con, "rb"): cannot open the connection
```
</center>

Now that its clear that there is a Difference of life expectancy by country lets investigate how `gdpPercap` influences this. 

```r
gpdPercap.bycountry.gg <- ggplot(gapminder, aes(x = year, y = gdpPercap, color = country)) + geom_point() + 
  geom_line(stat = "smooth", method = "loess") +
  facet_wrap(~continent, ncol = 1, scales = "free") +
  theme(legend.position = "none",
        plot.margin = margin(0,0,50,50))
```

```r
ggplotly(gpdPercap.bycountry.gg)
```
<center>

```
## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.
```

```
## Warning in normalizePath(f2): path[1]="./webshot2c941c1b6745.png": No such
## file or directory
```

```
## Warning in file(con, "rb"): cannot open file './webshot2c941c1b6745.png':
## No such file or directory
```

```
## Error in file(con, "rb"): cannot open the connection
```
</center>


```r
gapminder$gdpQuartile <- gapminder %>% group_by(year) %$% factor(dplyr::ntile(gdpPercap, 4))
gdpLifeExp.gg <- ggplot(gapminder, aes(x = as.factor(year), y = lifeExp, color = gdpQuartile)) + 
  geom_boxplot(alpha = .5, fill = NA) +
  geom_line(aes(group = gdpQuartile), stat = "smooth", method = "loess", alpha = .5) +
  facet_wrap(~continent, ncol = 1, scale = "free") +
  theme(legend.position = "none",
        plot.margin = margin(0,50,25,50)) 
```

```r
ggplotly(gdpLifeExp.gg)
```
<center>

```
## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.
```

```
## Warning in normalizePath(f2): path[1]="./webshot2c944e9aad2e.png": No such
## file or directory
```

```
## Warning in file(con, "rb"): cannot open file './webshot2c944e9aad2e.png':
## No such file or directory
```

```
## Error in file(con, "rb"): cannot open the connection
```
</center>

The things thats the most interesting about these graphs is before creating them I broke down `gdpQuartile` by year.  And you can see that in some years several continents didn't even have either the highest or lowest quartiles of gdp but regardless of that there is a clear trend that higher `gdpPer` Is having some effects on increased life expectancy.  Countries are of course a major part of it but there are clear gaps between each level of `gdpQuartile`

<h2>Conclusions</h2>

On a country by country basis `gdpPer` or GDP Per Capita does show that the life expectancy of an indivual relates directly to the wealth of your nation.  Visualizing the data and creating some linear models for this helps to show that in a way thats more easily digestable for people.  None of my visualization models or graphs take `pop` into consideration but using all of the other variables together the effects of wealth and location have on life expectancy is quite clear.

Some trickery has gone into the plotting of the graphs.  And that will be covered in the separate R file for the walkthrough of knitting this one.

