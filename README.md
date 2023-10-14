# Repartition of items in different groups

This tutorial is mostly based on [this guide](https://cran.r-project.org/web/packages/hamlet/vignettes/introduction.pdf)

## Installing the require packages and softwares

Theses steps should only be executed the first time you'll use this tutorial, no need to repeat them afterward.

You'll need to install first the `R programming language` and `RStudio`. To do this, simply follow the instructions provided on the [posit website](https://posit.co/download/rstudio-desktop/). 

**If you are on Mac**, you may need to check you CPU architecture to select the good binary for the `R programming language`. You can follow [this guide](https://help.arcstudiopro.com/all-how-tos/how-do-i-know-if-my-mac-has-intel-processor-or-apple-m1) to learn how to do this. Then select the binary corresponding to your computer architecture.

After that, in `RStudio`, you'll have to install the `Hamlet` package. To do so, just type the following command in the RStudio terminal (window at bottom left in the application):

```R
install.packages("hamlet")
```

## Importing the packages

Then, you'll have to import the packages to be able to use the commands defined in it. Just use the following command:

```R
library("hamlet")
```

## Opening a file (Excel/CSV)

First, if working with an excel file, convert your excel file in the csv format using `File> Save As > CSV`

Then, we'll open this file in `RStudio`. Just use the command below:

```R
ex <- read.table(file="PATH_FILE", sep=";", dec=".", stringsAsFactors=F, header=T)
```

In this command, you'll have to replace  `PATH_FILE` by the path of the csv you want to open in `RStudio`. To help you, you can use the TAB key to explore your files. Also replace `SEP` by the caracters used to delimitate the columns (should be generally set to `";"`) and `DEC` by the character used to separate the decimal part of number (generally set to `"."`)

## Calculating the distance matrix

The computations we'll use to create the repartition will need a distance matrix as input, so let's compute it directly. To do so, just use these commands:

```R
d <- dist(ex, method="euclidian")
d <- as.matrix(d)
```

You can change the type of distance used by changing the value of the method argument (see [the documentation](https://www.rdocumentation.org/packages/stats/versions/3.6.2/topics/dist) to know the different distances supported)

## Repartition of of the mouses into groups

To create a repartition, use the following commands:

```R
sol <- match.bb(d, g=2)
submatches <- paste("Submatch_", LETTERS[1:25][sol$solution], sep="")
names(submatches) <- names(sol$solution)
```

Change the value of `g` to be the number of mouses per group. For example, `g=3` will put 3 mouses per group.

## Adding randomization

We then randomize some of the results using the following commands:

```R
ex[,"Submatch"] <- submatches
set.seed(1) # for reproducibility
ex[,"AllocatedGroups"] <- match.allocate(ex[,"Submatch"])
```

## Saving the results on disk

Finally, we'll write those results in a file, to be able to have a consistent copy of those. To do so, just use:

```R
write.csv(ex, file="PATH")
```

and replace `PATH` by the path to the file you want to save. For example, `PATH` = `"Desktop/out.csv"` will save the results in a file named out.csv, on the desktop.

This file will contain two new columns, `Submatch` and `AllocatedGroups`. The first will contain the groups without adding randomization, and the second is the result of group formation after randomization.

## Plotting a heatmap
Finally, if you want to plot an heatmap of the distance matrix, first do the steps described up to section [distance](#calculating-the-distance-matrix), and then simply use the following command:

```R
heatmap(d)
```
