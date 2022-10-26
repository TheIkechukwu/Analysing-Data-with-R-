# Analysing-Data-with-R-

## Using Tidyverse To Explore a Dataset

### What is Tidyverse
Tidyverse is a system of packages in R with a common desin philosophy for data manipulation, exploration and visualization. This includes core packages like:

* ggplot2
* Tidyr
* readr
* Dplyr
and many more.
To read more about Analyzing Data with R [click here](https://medium.com/@ikechukwuemeka7/data-analysis-with-r-programming-9754739453e)

First, I had to use the install.packages() to install tidyverse and the library() to load it

```{r tidyverse, message=FALSE, warning=FALSE}
install.packages("tidyverse")
library(tidyverse)
```
For illustrative purposes, I'll be using the Sourav Banerjee's World population dataset. To upload this dataset, I used read_csv()

```{r world_population}
population <- read_csv("world_population.csv")
```
You can use the View(), glimpse() and head() to have a look at your data. 
NB: Assignment operator (<-) is used to assign valuesto variables and vectors
```{r populatio}
head(population)
```
Use colnames() to the column names of your dataset
```{r colnames}
colnames(population)
```
### Data Cleaning

Use the rename() to rename the columns '2022 Population', '2020 Population' and '2015 Population'
```{r rename col}
population_2 <- population %>%
  rename("population_2022" = "2022 Population", "population_2020" = "2020 Population", "population_2015" = "2015 Population")
```
Let's manipulate the data to display only the population of African countries in 2022 with over 1 million people using the select() and filter()
```{r African countries}
african_countries <- select(population_2, Continent, Country, population_2022) %>%
  filter(Continent == "Africa", population_2022 >= "1000000")
```
Again, Let's view our new data, "African_countries", using head()
```{r african_countries}
head(african_countries)
```
## Organizing your data

Now we have a data containing only African countries with more a million people, we can further organize this data in such a way it will be more meaningful with arrange()

```{r arrange}
populous_nations <- african_countries %>%
  arrange(desc(population_2022))
head(populous_nations)
```
From our new table you can easily see that the Top 3 most populous nations in Africa are; 
* Nigeria 
* Ethiopia, and 
* Egypt

### Plotting
To achieve great visualization, you use the gglot() function
Lets make a plot that will reveal Nigeria's population growth from 2015 to 2022
```{r Nigeria}
Nigeria <- population_2 %>%
  select(Country, population_2022, population_2020, population_2015) %>%
  filter(Country == "Nigeria")
```

It's important to rearrange our table with vectors and data.frame()
*Vectors* are a group of data elements of the same type stored in a sequence.

```{r}
population_n <- c(218541212,208327405,183995785)
year <- c(2022, 2020, 2015)
Nigeria_population <- data.frame(population_n, year)
```


```{r ggplot, warning=FALSE}
ggplot(data=Nigeria_population) +
  geom_smooth(mapping = aes(x = year, y = population_n)) +
  labs( title = "Nigeria", subtitle = "The Population Growth of Nigeria from 2015 to 2022")
```
