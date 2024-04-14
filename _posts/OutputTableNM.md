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
  - `R add_header_above` to header that can be grouped according to current column numbers
```{r}
iris2 <- iris[1:5, c(1, 3, 2, 4, 5)]
names(iris2) <- gsub('[.].+', '', names(iris2))
kable(iris2, booktabs = TRUE) %>%
  add_header_above(c("Length" = 2, "Width" = 2, " " = 1)) %>% 
  add_header_above(c("Measurements" = 4, "More attributes" = 1))
```
