## PCA biplot split by a factor of two levels

### Description

returns a biplot of a prcomp object with colors for two levels. Plot the density of points, which represents the data well when the number of rows is large.

### Usage

mybiplot(prcomp(x), color_vector, ...)

### Arguments

	x                  matrix of dimension n x p.
	                 
	prs                an object of class prcomp.
	                 
	color_vector       factor of length n, with two levels (or a character string of length n with two categories).
	                 
	color_main         title of the scale that splits the data in two.
	                 
	colors             character class of length 2 indicating colors for each level.
	                 
	scale              number to scale the rotation matrix.
	                 
	manyfeatures       logical value indicating whether grey dots should be plotted for each variable instead of the traditional arrows.
                 
	pcs      	   numeric vector indicating the two principal components to be plotted. By default the first and second component will be selected.


### Details

This function was design to create a biplot for contrasting two categories. The projections of the rows into the principal components are plotted as densities to better handle large numbers. Unless 'manyfeatures = FALSE', the variable loadings are protted as grey dots to better handle large number of variables. Density plots were also added for better contrasting.
If plotting in a for loop, wrap the function within a print function as print(mybiplot(...)).

### Examples
```R
require(ggplot2)
require(ggnewscale)
require(cowplot)

tstm <- matrix(rnorm(1000), 200, 50)
colnames(tstm) <- paste0('V', 1:5)
print(mybiplot(prs = prcomp(tstm), as.factor(rep(c('no', 'yes'), 100)), manyfeatures = T, pcs = c(1, 2)))
```
