# Stata Recitation - Week 10 - End of Semester Project 
McCourt School of Public Policy, Georgetown University

### Students Should Know
- Develop a do-file to transform initial data set to analysis data set
- Document results using comments
- Use relative path names to organize a project

### RESOURCES
[http://www.haverford.edu/TIER]
- Advice for organizing your do-files and overall project
- Sample research projects with initial data, all do-files, and final research paper 

## Warm Up problems using the `nslw88.dta` example dataset:
```
sysuse nlsw88.dta, clear
```
1. Regress hourly wage on total workforce experience. As your workforce experience increases, what is the predicted effect on your wage?
2. Predict the wage for a worker with 14 years of experience.
3. Three people in the dataset have exactly 14.25 years of experience. List their actual wages.
4. Regress the effect of hourly wage on total workforce experience, being in a union, living in a city, and being a college graduate. As your workforce experience increases, controlling for these factors, what is the predicted effect on your wage?

### Answer to the warm up problems
```
*1
regress wage ttl_exp

* Effect of 1 unit (year) increase in experience is a 0.3314 increase in hourly wage

*2
display 3.61+0.3314*14
*or:
predict wagep 
list wagep if ttl_exp == 14

*3
list wage if ttl_exp == 14.25

*4
regress wage ttl_exp union c_city collgrad
* Effect of a 1 unit (year) increase in experience is a 0.295 increase in wage, controlling for these other factors (keeping them at the same level)
```

## End of Semester Project 

At the end of this semester, students will be given a simulation project. 

It will be larger and more complex than any exercises we've done so far. 

This recitation will simulate a large data project. 

We will complete a small and simple project, but organize the project as if it were a large and complex project.

### Set-up
First create your main project folder, titled `recitation10`, on the desktop. 

Download the data set auto_raw.dta from Blackboard and save in recitation10 folder.

Open Stata. Type `use auto_raw.dta` in the command prompt. Did it work?

If no, you need to set the ***working directory***. 


### Working Directory 

Stata has a single working directory where it is looking at any given time. 

It is displayed at the bottom of the window or use pwd.

If you try to use/save a file without using the full path name to the file, Stata will only look in the current working directory.

See what is currently in your working directory: ls / dir

Working directory is unrelated to any saving or opening that you do using the drop-down menus in Stata. 

The only way to change it is with the cd command or the Change Working Directory menu item.

Use the Change Working Directory drop-down to change to the recitation folder.

Copy the cd command to the do-file as the first line.

You can also add the use and save commands to your do-file.

```

*change working directory
*cd "C:\Users\gppilab\Desktop\Recitation 10"

use auto_raw.dta, clear

* Stata commands

save auto_final.dta, replace

```

- As you progress through the project, you will develop the do-file to create your final analysis data set. 
- You generally do not have to save intermediate data sets. 
- Instead, just fill in the commands to create your final data set. 
- If you make a mistake, fix it in the do-file and re-run it.


Suppose you received the following data description. Fill in your do-file 
with commands and comments to replicate the analysis that follows:

This study looks at the relationship between vehicle mileage, weight, length, 
and price. We also examine whether this relationship changes according to the 
vehicle's repair record.

We had to correct two problems in price reporting. First, prices for domestic 
automobiles were reported in 1978 nominal dollars, while prices for foreign 
automobiles were reported in 1977 nominal dollars. To correct for this 
discrepency, we inflated the prices of foreign automobiles by the 1977 
inflation rate of 6.8%. 

We also know that some very high and very low prices are mistakes in the data.
The maximum automobile price in 1978 was $15,000 and the minimum price was
4,000. For prices outside this range, we recoded the value of price to 
the minimum or maximum value. Prices higher than $15,000 were recoded to
exactly $15,000, and prices lower than $4,000 were recoded to exactly $4,000.
This operation changed prices for 8 observation. The mean of the corrected price
variable is $6,311.49.

After this correction of price, we had to drop several observations for 
different reasons.  Five records were dropped because, they were missing data on their 
repair record. Furthermore, data on mileage for Datsun vehicles is known to be inaccurate,
so all Datsun automobiles were dropped from the sample. Thus, our final 
analysis sample contained 65 records.

Before dropping these records, we compared the average price of the vehicles 
were were dropping from the analysis to the average price of the vehicles that
would remain in the analysis. We found that the dropped vehicles cost $264.71 
more, on average, than non-dropped vehicles. This difference was not statistically
different from zero. 

In addition to the basic variables, mileage, weight, and length, we analysed
specifications with variable transformations including the natural logs of
mileage, weight, and length, and the ratio of weight to length. 

When analyzing records with different repair records, we divided the sample
into two groups. The first group had a low repair record, with a value of
1, 2, or 3. The second group had a high repair record, with a value of 4 or 5.
The distribution of the final sample was roughly 60% low, 40% high.

We also wanted to compare cars by categories of price. We created a new 
categorical variable, price_cat, to divide cars into 4 categories according
to the following conditions on price: 
1 Less than or equal to 4,000
2 Greater than 4,000 and less than or equal to 5,000
3 Greater than 5,000 and less than or equal to 10,000
4 Greater than 10,000
We found that the largest category was number 2, with 26 vehicles or 40 percent
of the distribution. 

For our final analysis, we ran a regression of mileage on length, weight, and price. 
We repeated this specification, using only vehicles with a low repair record.
Finally, we ran the same regression, omitting those vehicles in the highest and lowest price categories.   
The final sample sizes for our three final regressions were 65, 40, and 48.


```
clear
use auto_raw.dta


*** Correct price data ***
replace price = price * 1.068 if foreign==1

replace price = 4000 if price < 4000
replace price = 15000 if price > 15000
codebook price
* Verify number of changes 
* Verify mean of corrected price variable: 6,364.73.	


*** Sample Restriction ***

* Generate a variable to mark the observations that will be dropped:
gen todrop = 0

* Drop 5 records with missing data from rep78
codebook rep78
replace todrop = 1 if rep78==.
* Drop Datsuns
tab make
replace todrop = 1 if word(make,1)=="Datsun"
* Verify resulting number of observations:
tab todrop
* 65 observations

* Compare the price of the sample observations to the observations that will be dropped:
ttest price , by(todrop)
* Difference is -264.7114 
* P-value for two-tailed test is : Pr(|T| > |t|) = 0.8027

* Drop the "todrop" observations
drop if todrop==1


*** Variable Creation ***

* First examine variables
codebook mpg weight length price
sum mpg weight length price, de

* Create Log variables
gen lnmpg	=ln(mpg)
gen lnweight=ln(weight)
gen lnlength=ln(length)

* Label log variables
lab var lnmpg "Log MPG"
lab var lnweight "Log Weight"
lab var lnlength "Log Length"

* Create and label ratio variable
gen weightperinch = weight/length
lab var weightperinch "Weight Per Inch"

sum lnmpg lnweight lnlength weightperinch

* Create indicator for high rep78 (4 or 5)
gen highrep = 0
replace highrep = 1 if rep78 >= 4
replace highrep = . if rep78 ==.

*OR in one line with recode
recode rep78 (1 2 3 = 0 "Low Rep") (4 5 = 1 "High Rep"), gen(highrep2)

* verify variable creation
tab rep78 highrep, missing
tab rep78 if highrep ==1

* verify 60-40 distribution
tab highrep, m
* 61.54 percent low, 38.46 percent high

* Create Price categorical
gen price_cat=.
replace price_cat=1 if price <=4000
replace price_cat=2 if price > 4000  & price <= 5000 
replace price_cat=3 if price > 5000  & price <= 10000 
replace price_cat=4 if price > 10000 & price <  . 

* Verify variable creation with sum of the different categories.
sum price if price_cat== 1
sum price if price_cat== 2
sum price if price_cat== 3
sum price if price_cat== 4

* Report distribution of categories:
tab price_cat

*** Final Regressions ***
regress mpg weight length price
regress mpg weight length price if highrep==0
regress mpg weight length price if price_cat!=1 & price_cat!=4

*Save modifed version of the data
save auto_final.dta, replace

```

