
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.4     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(dplyr)
```

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
deaths <- av |>
  pivot_longer(cols = starts_with("Death"), names_to = "Time", values_to = "Death") |>
  mutate(Time = parse_number(Time)) 
```

Similarly, deal with the returns of characters.

``` r
returns <- av |>
  pivot_longer(cols = starts_with("Return"), names_to = "Time", values_to = "Return") |>
  mutate(Time = parse_number(Time))
```

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
avg_deaths = length(which(deaths$Death == "YES")) / length(unique(deaths$URL))
print(avg_deaths)
```

    ## [1] 0.5144509

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

### Nadia’s FiveThirtyEight Statement

“There’s … a 50 percent chance \[Avengers\] recovered from a second or
third death.”

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
deaths2_3 <- av %>% select(Name.Alias, Death2, Return2, Death3, Return3) %>%
  filter(Death2 == "YES")
# 16 Avengers died a second time
count(deaths2_3)
```

    ##    n
    ## 1 16

``` r
# 8 Avengers returned from their second death
deaths2_3 %>% filter(Return2 == "YES") %>% count()
```

    ##   n
    ## 1 8

``` r
# 2 Avengers died a third time
deaths2_3 %>% filter(Death3 == "YES") %>% count()
```

    ##   n
    ## 1 2

``` r
# 1 Avenger returned from their third death
deaths2_3 %>% filter(Return3 == "YES") %>% count()
```

    ##   n
    ## 1 1

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.  

#### Deo Shaji

- My statement to verify: Out of the 173 listed Avengers , my analysis
  found that 69 had died atleast once since they had joined the
  avengers.

``` r
total_deaths <- deaths |>
  filter(Death == 'YES') 


length(unique(total_deaths$URL))
```

    ## [1] 69

- I filtered the deaths dataset based on the Death column having a
  ‘YES’. Then I filtered based on the unique URLs which gave me the
  number of avengers who had died atleast once since they had joined the
  team. My value of 69 matches with what the article originally
  mentioned. Initially, I was going to count the number of unique
  name.Alias but I noticed some of the rows were empty for name.Alias.,
  which is why I used URL

## Grace Wu

#### FiveThirtyEight Statement

- “I counted 89 total deaths — some unlucky Avengers7 are basically Meat
  Loaf with an E-ZPass — and on 57 occasions the individual made a
  comeback.”

#### Code

``` r
length(which(deaths$Death == "YES"))
```

    ## [1] 89

``` r
length(which(returns$Return == "YES"))
```

    ## [1] 57

#### Answer

- I counted the total number of deaths by counting the number of times
  Death in the deaths data set returned “YES”. Then I counted the total
  number of returns by counting the number of times Return in the data
  set returned “YES”. My value for total deaths was 89, and my value for
  total returns was 57, which aligns with the analysis statement above.

- My statement: “I counted 89 total deaths — some unlucky Avengers7 are
  basically Meat Loaf with an E-ZPass — and on 57 occasions the
  individual made a comeback.”
