# Stata Recitation - Week 3 - Graphs
McCourt School of Public Policy, Georgetown University

## CONTENTS:
 - Graph Intro
 - Graph dialogue box
 - Graph commands
 - Graph editor window
 - Save/Export graphs


## GOALS:
 - Create graphs using graph dialogue window
 - Edit graphs using graph editor
 - Find graph examples in `graph intro` in help file and manual
 - Find examples for individual commands in manual
 - Save and export a graph




## Drawing Graphs
### Basic graphs from the twoway family 


* Example from Definition section of help graph_twoway:
```
help graph twoway
sysuse uslifeexp2
list in 1/8
twoway scatter le year
twoway line le year
twoway connected le year
twoway (scatter le year) (lfit le year)
```

* know how to create these graphs using graph dialogue box (keep record!)
      (from drop-down menu: `Graphics>Twoway Graph (Scatter Line, etc.)`)
  * Select `Create` to create a new graph:
      - `Basic plots > Scatter` for twoway scatter of selected Y and X variables.
      - `Basic plots > Line` for twoway line graph.
      - `Basic plots > Connected` for twoway datapoints and line graph.
      - `Fit plots > Linear prediction` for graph of line that best `fits` the data. Shows best guess for linear trend/relationship between Y and X variables.

  * Can overlay multiple graphs by creating more than one and then submitting 
    all at once from the dialogue window.
      - To change a graph, select it and then hit `edit` from the dialogue window.
      - Select a graph from the dialogue window and hit `disable` to have it not appear/run.
      - Use other tabs in the graph dialogue window to
          - Change to which observations are included (if/in)
          - The layout and title of the axes (Y axis/X axis)
          - Title, Legend, etc.
   * To clear all graphs and labels hit the reset button in the lower left of the graph dialogue window, small button with a `R` icon.
       

* Can keep adding options to graph dialogue box until you have a long command like this:
```
twoway (scatter le year) (lfit le year), ytitle(Life Expectancy (Years)) ///
title (Life Expectancy by Year) subtitle(with linear fit line) ///
legend(order(1 `Actual` 2 `Fitted`))
```
     * Note: When you change the `Legend` options in the graph dialogue window, 
         use the [?] icon to get help on how to specify labels 
          * Keep adding options and submitting, and note how the command changes
         
* When submitting graphs through the dialogue window, the commands are printed out in results window. 
   * You can copy these from results back into the command line & make tweaks to the graph 
   by changing the text until you get what you want and are familiar with 
   how to specify different options for the graph such as axes titles, etc.
      - Make sure you keep the ordering of your variable names consistent when
        entering commands that overlay multiple graphs as switching the order
        will screw up the  Y and X axes.

```
twoway (scatter y-var_name x-var_name) (lfit y-var_name x-var_name)
```
   

```
[INCORRECT]
twoway (scatter y-var_name x-var_name) (lfit x-var_name y-var_name)      
```

* When your command is complete, save the final version in a do-file.
* Commands can also be copied to clipboard using copy icon on graph dialogue box.



## Graph Editor 

* Another way to modify graphs, titles, etc., is with the graph editor
* Right click on graph and select `open graph editor`
* But, changes that you make here are not saved in a command.
* If the data changes, or you have to remake this graph, you will have to re-do any changes you made in the graph editor.
* Better to use options to get your graph as complete as possible, then make final changes using graph editor.



## Saving and Exporting Graphs 

* In graph window, can save graph using `File > Save`, save final graph as .gph file type.
      * The `.gph` file type is Stata specific and can be opened by the program and modified again later.
          * However, it is not compatible with other programs, so you will either have to copy and paste each graph one-by-one (right-click and select copy to copy it) into other programs or use the export
            function.

* In graph window, select, `file>Save as` .gph for future use or editing within Stata. Export for use with other programs using `File > Save as`, export final graph as file types: .png, .tif, .wmf, .pdf, .jpg, ...
* File type depends on how you will be using it.

* Saving and exporting graphs can also be done from the do-file

```
help graph export
graph export `...\\MyNewGraph.pdf`, as(png) replace
```

```
help graph save 
graph save Graph_name `G:...\\MyNewGraph.pdf`, as(tif) replace
```

### Exercise I 

- Use auto.dta from the Stata example datasets. 
```
sysuse auto.dta
```
- Create a scatterplot with mpg on the x axis and price on the y axis.
```
twoway scatter price mpg
```
- Impose a linear prediction fit line on the graph (along with the scatter plot).
```
twoway (scatter price mpg) (lfit price mpg)
```
- Title the y axis “Price in Dollars”
```
twoway (scatter price mpg) (lfit price mpg), ytitle(Price in Dollars)
```
Or use graph dialogue window.


## Other Types of Graphs

help graph intro
* Also see > Manual
* Show examples of other graphs from the manual entry
* This gives an idea of the types of graphs that are available
* You can find detailed information on specific graphs using `help graph graphtype` or by looking through the pdf manuals' bookmarks



### Bar Graphs
```
help graph bar
```
* Major distinction from `scatter` (from help file's `Description` section):
	- Scatter plots actual values of x and y variables
   - Bar displays calculated statistics of y-variable over x-variable categories.

* Bar graphs are conceptually similar to tabstat command that we used last week.
* From after class exercises last week:
```
sysuse census.dta
tabstat medage, by(region)
graph bar medage, over(region)
```
![bar plot](figures/3-bar.png)
* Other example bar graphs are in the `Remarks - Introduction` section of `help graph bar` file.

* `Remarks - Examples of Syntax` section of `help graph bar` file shows you how to enter the commands.

* Can also experiment using graph dialogue window: 
   (from the drop-down menu: `Graphics>Bar Chart`)



### Distributional Plots 
 * Single variable graphs showing its distribution



#### Histogram 

* Histogram is the most common distributional graph
```
help histogram
```

* example 1, from the `Remarks > Histograms of continuous variables` section of help file:
```
sysuse sp500
histogram volume
```
![histogram plot](figures/3-histogram.png)    
- Stata automatically determines the number of bars or `bins` approriate to show the distribution of your variable.

* example 2, from the `Remarks > Histograms of discrete variables` section of help file:
```
sysuse auto
histogram mpg, discrete
```
![histogram plot](figures/3-histogram-2.png)  

- the `, discrete` option forces stata to show each distinct value of the variable in a separate bar/bin. Useful for categorical variables or numeric variables that can only take on a certain, relatively limited number of values, i.e. years of education.

* Can also experiment using graph dialogue window: 
   (from the drop-down menu: `Graphics>Histogram`)


       
#### Kernel Density 
* A smoothed version of histogram
* Creates a line, rather than a series of rectangles
* Can be used as an option to histogram
```
sysuse sp500
histogram volume , kdensity
```
![kdensity](figures/3-kdensity.png)  

* Or as a stand-alone command
```
kdensity volume
```
![kdensity](figures/3-kdensity-2.png)  

### Diagnostic Plots 
```
help diagnostic plots
```
* Compares the distribution of a variable to some known distribution.
* Some quant professors will use these commands in class, but the 
* concepts have not been covered yet.
* Students should know how to find documentation on these commands, but do not 
* need to know how to interpret them. 
* Show extended examples in manual page.

![qnorm](figures/3-qnorm.png)

### Exercise II 

- Use auto.dta from the Stata example datasets.
```
sysuse auto
```
- Create a bar graph that shows the mean gear ratio for foreign and domestic cars.
```
graph bar gear_ratio, over(foreign)
```

- Change the color of the bars to your favorite color.[for black bars]
```
graph bar (mean) gear_ratio, over(foreign) bar(1, fcolor(black))
```

     Or (not recommanded): right-click on graph and select `start graph editor`
         Then right-click on a bar and select `Rectangle Properties` to change
           the color of both bars.
             Or right-click on a bar and select `Object Specific Properties` 
                to change the color of that specific bar.

- Create a graph of your choice using the `auto.dta`.
     - To see & compare the distribution of price for foreign and domestic cars:
```
histogram price, by (foreign)
```
Or use the graph dialogue windows accessible via the drop down menu.
       
