# first need to organize it in the tibble
## rownames 
Rownames cannot be duplicated in a table
- check whether there are rownames
```{r}
has_rownames(mtcars)
#> [1] TRUE
```
- remove row names
```{r}
remove_rownames(mtcars) %>% has_rownames()
#> [1] FALSE
```
- convert between rownames and column
```{r}
#transfer rownames to column
mtcars_tbl <- rownames_to_column(mtcars, var = "car") %>% as_tibble()
mtcars_tbl
#> # A tibble: 32 × 12
#>    car     mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
#>    <chr> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1 Mazd…  21       6  160    110  3.9   2.62  16.5     0     1     4     4
#>  2 Mazd…  21       6  160    110  3.9   2.88  17.0     0     1     4     4
#>  3 Dats…  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
#>  4 Horn…  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
#>  5 Horn…  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
#>  6 Vali…  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
#>  7 Dust…  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
#>  8 Merc…  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
#>  9 Merc…  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
#> 10 Merc…  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
#> # … with 22 more rows
#transfer a certain column to rownames
column_to_rownames(mtcars_tbl, var = "car") %>% head()
#>                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
#> Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
#> Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
#> Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
#> Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
#> Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
#> Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

# Adding rowid as a column --------------------------------------------
rowid_to_column(trees) %>% head()
#>   rowid Girth Height Volume
#> 1     1   8.3     70   10.3
#> 2     2   8.6     65   10.3
#> 3     3   8.8     63   10.2
#> 4     4  10.5     72   16.4
#> 5     5  10.7     81   18.8
#> 6     6  10.8     83   19.7

```

# Kable function in R
## some general settings 
- Set how the NA is displayed in the table
```{r}
options(knitr.kable.NA="-")
options(opts)  # restore global R options
```

- Set how many digits to put in the table
```{r}
knitr::kable(d, digits = 4)
# round columns separately
knitr::kable(d, digits = c(5, 0, 2))
# add commas to big numbers
knitr::kable(d, digits = 3, format.args = list(big.mark = ",",
  scientific = FALSE))
```

# Kable_extra function in R
- set font size
```{r}
kable(head(iris, 5), booktabs = TRUE) %>%
  kable_styling(font_size = 8)
```
- style specific rows
```{r}
kable(head(iris, 5), align = 'c', booktabs = TRUE) %>%
  row_spec(1, bold = TRUE, italic = TRUE) %>% 
  row_spec(2:3, color = 'white', background = 'black') %>%
  row_spec(4, underline = TRUE, monospace = TRUE) %>% 
  row_spec(5, angle = 45) %>% 
  column_spec(5, strikeout = TRUE)
```
- group rows and columns.
  - `add_header_above` to header that can be grouped according to current column numbers
```{r}
iris2 <- iris[1:5, c(1, 3, 2, 4, 5)]
names(iris2) <- gsub('[.].+', '', names(iris2))
kable(iris2, booktabs = TRUE) %>%
  add_header_above(c("Length" = 2, "Width" = 2, " " = 1)) %>% 
  add_header_above(c("Measurements" = 4, "More attributes" = 1))
```
  - `pack_rows()` adds additonal rows for groups based on the original row number
```{r}
iris3 <- iris[c(1:2, 51:54, 101:103), ]
kable(iris3[, 1:4], booktabs = TRUE) %>% pack_rows(
  index = c("setosa" = 2, "versicolor" = 4, "virginica" = 3)
)
```
