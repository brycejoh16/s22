# DON'T START YET (UNDER REVISION)

# Project 2: Loan Analysis

## Corrections/Clarifications

* none yet

## Overview

Sadly, there is a long history of lending discrimination based on race
in the United States.  Lenders have literally drawn red
lines on a map around certain neighbourhoods where they would not
offer loans, based on the racial demographics of those neighbourhoods
(read more about redlining here: https://en.wikipedia.org/wiki/Redlining).
In 1975, congress passed the Home Mortgage Disclosure Act (HDMA) to
bring more transparency to this injustice
(https://en.wikipedia.org/wiki/Home_Mortgage_Disclosure_Act).  The
idea is that banks must report details about loan applications and
which loans they decided to approve.

The public HDMA dataset spans all the states and many years, and is documented here:
* https://www.ffiec.gov/hmda/pdf/2020guide.pdf
* https://cfpb.github.io/hmda-platform/#hmda-api-documentation

In this project, we'll analyze every loan application made in Wisconsin in
2020.

Things you'll practice:
* classes
* large datasets
* trees

## Testing

Run `python3 tester.py p2.ipynb` often and work on fixing any issues.


## Submission

As last time, your notebook should have a comment like this:

```python
# project: p2
# submitter: ????
# partner: none
# hours: ????
```

You'll hand in 3 files:
* p2.ipynb
* loans.py (first module developed in lab)
* search.py (second module developed in lab)

Combine these into a zip by running the following in the `p2` directory:

```
zip ../p2.zip p1.ipynb loans.py search.py
```

Hand in the resulting p2.zip file.

# Group Part (75%)

For this portion of the project, you may collaborate with your group
members in any way (even looking at working code).  You may also seek
help from 320 staff (mentors, TAs, instructor).  You <b>may not</b>
seek receive help from other 320 students (outside your group) or
anybody outside the course.

## Part 1: Loan Classes

Finish the `Applicant` and `Loan` classes from lab (if you haven't already done so): https://github.com/cs320-wisc/s22/blob/main/labs/lab4.md

We'll now add a `Bank` class to `loans.py`.  A `Bank` can be created like this (create an class with the necessary constructor for this to work):

```python
uwcu = loans.Bank("University of Wisconsin Credit Union")
```

### banks.json

The `__init__` of your `Bank` class should check that the given name appears in the `banks.json` file.  It should also lookup the `lei` ("Legal Entity Identifier") corresponding to the name and store that in an `lei` attribute.  In other words, `uwcu.lei` should give the LEI for UWCU, in this case "254900CN1DD55MJDFH69".

### wi.zip

The `__init__` should also read the loans from the CSV inside `wi.zip` for the given bank.  You already learned how to read text from a zip file in lab using `TextIOWrapper` and the `zipfile` module.

Read the documentation and example for how to read CSV files with `DictReader` here: https://docs.python.org/3/library/csv.html#csv.DictReader.  You can combine this with what you learned about zipfiles.  When you create a `DictReader`, just pass in a `TextIOWrapper` object instead of a regular file object.

As your `__init__` loops over the loan `dict`s, it should skip any that don't match the bank's `lei`.  The loan dicts that match should get converted to `Loan` objects and appended to a list, stored as an attribute in the `Bank` object.

### Special Methods

We don't tell you what to call the attribute storing the loans, but you should be able to print the last loan like this:

```python
print(uwcu.SOME_ATTRIBUTE_NAME[-1])
```

We can check how many loans there are with this:

```python
print(len(uwcu.SOME_ATTRIBUTE_NAME))
```

For convenience, we want to be able to directly use brackets and `len` directly on `Bank` objects, like this:
* `uwcu[-1]`
* `len(uwcu)`

Add the special methods to `Bank` necessary to make this work.

## Part 2: Binary Search Tree

Finish the `Node` and `BST` classes from lab (if you haven't already done so): https://github.com/cs320-wisc/s22/blob/main/labs/lab5.md

Add a special method to `BST` so that if `t` is a `BST` object, it is possible to lookup items with `t["some key"]` instead of `t.root.lookup("some key")`.

## Part 3: First Home Bank Analysis

For the following questions, create a `Bank` object for the bank named "First Home Bank".  Create a `BST` tree as well.  Loop over every loan in the bank, adding each to the tree.  The `key` passed to the `add` call should be the `.interest_rate` of the `Loan` object, and the `val` passed to `add` should be the `Loan` object itself.

### Q1: what is the average interest rate for the bank?

Skip missing loans where the interest rate is not specified in your calculation.

### Q2: how many applicants are there per loan, on average?

### Q3: what is the distribution of ages?

Answer with a dictionary, like this:

```
{'65-74': 21, '45-54': 21, ...}
```

### Q4: how many interest rate values are missing?

Don't loop over every loan to answer.  Use your tree to get and count loans with missing rates (that is, `-1`).

### Q5: how tall is the tree?

The height is the number of nodes in the path from the root to the deepest node.  Write a recursive function or method to answer.

# Individual Part (25%)

You have to do the remainder of this project on your own.  Do not
discuss with anybody except 320 staff (mentors, TAs, instructor).

## Part 4: University of Wisconsin Credit Union Analysis

Build a new `Bank` and corresponding `BST` object as before, but now for "University of Wisconsin Credit Union".

### Q6: how long does it take to add the loans to the tree?

Answer with a plot, where the x-axis is how many loans have been added so far, and the y-axis is the total time that has passed so far.  You'll need to measure how much time has elapsed (since the beginning) after each `.add` call using `time.time()`.

<img src="q6.png">

### Q7: how fast are tree lookups?

Create a bar plot with two bars:
1. time to find missing `interest_rate` values (`-1`) by looping over every loan and keeping a counter
2. time to compute `len(NAME_OF_YOUR_BANK_OBJECT[-1])`

<img src="q7.png">

### Q8: what is the relationship between property value and loan amount?

Answer with a scatter plot where each point is a loan, the x-axis is the property value, and the y-axis is the loan amount.  Use black points and `alpha` (transparency) of 0.01.

<img src="q8.png">

### Q9: what is the distribution of race for UWCU loan applicants?

Answer with a bar graph, where the x-axis is race, and the y-axis is number of applicants.  If an applicant has selected a single value for race, that is what should appear on the x-axis.  Otherwise, if they made multiple selections, they should be counted as "2+", and if they made zero selections, they should be counted as "unknown".

### Q10: how many nodes are in the tree?

Write a recursive function or method to count the nodes.

