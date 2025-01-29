Lab 03 - Nobel laureates
================
Benjamin Egan
1/27

## Exercises

### Exercise 1

Using dataset “Nobel”, which examines information about nobel winners

There are 26 variables and 935 rows. Each row is a person (so in theory
there are 934 people in the nobel dataset).

``` r
glimpse(nobel)
```

    ## Rows: 935
    ## Columns: 26
    ## $ id                    <dbl> 1, 2, 3, 4, 5, 6, 6, 8, 9, 10, 11, 12, 13, 14, 1…
    ## $ firstname             <chr> "Wilhelm Conrad", "Hendrik A.", "Pieter", "Henri…
    ## $ surname               <chr> "Röntgen", "Lorentz", "Zeeman", "Becquerel", "Cu…
    ## $ year                  <dbl> 1901, 1902, 1902, 1903, 1903, 1903, 1911, 1904, …
    ## $ category              <chr> "Physics", "Physics", "Physics", "Physics", "Phy…
    ## $ affiliation           <chr> "Munich University", "Leiden University", "Amste…
    ## $ city                  <chr> "Munich", "Leiden", "Amsterdam", "Paris", "Paris…
    ## $ country               <chr> "Germany", "Netherlands", "Netherlands", "France…
    ## $ born_date             <date> 1845-03-27, 1853-07-18, 1865-05-25, 1852-12-15,…
    ## $ died_date             <date> 1923-02-10, 1928-02-04, 1943-10-09, 1908-08-25,…
    ## $ gender                <chr> "male", "male", "male", "male", "male", "female"…
    ## $ born_city             <chr> "Remscheid", "Arnhem", "Zonnemaire", "Paris", "P…
    ## $ born_country          <chr> "Germany", "Netherlands", "Netherlands", "France…
    ## $ born_country_code     <chr> "DE", "NL", "NL", "FR", "FR", "PL", "PL", "GB", …
    ## $ died_city             <chr> "Munich", NA, "Amsterdam", NA, "Paris", "Sallanc…
    ## $ died_country          <chr> "Germany", "Netherlands", "Netherlands", "France…
    ## $ died_country_code     <chr> "DE", "NL", "NL", "FR", "FR", "FR", "FR", "GB", …
    ## $ overall_motivation    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ share                 <dbl> 1, 2, 2, 2, 4, 4, 1, 1, 1, 1, 1, 1, 2, 2, 1, 1, …
    ## $ motivation            <chr> "\"in recognition of the extraordinary services …
    ## $ born_country_original <chr> "Prussia (now Germany)", "the Netherlands", "the…
    ## $ born_city_original    <chr> "Lennep (now Remscheid)", "Arnhem", "Zonnemaire"…
    ## $ died_country_original <chr> "Germany", "the Netherlands", "the Netherlands",…
    ## $ died_city_original    <chr> "Munich", NA, "Amsterdam", NA, "Paris", "Sallanc…
    ## $ city_original         <chr> "Munich", "Leiden", "Amsterdam", "Paris", "Paris…
    ## $ country_original      <chr> "Germany", "the Netherlands", "the Netherlands",…

### Exercise 2

Filtered out people who don’t have a country, organizations that won a
nobel, and people who are dead

``` r
nobel_living <- nobel %>%
 filter(country != "NA" & gender != "org" & is.na(died_date))
```

### Exercise 3

Created a dataset for American winners in physics, medicine, chemistry,
and economics

``` r
#are they in the US
nobel_living <- nobel_living %>%
  mutate(
    country_us = if_else(country == "USA", "USA", "Other")
  )

#Physics, Medicine, Chemistry, and Economics
nobel_living_science <- nobel_living %>%
  filter(category %in% c("Physics", "Medicine", "Chemistry", "Economics"))
```

Graph for the relationship between the category of prize and whether the
laureate was in the US when they won the nobel prize

``` r
ggplot(
  data = nobel_living_science,
  mapping = aes(
    x = country_us,
  )
) +
  facet_wrap(~category)+
  theme_bw()+
  geom_bar()+
  coord_flip()+
  labs(
      x = "Country",
      y = "Number of Prize Winners",
      title = "Relationship Between Prize Winner and Country"
  )
```

![](lab-03_files/figure-gfm/visual%20plot-1.png)<!-- --> More USA
winners in each category. Economics has a disproportionate number of USA
winners. This isn’t supporting much to support the buzzfeed article.

### Exercise 4

``` r
#adding in country born in US to the other nobel living
us_born_nobel <- nobel_living_science %>%
  mutate(
    born_country = if_else(born_country == "USA", "USA", "Other")
  )

us_born_nobel %>%
count(born_country)
```

    ## # A tibble: 2 × 2
    ##   born_country     n
    ##   <chr>        <int>
    ## 1 Other          123
    ## 2 USA            105

105 people born in the USA, 123 were not. …

### Exercise 5

Plot for born in USA winners

``` r
ggplot(
  data = us_born_nobel,
  mapping = aes(
    x = born_country,
  )
) +
   geom_bar()+
  facet_wrap(~category)+
  theme_bw()+
  coord_flip()+
  labs(
      x = "Birth Country",
      y = "Number of Prize Winners",
      title = "Relationship Between Prize Winner and Birth Country"
  )
```

![](lab-03_files/figure-gfm/born%20in%20USA-1.png)<!-- --> Buzzfeed got
it right about more nobel winners immigrating to USA. The only exception
is if we’re talking about economics, as econ winners mostly are US
citizens in the united states.

…

### Exercise 6

``` r
nobel_international <- nobel %>%
 filter(country == "USA" & born_country != "USA") %>%
  count(born_country) %>%
  arrange(desc(n)) %>%
  print()
```

    ## # A tibble: 37 × 2
    ##    born_country       n
    ##    <chr>          <int>
    ##  1 United Kingdom    15
    ##  2 Canada            12
    ##  3 Germany           10
    ##  4 China              6
    ##  5 Poland             6
    ##  6 France             5
    ##  7 Italy              5
    ##  8 Japan              5
    ##  9 Austria            4
    ## 10 Hungary            4
    ## # ℹ 27 more rows

UK is most common for US immigrants who won the nobel prize.

``` r
rnorm(5, mean = 0, sd = 1)
```

    ## [1]  0.47229037 -1.22279934  0.22116256  0.70893757 -0.05047655
