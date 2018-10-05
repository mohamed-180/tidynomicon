---
title: "Simple Things in Python and R"
output: md_document
permalink: /beginnings/
---



Let's try some simple things at an interactive prompt.

## How do I greet the world?

Let us begin as is traditional,
using blue to show Python and green to show R:


```python
print("Hello, world!")
```

```
## Hello, world!
```


```r
print("Hello, world!")
```

```
## [1] "Hello, world!"
```

A sense of weariness settles over us immediately.
Python prints exactly what we have asked for,
but what is that `[1]` in R's output?
Is it perhaps something akin to a line number?
Let's take a closer look by simply evaluating an expression without bothering with `print`:


```r
'This is in single quotes.'
```

```
## [1] "This is in single quotes."
```

```r
"This is in double quotes."
```

```
## [1] "This is in double quotes."
```

Oh dear.
It appears that R uses double quotes to display strings even when we give it a single-quoted string
(which is no worse than Python using single quotes when we've given it doubles).
The bad news is that the mysterious `[1]` doesn't appear to be a line number.
Let's ignore it for now and do a little more exploring.

## How do I add numbers?

In Python,
I add numbers using `+`.
(Note that, as above, we use `print` to display the output instead of simply typing in the expression.
This is a feature of the current version of RStudio.)


```python
print(1 + 2 + 3)
```

```
## 6
```

I can check the type of the result using the built-in `type` function,
which in this case tells me that my `6` is an integer:


```python
print(type(6))
```

```
## <class 'int'>
```

What does R do?


```r
1 + 2 + 3
```

```
## [1] 6
```

```r
typeof(6)
```

```
## [1] "double"
```

Hm.
Calling the type-checking function `typeof` rather than `type` isn't frightening,
and having it return the name of a type rather than a class can also be excused,
but it does seem odd for integer addition to produce a double-precision floating-point result
(which is what the type `"double"` means).
Let's try an experiment:


```r
typeof(6)
```

```
## [1] "double"
```

Ah ha.
By default,
R represents numbers as floating-point values,
even if they look like integers when written.
We can force a literal value to be an integer by putting an upper-case L after it:


```r
typeof(6L)
```

```
## [1] "integer"
```

Arithmetic on integers produces integers:


```r
1L + 2L + 3L
```

```
## [1] 6
```

```r
typeof(1L + 2L + 3L)
```

```
## [1] "integer"
```

and if we want to convert a "normal" (floating-point) number to an integer
we can do so:


```r
as.integer(6)
```

```
## [1] 6
```

```r
typeof(as.integer(6))
```

```
## [1] "integer"
```

But wait:
what is that dot doing in that function's name?
Is there an object called `as` with a method called `integer`?
No.
In R,
`.` is just another character.
Like the underscore `_`,
it is used to make names more readable,
but it has no special meaning.

## How do I store many numbers together?

The Elder Gods do not bother to learn most of our names because there are so many of us and we are so…ephemeral.
Similarly, we only give proper names to a handful of values in our programs;
we lump the rest together into lists and matrices and more esoteric structure so that
we too can create, manipulate, and dispose of multitudes with a single dark command.

In Python,
we create a list using square brackets,
and assign that list to a variable using `=`.
If the variable does not exist, it is created:


```python
primes = [3, 5, 7, 11]
```

Since assignment is a statement rather than an expression,
it has no result,
so Python does not display anything when this command is run.

The equivalent operation in R uses a function called `c`,
which stands for "column":


```r
primes <- c(3, 5, 7, 11)
```

Assignment is done using a left-pointing arrow `<-`.
Like Python,
R does not display a value after an assignment statement.

Now that we can create vectors in R,
we can explain that errant `[1]` in our previous examples.
To begin with,
let's have a look at the lengths of various things in Python:


```python
print(len(primes))
```

```
## 4
```


```python
print(len(4))
```

```
## TypeError: object of type 'int' has no len()
## 
## Detailed traceback: 
##   File "<string>", line 1, in <module>
```

Fair enough:
the length of a list is the number of elements it contains,
and since a scalar value like the integer 4 doesn't contain elements,
we are committing a minor sin by asking for its length.

Now,
what of R?


```r
length(primes)
```

```
## [1] 4
```

Good.


```r
length(4)
```

```
## [1] 1
```

That's unexpected.
Let's have a closer look:


```r
typeof(primes)
```

```
## [1] "double"
```

That's also unexpected,
but all becomes clear once we realize that *there are no scalars in R*.
`4` is not a single lonely integer,
but rather a vector of length one containing the value 4.
When we display its value,
the `[1]` is the index of its first value.
We can prove this by creating and displaying a much longer vector:


```r
longer <- c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
longer
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10  1  2  3  4  5  6  7  8  9 10  1  2  3
## [24]  4  5  6  7  8  9 10  1  2  3  4  5  6  7  8  9 10
```

In order to help us find out way in our data,
R automatically breaks long lines
and displays the starting index of each line.
These index values also show us that R counts from 1 as humans do,
rather than from zero.
(There are a great many myths about why programming languages do the latter.
If you dare know the truth,
[Mike Hoye has it for you](http://exple.tive.org/blarg/2013/10/22/citation-needed/).)

## How do I index a ~~list~~ vector?

Python's rules are simple once you understand them
(a statement which is also true of quantum mechanics).
To avoid confusing indices with values,
let's create a list of color names and index that:


```python
colors = ["eburnean", "glaucous", "wenge"]
print(colors[0])
```

```
## eburnean
```

```python
print(colors[2])
```

```
## wenge
```

```python
colors[3]
```

```
## IndexError: list index out of range
## 
## Detailed traceback: 
##   File "<string>", line 1, in <module>
```

```python
print(colors[-1])
```

```
## wenge
```

Indexing the equivalent vector in R with the indices 1 to 3 produces unsurprising results:


```r
colors <- c("eburnean", "glaucous", "wenge")
colors[1]
```

```
## [1] "eburnean"
```

```r
colors[3]
```

```
## [1] "wenge"
```

What happens if we go off the end?


```r
colors[4]
```

```
## [1] NA
```

Our understanding of the universe is unavoidably incomplete:
limited as they are to three dimensions of space and one of time,
our minds simply cannot grasp reality without shattering into a million gibbering pieces.
In statistics,
this is often reflected by gaps in data.
R uses the special value `NA` (short for "not available") to represent these gaps,
and tries to shield us from the full horror of the unknowable by returning `NA`
when we ask for knowledge that does not exist.

R does more than this—much more.
In Python,
a negative index counts backward from the end of a list.
In R,
we use a negative index to indicate a value that we don't want:


```r
colors[-1]
```

```
## [1] "glaucous" "wenge"
```

But wait.
If every value in R is a vector,
then when we use 1 or -1 as an index,
we're actually using a vector to index another one.
What happens if the index itself contains more than one value?


```r
colors[1, 2]
```

```
## Error in colors[1, 2]: incorrect number of dimensions
```

All right, that didn't work.
What if we make the vector subscript explicit using `c`?


```r
colors[c(3, 1, 2)]
```

```
## [1] "wenge"    "eburnean" "glaucous"
```

```r
colors[c(1, 1, 1)]
```

```
## [1] "eburnean" "eburnean" "eburnean"
```

```r
colors[c(-1, -2)]
```

```
## [1] "wenge"
```

```r
colors[c(1, -1)]
```

```
## Error in colors[c(1, -1)]: only 0's may be mixed with negative subscripts
```

This looks promising.
If we provide a vector of positive indices,
we get the elements they identify in their order,
with elements duplicated if an index was used multiple times.
The same thing happens if we use negative indices,
and if we try to mix them,
R heaves a heavy sigh and chides us for our mistake,
presumably because an index like `c(1, -1)` is ambiguous.

What of zero?


```r
colors[0]
```

```
## character(0)
```

In order to understand this rather cryptic response,
we can try calling the function `character` ourselves
with a positive argument:


```r
character(3)
```

```
## [1] "" "" ""
```

Ah—it appears that `character` constructs a vector of character strings of the specified length
and fills it with empty strings.
The expression `character(0)` presumably therefore means
"an empty character vector".
From this,
we conclude that the index 0 doesn't correspond to any elements,
so R gives us back something of the right type but with no content.
As a check,
let's try indexing with 0 and 1 together:


```r
colors[c(0, 1)]
```

```
## [1] "eburnean"
```

This unfortunate behavior is fertile ground for breeding bugs.
Be on guard against it.

## How do I choose and repeat things?

We cherish the illusion of free will so much that we embed a pretense of it in our machines
in the form of conditional statements.
We then instruct those same machines to make the same decisions over and over,
often for no discernible purpose.
For example,
we could write the following in Python:


```python
values = [-1, 0, 1]
for v in values:
    if v < 0:
        sign = -1
    elif v == 0:
        sign = 0
    else:
        sign = 1
    print("The sign of", v, "is", sign)
```

```
## The sign of -1 is -1
## The sign of 0 is 0
## The sign of 1 is 1
```

and then create something equivalent in R:


```r
values <- c(-1, 0, 1)
for (v in values) {
  if (v < 0) {
    sign <- -1
  }
  else if (v == 0) {
    sign <- 0
  }
  else {
    sign <- 1
  }
  print(paste("The sign of", v, "is", sign))
}
```

```
## [1] "The sign of -1 is -1"
## [1] "The sign of 0 is 0"
## [1] "The sign of 1 is 1"
```

There are a few things to note here:

1.  The parentheses in the loop header are required:
    we cannot simply write `for v in values`.
2.  The curly braces around the bodies of the loop and the conditional branches are optional,
    but should always be there to help readability.
3.  `paste` converts its arguments to strings and then concatenates those,
    placing a single space between each unless instructed to do otherwise.
    `print` then prints the resulting (single) string.
    The functions `cat` and `message` can also be used for text output.
4.  By calling our temporary variable `sign`
    we risk collision with the rather useful built-in R function of the same name.

Explicit loops in R programs are generally regarded with the same sort of suspicion
as hearty claims by over-confident graduate students that
the text they are about to read aloud is purely metaphorical,
as are the dire warnings scrawled in the margins of the ancient tome in which it was found.
If we wish to get the signs of some values in R,
we can more idiomatically do this:


```r
result <- sign(c(-1, 0, 1))
```

Vectors being ubiquitous in R,
almost every function will operate on multiple values.
And as sequences such as (-1, 0, 1) are also very common,
R provides a notation for expressing them:

or even:


```r
result <- sign(-1:1)
```

Where Python only allows ranges such as `-1:1` to be used as indices,
R considers them first-class entities.
Their most common use is as indices to vectors:


```r
colors <- c("eburnean", "glaucous", "squamous", "wenge")
colors[1:3]
```

```
## [1] "eburnean" "glaucous" "squamous"
```
We can similarly subtract a range of colors by index:

```r
colors[-1:-3]
```

```
## [1] "wenge"
```

R does not allow tripartite expressions of the form `start:end:stride`;
for that,
we must use the `seq` function:


```r
seq(1, 10, 3)
```

```
## [1]  1  4  7 10
```

This example also shows that ranges in R are inclusive at both ends,
i.e.,
they run up to *and including* the upper bound.
As is traditional among programming language advocates,
we claim that this is more natural,
and then cite as proof some supportive anecdote such as,
"Most people do not interpret the expression 'from one to five'
to mean 'one, two, three, or four'."

What we *cannot* do is use vectors directly as conditions in `if` statements:


```r
numbers <- c(0, 1, 2)
if (numbers) {
  print("This should not have worked.")
}
```

```
## Warning in if (numbers) {: the condition has length > 1 and only the first
## element will be used
```

Instead,
we must collapse them into a single logical (Boolean) value:


```r
numbers <- c(0, 1, 2)
if (all(numbers >= 0)) {
  print("This, on the other hand, should work.")
}
```

```
## [1] "This, on the other hand, should work."
```

When evaluating the expression `numbers >= 0`,
R automatically replicates or *recycles* the shorter vector
(in this case, the vector of length 1 containing the value 0)
and then performs element-by-element comparison to construct a vector of logical values:


```r
numbers >= 0
```

```
## [1] TRUE TRUE TRUE
```

The function `all` then checks if they are all `TRUE`.
We could use a corresponding function `any` to check if at least one was `TRUE`;
these two functions therefore correspond to "and" and "or" across the whole vector.

Logical vectors have a special place in the R universe,
just as human beings don't in the larger one.
If a vector is indexed with a logical vector,
the result is a vector containing only the values at locations where the index vector is `TRUE`.
This is easier to show than to describe:


```r
both <- -2:2         # c(-2, -1, 0, 1, 2)
mask <- both > 0     # c(FALSE, FALSE, FALSE, TRUE, TRUE)
result <- both[mask] # c(1, 2)
result
```

```
## [1] 1 2
```

We can of course make the code shorter by eliminating `mask`:


```r
result <- both[both > 0]
result
```

```
## [1] 1 2
```

## How do I create and call functions?

As we have already seen,
we call functions in R much as we do in Python:


```r
max(1, 3, 5) + min(1, 3, 5)
```

```
## [1] 6
```

We can define a new function using the `function` keyword.
Doing so does not name the function;
to accomplish that,
we assign the newly-created function to a variable:


```r
swap <- function(pair) {
  c(pair[2], pair[1])
}
swap(c("left", "right"))
```

```
## [1] "right" "left"
```

As this example shows,
the result of a function is the value of the last expression evaluated within it.
A function can return a value earlier using the `return` function;
we can use `return` for the final value as well,
but most R programmers do not.


```r
swap <- function(pair) {
  if (length(pair) != 2) {
    return(NULL) # This is very bad practice.
  }
  c(pair[2], pair[1])
}
swap(c("one"))
```

```
## NULL
```

```r
swap(c("left", "right"))
```

```
## [1] "right" "left"
```

We pause now to answer some questions that may have occurred to attentive readers.
First,
the special value `NULL` represents the absence of a vector.
It is not the same as a vector of zero length,
though testing that statement produces a rather odd result:


```r
NULL == integer(0)
```

```
## logical(0)
```

To test for nullity,
use the function `is.null`:


```r
is.null(NULL)
```

```
## [1] TRUE
```

Second,
and more importantly,
returning `NULL` when our function's inputs are invalid is an express route to madness,
as doing so means that `swap` can fail without telling us that it has done so.
Consider:


```r
NULL[1]  # Try to access an element of the vector that does not exist.
```

```
## NULL
```

```r
values <- 5:10          # More than two values.
result <- swap(values)  # Attempting to swap the values produces NULL.
result[1]               # But we can operate on the result without error.
```

```
## NULL
```

The next section will explore what we ought to do instead.
Before then,
however,
we can offer some slight comfort.
If the number of arguments given to a function is not the number expected,
R will complain:


```r
swap("one", "two", "three")
```

```
## Error in swap("one", "two", "three"): unused arguments ("two", "three")
```

Note that in this example we as passing three values,
not a single vector containing three values.
If we want a function to handle a varying number of arguments,
we represent the "extra" arguments with an ellipsis `...` (three dots),
which serves the same purpose as Python's `*args`:


```r
print_with_title <- function(title, ...) {
  title <- paste("==", title, "==\n")
  items <- paste(..., sep = "\n")
  cat(title)
  cat(items)
  cat("\n")
}
print_with_title("to-do", "Monday", "Tuesday", "Wednesday")
```

```
## == to-do ==
## Monday
## Tuesday
## Wednesday
```

If we want to work with the extra arguments one by one,
we must first convert `...` to a list:


```r
add <- function(...) {
  result <- 0
  for (value in list(...)) {
    result <- result + value
  }
  result
}
add(1, 3, 5, 7)
```

```
## [1] 16
```

A list in R is a vector that can contain values of many different types.
R's lists are complex enough that we will devote an entire section to exploring them;
until then,
suffice to say that they can be iterated over,
but that indexing them may surprise you.

## How do I live without loops?

Modern Python encourages programmers to use list comprehensions instead of loops,
e.g.,
to write:


```python
original = [3, 5, 7, 9]
doubled = [2 * x for x in original]
print(doubled)
```

```
## [6, 10, 14, 18]
```

instead of:


```python
doubled = []
for x in original:
  doubled.append(2 * x)
print(doubled)
```

```
## [6, 10, 14, 18]
```

If `original` is a NumPy array,
we can shorten this to:


```python
doubled = 2 * original
```

As we have already seen,
R provides the same capability in the language itself:


```r
original <- c(3, 5, 7, 9)
doubled <- 2 * original
```

If we want to perform more complex calculations,
R encourages us to use a functional style of programming,
in which data is transformed by successive operations
much as reality itself is transformed by the will of the Great Old Ones.
We begin by defining a function that does what we want:


```r
double <- function(x) {
  2 * x
}
```

and then pass this function to `sapply` along with the data on which we want to operate:


```r
sapply(1:10, double)
```

```
##  [1]  2  4  6  8 10 12 14 16 18 20
```

This is clumsier than simply multiplying our values by two,
but it illustrates the way in which *higher-order functions* work.
We can use other higher-order functions to perform other operations,
such as conditionals:


```r
values <- -5:5
ifelse(values > 0, values, NA)
```

```
##  [1] NA NA NA NA NA NA  1  2  3  4  5
```

R provides many other higher-order functions,
but their semantics are sometimes confusing.
We therefore encourage programmers to use the functions provided by the tidyverse instead;
these will be discussed in [the next lesson](../tidyverse/).