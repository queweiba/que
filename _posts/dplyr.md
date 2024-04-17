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
```

# Meta programming
## Data masking
You can use dara variables as it they were vallables in the enviroment.
## Data selection
You can choose valables based on their position, names , or type.

# Set function
Set operations
Description
Perform set operations using the rows of a data frame.

intersect(x, y) finds all rows in both x and y.

union(x, y) finds all rows in either x or y, excluding duplicates.

union_all(x, y) finds all rows in either x or y, including duplicates.

setdiff(x, y) finds all rows in x that aren't in y.

symdiff(x, y) computes the symmetric difference, i.e. all rows in x that aren't in y and all rows in y that aren't in x.

setequal(x, y) returns TRUE if x and y contain the same rows (ignoring order).

Note that intersect(), union(), setdiff(), and symdiff() remove duplicates in x and y.

Set Operations
Description
Performs set union, intersection, (asymmetric!) difference, equality and membership on two vectors.

Usage
union(x, y)
intersect(x, y)
setdiff(x, y)
setequal(x, y)
