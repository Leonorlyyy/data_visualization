data_visualization1
================
2024-09-26

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = case_match(
      id, 
      "USW00094728" ~ "CentralPark_NY", 
      "USW00022534" ~ "Molokai_HI",
      "USS0023B17S" ~ "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## using cached file: /Users/leyangrui/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2024-09-26 10:27:04.333212 (8.651)

    ## file min/max dates: 1869-01-01 / 2024-09-30

    ## using cached file: /Users/leyangrui/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2024-09-26 10:27:22.710166 (3.932)

    ## file min/max dates: 1949-10-01 / 2024-09-30

    ## using cached file: /Users/leyangrui/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2024-09-26 10:27:28.464777 (1.036)

    ## file min/max dates: 1999-09-01 / 2024-09-30

Making first plot

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
weather_df |>
  ### mutate |>
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
ggp_weather_scatterplot = 
  weather_df |>
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point()
```

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .3, size = .8) +
### more transparent and smaller
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).
    ## Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->

``` r
### no error bars


## Where you define aesthetics matter
weather_df |>
  ### only x and y apply through the graph --> the line only applies to the single pair of x and y
  ggplot(aes(x = tmin, y = tmax)) +
  ### color only apply to points
  geom_point(aes(color = name), alpha = .3, size = .8) +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).
    ## Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->

Use facetting real quick

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .3) +
  geom_smooth(se = FALSE) +  
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
### separate rows and columns
## . means nothing, does not split rows, split the column according to names
```

Let’s make a somewhat more interesting scatterplot

``` r
weather_df |>
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point(aes(size = prcp), alpha = .3) + 
  geom_smooth(se = FALSE) +
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Learning Assessment1:

``` r
weather_df |>
  filter(name == "CentralPark_NY") |>
  mutate(
    tmin_fahr = tmin * (9/5) + 32,
    tmax_fahr = tmax * (9/5) + 32
  ) |>
  ggplot(aes(x = tmin_fahr, y = tmax_fahr)) +
  geom_point() +
  ### lm: use linear function
  geom_smooth(method = "lm", se = FALSE)
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](data_visualization1_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax)) +
  geom_hex()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_binhex()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
### show data density
```

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax, color = "blue")) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(color = "blue")
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-10-2.png)<!-- -->

## Univariate plots

``` r
weather_df |>
  ggplot(aes(x = tmin)) +
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmin, fill = name)) +
  geom_histogram(position = "dodge")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-11-2.png)<!-- -->

### Fix by facet:

``` r
weather_df |>
  ggplot(aes(x = tmin, fill = name)) +
  geom_histogram() +
  facet_grid(. ~ name)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmin)) +
  geom_histogram(aes(fill = name)) +
  facet_grid(. ~ name)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-12-2.png)<!-- -->

Maybe a density plot?

``` r
weather_df |>
  ggplot(aes(x = tmin, fill = name)) +
  geom_density(alpha = .3)
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

### Boxplot

``` r
weather_df |>
  ggplot(aes(x = name, y = tmin, fill = name)) +
  geom_boxplot()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

### Violin plots

``` r
weather_df |>
  ggplot(aes(x = name, y = tmin, fill = name)) +
  geom_violin()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

``` r
### symmetric density plots
```

### Ridge Plot

``` r
### density separated vertically according to Y
## categorical vs. numeric
weather_df |>
  ggplot(aes(x = tmin, y = name)) +
  geom_density_ridges()
```

    ## Picking joint bandwidth of 1.41

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density_ridges()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

Learning Assessment 2

``` r
weather_df |>
  ggplot(aes(x = prcp)) +
  geom_histogram(aes(fill = name)) +
  facet_grid(. ~ name)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 15 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

``` r
weather_df |>
  filter(prcp > 10) |>
  ggplot(aes(x = prcp, fill = name)) +
  geom_density(alpha = .3)
```

![](data_visualization1_files/figure-gfm/unnamed-chunk-17-2.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = name, y = prcp, fill = name)) +
  geom_boxplot()
```

    ## Warning: Removed 15 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-17-3.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = name, y = prcp, fill = name)) +
  geom_violin()
```

    ## Warning: Removed 15 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-17-4.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = prcp, y = name)) +
  geom_density_ridges()
```

    ## Picking joint bandwidth of 9.22

    ## Warning: Removed 15 rows containing non-finite outside the scale range
    ## (`stat_density_ridges()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-17-5.png)<!-- -->

## Saving plots

``` r
gg_weather =
weather_df |>
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point()

## default: save the most recent plot
ggsave("plots/ggp_weather.pdf", gg_weather, width = 8, height = 6)
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

## Embedding plots

``` r
weather_df |>
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_visualization1_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->
