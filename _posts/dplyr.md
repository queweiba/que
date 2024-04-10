# Pipe line
The pipe operator will forward a value, orthe results of an expression, into the next function call/expression
## Usage
1. simple usage,
- If the right hand side expression has one argument, the pipe will just put the lhs to the rhs
- if the rhs has two argument, pipe will pass the left hand side as the first input to the right hand side function call/expression.
```{r}
1 %>% '/' (2)
```
This is equivalent as `r 1/2` 
2. With place holder ".", we can indicate which argument we want the lhs to put in.
```{r}
1 %>% '/' (2,.)
```
3. for nested functions (one function is nested in another function), lhs will be injected in both functions as the first input in there are no place holder
```{r}
list(a=1) %>% '/' (.$a,2)
# this will be read as '/' (.,.$a,2) and cause error
```
to prevent this, we use `r {}`
```{r}
list(a=1) %>% {'/' (.$a,2)}
# ```
