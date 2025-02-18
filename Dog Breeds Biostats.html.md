---
title: "BioStats Project: Dog Breeds"
format: html
editor: visual
execute:
  keep-md: true
---




## Dog Breeds Biostatistical Analysis

Analysis of traits of dogs by breed.



::: {.cell}

```{.r .cell-code}
#install.packages("tidytuesdayR")
```
:::

::: {.cell}

```{.r .cell-code}
tuesdata <- tidytuesdayR::tt_load('2022-02-01')
```

::: {.cell-output .cell-output-stderr}

```
---- Compiling #TidyTuesday Information for 2022-02-01 ----
--- There are 3 files available ---


── Downloading files ───────────────────────────────────────────────────────────

  1 of 3: "breed_traits.csv"
  2 of 3: "trait_description.csv"
  3 of 3: "breed_rank.csv"
```


:::
:::

::: {.cell}

```{.r .cell-code}
breed_traits <- tuesdata$breed_traits
```
:::

::: {.cell}

```{.r .cell-code}
breed_traits <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/main/data/2022/2022-02-01/breed_traits.csv")
```

::: {.cell-output .cell-output-stderr}

```
Rows: 195 Columns: 17
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr  (3): Breed, Coat Type, Coat Length
dbl (14): Affectionate With Family, Good With Young Children, Good With Othe...

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


:::
:::

::: {.cell}

```{.r .cell-code}
trait_description <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/main/data/2022/2022-02-01/trait_description.csv")
```

::: {.cell-output .cell-output-stderr}

```
Rows: 16 Columns: 4
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr (4): Trait, Trait_1, Trait_5, Description

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


:::
:::

::: {.cell}

```{.r .cell-code}
breed_rank_all <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/main/data/2022/2022-02-01/breed_rank.csv')
```

::: {.cell-output .cell-output-stderr}

```
Rows: 195 Columns: 11
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr (3): Breed, links, Image
dbl (8): 2013 Rank, 2014 Rank, 2015 Rank, 2016 Rank, 2017 Rank, 2018 Rank, 2...

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


:::
:::

::: {.cell}

```{.r .cell-code}
library(tidyverse)
```

::: {.cell-output .cell-output-stderr}

```
── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
✔ dplyr     1.1.4     ✔ readr     2.1.5
✔ forcats   1.0.0     ✔ stringr   1.5.1
✔ ggplot2   3.5.1     ✔ tibble    3.2.1
✔ lubridate 1.9.4     ✔ tidyr     1.3.1
✔ purrr     1.0.2     
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```


:::
:::

::: {.cell}

```{.r .cell-code}
breed_traits
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 195 × 17
   Breed    Affectionate With Fa…¹ Good With Young Chil…² `Good With Other Dogs`
   <chr>                     <dbl>                  <dbl>                  <dbl>
 1 Retriev…                      5                      5                      5
 2 French …                      5                      5                      4
 3 German …                      5                      5                      3
 4 Retriev…                      5                      5                      5
 5 Bulldogs                      4                      3                      3
 6 Poodles                       5                      5                      3
 7 Beagles                       3                      5                      5
 8 Rottwei…                      5                      3                      3
 9 Pointer…                      5                      5                      4
10 Dachshu…                      5                      3                      4
# ℹ 185 more rows
# ℹ abbreviated names: ¹​`Affectionate With Family`, ²​`Good With Young Children`
# ℹ 13 more variables: `Shedding Level` <dbl>, `Coat Grooming Frequency` <dbl>,
#   `Drooling Level` <dbl>, `Coat Type` <chr>, `Coat Length` <chr>,
#   `Openness To Strangers` <dbl>, `Playfulness Level` <dbl>,
#   `Watchdog/Protective Nature` <dbl>, `Adaptability Level` <dbl>,
#   `Trainability Level` <dbl>, `Energy Level` <dbl>, `Barking Level` <dbl>, …
```


:::
:::

::: {.cell}

```{.r .cell-code}
trait_description
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 16 × 4
   Trait                      Trait_1                    Trait_5     Description
   <chr>                      <chr>                      <chr>       <chr>      
 1 Affectionate With Family   Independent                Lovey-Dovey How affect…
 2 Good With Young Children   Not Recommended            Good With … A breed's …
 3 Good With Other Dogs       Not Recommended            Good With … How genera…
 4 Shedding Level             No Shedding                Hair Every… How much f…
 5 Coat Grooming Frequency    Monthly                    Daily       How freque…
 6 Drooling Level             Less Likely to Drool       Always Hav… How drool-…
 7 Coat Type                  -                          -           Canine coa…
 8 Coat Length                -                          -           How long t…
 9 Openness To Strangers      Reserved                   Everyone I… How welcom…
10 Playfulness Level          Only When You Want To Play Non-Stop    How enthus…
11 Watchdog/Protective Nature What's Mine Is Yours       Vigilant    A breed's …
12 Adaptability Level         Lives For Routine          Highly Ada… How easily…
13 Trainability Level         Self-Willed                Eager to P… How easy i…
14 Energy Level               Couch Potato               High Energy The amount…
15 Barking Level              Only To Alert              Very Vocal  How often …
16 Mental Stimulation Needs   Happy to Lounge            Needs a Jo… How much m…
```


:::
:::

::: {.cell}

```{.r .cell-code}
breed_rank_all
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 195 × 11
   Breed `2013 Rank` `2014 Rank` `2015 Rank` `2016 Rank` `2017 Rank` `2018 Rank`
   <chr>       <dbl>       <dbl>       <dbl>       <dbl>       <dbl>       <dbl>
 1 Retr…           1           1           1           1           1           1
 2 Fren…          11           9           6           6           4           4
 3 Germ…           2           2           2           2           2           2
 4 Retr…           3           3           3           3           3           3
 5 Bull…           5           4           4           4           5           5
 6 Pood…           8           7           8           7           7           7
 7 Beag…           4           5           5           5           6           6
 8 Rott…           9          10           9           8           8           8
 9 Poin…          13          12          11          11          10           9
10 Dach…          10          11          13          13          13          12
# ℹ 185 more rows
# ℹ 4 more variables: `2019 Rank` <dbl>, `2020 Rank` <dbl>, links <chr>,
#   Image <chr>
```


:::
:::

::: {.cell}

```{.r .cell-code}
#install.packages("tidymodels")
library(tidymodels)
```

::: {.cell-output .cell-output-stderr}

```
── Attaching packages ────────────────────────────────────── tidymodels 1.2.0 ──
```


:::

::: {.cell-output .cell-output-stderr}

```
✔ broom        1.0.7     ✔ rsample      1.2.1
✔ dials        1.3.0     ✔ tune         1.2.1
✔ infer        1.0.7     ✔ workflows    1.1.4
✔ modeldata    1.4.0     ✔ workflowsets 1.1.0
✔ parsnip      1.2.1     ✔ yardstick    1.3.2
✔ recipes      1.1.0     
```


:::

::: {.cell-output .cell-output-stderr}

```
── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
✖ scales::discard() masks purrr::discard()
✖ dplyr::filter()   masks stats::filter()
✖ recipes::fixed()  masks stringr::fixed()
✖ dplyr::lag()      masks stats::lag()
✖ yardstick::spec() masks readr::spec()
✖ recipes::step()   masks stats::step()
• Search for functions across packages at https://www.tidymodels.org/find/
```


:::

```{.r .cell-code}
my_data_splits <- initial_split(breed_traits, prop = 0.5)

exploratory_data <- training(my_data_splits)
test_data <- testing(my_data_splits)
```
:::

::: {.cell}

```{.r .cell-code}
names(breed_rank_all)<-
  janitor::make_clean_names(names(breed_rank_all))
```
:::

::: {.cell}

```{.r .cell-code}
library (kableExtra)
```

::: {.cell-output .cell-output-stderr}

```

Attaching package: 'kableExtra'
```


:::

::: {.cell-output .cell-output-stderr}

```
The following object is masked from 'package:dplyr':

    group_rows
```


:::
:::

::: {.cell}

```{.r .cell-code}
library(tidyverse)
```
:::

::: {.cell}

```{.r .cell-code}
#install.packages("janitor")
names(exploratory_data)<-
  janitor::make_clean_names(names(exploratory_data))
```
:::

::: {.cell}

```{.r .cell-code}
names(breed_traits)<-
  janitor::make_clean_names(names(breed_traits))
```
:::



# Abstract

The ideas of small dog breeds having "Little Dog Syndrome" and large dog breeds being "Gentle Giants"come mostly from social expectations of these dog breeds. America's favorite large and small dog breeds were used to test these social ideas. The data was consistent with large dogs indeed being "gentle giants." However, the idea of "Little Dog Syndrome" was tested and found not to be shown in the data set as analyzed.

# Introduction

Dogs are often called man's best friend. However, there are just so many dog breeds. The personality of a dog can vary widely based on a number of different factors, including breed. The type of dog preferred by any one person depends on multitudinous factors such as cuteness, size, coat type, and personality traits. A person's opinion of small and large dog breeds commonly comes from a small handful of strongly positive or negative experiences with such dogs. Those who feel strongly about their favorite breeds of dogs may have biases for many reasons, including the social ideas of "Little Dog Syndrome" and larger dogs being considered "Gentle Giants".

"Little Dog Syndrome" is a term used to describe the personality traits of smaller dogs. Often, it is used to accuse small dog breeds of being quite barky, less trainable, not open to strangers, and hyper. The idea of large dogs being "Gentle Giants" is often used by rescue groups who explain that the bad reputation given to large dog breeds comes from bad owners and it is in the nature of large dogs to not act badly. These "Gentle Giants" are more likely to be good with children and other dogs, more open to strangers, and more protective of their homes and families.

# Hypotheses

If there is truth to the idea of “Little Dog Syndrome,” then smaller dog breeds will score unfavorably for trait scores in openness to strangers, trainability, energy level, and barking level.

If there is truth to the social idea of big dogs being “Gentle Giants,” then larger dog breeds will score favorably in good with young children and other dogs, openness to strangers, and watchdog/protective nature.

# Determining the Top 10 Large & Small Dog Breeds

The top 10 Large and Small dog breeds will be selected to narrow the data set. Dogs that are considered medium-sized will be ignored. The rankings from the most recent year included in the data set are what will be considered.



::: {.cell}

```{.r .cell-code}
breed_rank_all %>%
  select("x2020_rank", breed)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 195 × 2
   x2020_rank breed                        
        <dbl> <chr>                        
 1          1 Retrievers (Labrador)        
 2          2 French Bulldogs              
 3          3 German Shepherd Dogs         
 4          4 Retrievers (Golden)          
 5          5 Bulldogs                     
 6          6 Poodles                      
 7          7 Beagles                      
 8          8 Rottweilers                  
 9          9 Pointers (German Shorthaired)
10         10 Dachshunds                   
# ℹ 185 more rows
```


:::
:::

::: {.cell}

```{.r .cell-code}
breed_rank_all %>%
  select("x2020_rank", breed) %>%
  filter(breed == "Retrievers (Labrador)"| breed == "German Shepherd Dogs" | breed == "Retrievers (Golden)" | breed == "Poodles" | breed== "Rottweilers" | breed == "Pointers (German Shorthaired)" | breed == "Australian Shepherds" | breed == "Boxers" | breed == "Great Danes" | breed == "Siberian Huskies")
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 10 × 2
   x2020_rank breed                        
        <dbl> <chr>                        
 1          1 Retrievers (Labrador)        
 2          3 German Shepherd Dogs         
 3          4 Retrievers (Golden)          
 4          6 Poodles                      
 5          8 Rottweilers                  
 6          9 Pointers (German Shorthaired)
 7         12 Australian Shepherds         
 8         14 Boxers                       
 9         15 Great Danes                  
10         16 Siberian Huskies             
```


:::
:::



List of top ten large dog breeds to be used for the sake of testing hypothesis: If there is truth to the social idea of big dogs being “Gentle Giants,” then larger dog breeds will score favorably in good with young children and other dogs, openness to strangers, and watchdog/protective nature.



::: {.cell}

```{.r .cell-code}
breed_rank_all %>%
  select("x2020_rank", breed) %>%
  filter(breed == "French Bulldogs"| breed == "Dachshunds" | breed == "Pembroke Welsh Corgis" | breed == "Yorkshire Terriers" | breed == "Cavalier King Charles Spaniels" | breed == "Miniature Schnauzers" | breed == "Shih Tzu" | breed == "Boston Terriers" | breed == "Pomeranians" | breed == "Havanese")
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 10 × 2
   x2020_rank breed                         
        <dbl> <chr>                         
 1          2 French Bulldogs               
 2         10 Dachshunds                    
 3         11 Pembroke Welsh Corgis         
 4         13 Yorkshire Terriers            
 5         17 Cavalier King Charles Spaniels
 6         19 Miniature Schnauzers          
 7         20 Shih Tzu                      
 8         21 Boston Terriers               
 9         23 Pomeranians                   
10         24 Havanese                      
```


:::
:::



List of top ten small dog breeds to be used to test Hypothesis: If there is truth to the idea of “Little Dog Syndrome,” then smaller dog breeds will score unfavorably for trait scores in openness to strangers, trainability, energy level, and barking level.

# Breed Attributes Analysis Large Dogs

attempt filter command according to slack instructions identify column numbers



::: {.cell}

```{.r .cell-code}
breed_traits %>%
   select(breed, `watchdog_protective_nature`, `good_with_young_children`, `good_with_other_dogs`, `openness_to_strangers`, `trainability_level`) %>%
  slice(1, 3, 4, 6, 8, 12, 14, 15, 16, 18)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 10 × 6
   breed      watchdog_protective_…¹ good_with_young_chil…² good_with_other_dogs
   <chr>                       <dbl>                  <dbl>                <dbl>
 1 Retriever…                      3                      5                    5
 2 German Sh…                      5                      5                    3
 3 Retriever…                      3                      5                    5
 4 Poodles                         5                      5                    3
 5 Rottweile…                      5                      3                    3
 6 Australia…                      3                      5                    3
 7 Boxers                          4                      5                    3
 8 Great Dan…                      5                      3                    3
 9 Siberian …                      1                      5                    5
10 Doberman …                      5                      5                    3
# ℹ abbreviated names: ¹​watchdog_protective_nature, ²​good_with_young_children
# ℹ 2 more variables: openness_to_strangers <dbl>, trainability_level <dbl>
```


:::
:::



Large dogs scored consistently with the idea of large breeds being "Gentle Giants". The selected large dog breeds scored well in every selected trait category with little exception

# Breed Attributes Analysis Small Dogs



::: {.cell}

```{.r .cell-code}
breed_traits %>%
   select(breed, `energy_level`, `barking_level`, `good_with_other_dogs`, `openness_to_strangers`, `trainability_level`) %>%
  slice(2, 10, 11, 13, 17, 19, 20, 21, 23, 24)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 10 × 6
   breed   energy_level barking_level good_with_other_dogs openness_to_strangers
   <chr>          <dbl>         <dbl>                <dbl>                 <dbl>
 1 French…            3             1                    4                     5
 2 Dachsh…            3             5                    4                     4
 3 Pembro…            4             4                    4                     4
 4 Yorksh…            4             4                    3                     5
 5 Cavali…            3             3                    5                     4
 6 Miniat…            3             5                    3                     3
 7 Shih T…            3             3                    5                     3
 8 Boston…            4             2                    4                     5
 9 Pomera…            3             4                    3                     3
10 Havane…            3             4                    5                     5
# ℹ 1 more variable: trainability_level <dbl>
```


:::
:::



The only scores consistent with the idea of "Little Dog Syndrome" are the score trends for energy and barking levels. With a few exceptions the small breeds tend to have a higher energy level and higher barking levels. The small dog breeds scored well with consideration to being good with other dogs and strangers as well as their trainability scores.

# Acknowledgment of Bias

Selecting the top breeds for each size group may prove problematic due to a higher breed rank, most likely being some of the best-behaved of each group. It may be better to select from all across the breed rank list or perhaps also look at the lowest scorers in terms of rank to see if they also following the trends. Selecting the breeds to be used based off of how they ranked could bias the data in favor of the dog sizes being better behaved than is truthful for the trend of such breeds.
