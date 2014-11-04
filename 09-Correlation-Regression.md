# Stata Recitation - Week 9 - Correlation and Regression
McCourt School of Public Policy, Georgetown University

### MAIN IDEAS:
 - Corr and pwcorr
 - Basic regression and location of relevant output 
 - Understand the role of missing data in analysis sample 

## Correlation
 Quantifies the way two variables move together.
 Can be calculated for any numeric variables, but, like other summary statistics,
 interpretation is not always clear for categorical variables. 

### Example: Wage and Tenure
* Wage tends to increase with tenure

```
twoway (scatter wage tenure) (lfit wage tenure)
```

* Correlation is positive:

```
correlate wage tenure
```

* Average levels of education are lower for older people in this sample:

```
correlate age grade
```

* Correlate help page

```
help correlate
```

* Correlate can be abbreviated to cor
* Correlate takes a varlist

```
corr age grade wage tenure union
```

* When using correlate with multiple variables, what happens to observations with missing data?
* Compare sample size of previous command with observation counts from:

```
sum age grade wage tenure union
```

* If we don't want to use a single sample for entire correlation matrix, we can use pwcorr
* pwcorr is an alternative version of corr 

```
help corr
pwcorr age grade wage tenure union
```

* But, we want to see the number of observations for each comparison: Look at option list

```
pwcorr age grade wage tenure union, obs
```
 
### Correlate Practice Questions: Use `IPEDSpull_recitation.dta`, which can be found on Blackboard 

1. Create a variable, percent_women,  that gives the percentage of full-time students that are women.
2. Determine the correlation of percent_women with the graduation rate.
3. Using pwcorr, find the correlation between retention rate, graduation rate, transfer rate, and percent of first-time full-time students with financial aid. Give the number of observations for each correlation.
4. Tabulate the number of colleges in each region by size category. What percentage of colleges in the Far West are "Very Large"?
5. Tabulate the number of colleges in each region by whether the college is a land-grant college. Does it seem likely that the proportion of land-grant colleges is similar in each region?

### Answers:

```
use "C:\Users\gppilab\Desktop\IPEDSdata_recitation.dta", clear
gen percent_women = ftwomen / allfttotal
pwcorr percent_women gradrate
pwcorr ftretention gradrate transfer percentftftanyaid, obs sig
tab  region sizecat, row
tab region landgrant, chi2
```


## Regression 
We will cover how to run regressions and where to find output components.
Some output components have not been covered in class, and won't be covered until Quant II.
You don't have to know what the output means yet. 

### Regression example:

```
clear
sysuse auto

twoway (scatter mpg weight) (lfit mpg weight)
```

## Single-variable regression

```
regress mpg weight
```

* Review components of regression output 
* Many of these components won't be covered until Quant 2. 
* Overall output: Number of obs, R-squared, Sum of squares
* Variable specific output: Coefficients, standard errors, p-values
* Note: P-values are not actually zero, they just may have too many leading zeros to display.

### Regression on a restricted sample: domestic cars only

```
regress mpg weight if foreign==0
```

* Note number of observations

## Multiple-variable regression

```
regress mpg weight length
```

### Regression with missing values
* So far the variables that we've used have no missing values:

```
summarize mpg weight length
```

* What if mpg has some missing values?
* Go into data editor and make some observations equal to .
* Copy the resulting commands into your do-file:

```
replace mpg = . in 9
replace mpg = . in 18
replace mpg = . in 30
replace mpg = . in 50
replace mpg = . in 62 
```

* Run regression on modified data

```
regress mpg weight length
```

* What happens to number of observations? Why?
* What if other variables also have missing data in different observations?

```
replace weight = . in 7 
replace weight = . in 22 
replace weight = . in 27 
replace weight = . in 41 
replace weight = . in 60 
```

* Run regression on modified data

```
regress mpg weight length
```

* What happens to number of observations? Why?

* What if other variables also have missing data in the same observations?
* Change the length variable to missing for the same observations that are missing mpg.

```
replace length = . if mpg==. 
```

* Run regression on modified data

```
regress mpg weight length
```

* What happens to number of observations? Why?

### Practice Problems: Use IPEDSpull_recitation.dta, which can be found on Blackboard.

```
use "C:\Users\gppilab\Desktop\IPEDSdata_recitation.dta", clear
```

1. Regress the graduation rate (dependent variable) on the the number of full-time students. What does the relationship seem to be?
2. Regress the graduation rate (dependent variable) on the number of college employees, the percent of students with aid, and the full time retention rate. 
3. Regression example using if statement 
4. Regression example with missing data

### Answers

```

regress gradrate allfttotal
regress gradrate ftretention totemp percentftftanyaid

```

