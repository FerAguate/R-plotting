# R-plotting
###Here let's include some plots examples and functions with small explanations
Use function 'windows()' to create a new window where the plots will be displayed.
To adjust margins use:
```R
par(mar=c(5,5,2,2)) #clock-wise starting from bottom margin
```
To split screen in cells (like a matrix):
```R
par(mfrow=c(1,2)) #One row & two columns
```
To plot on top of another plot:
```R
plot(...) #First plot
par(new=T)
plot(...) #This plots on top of the first
```
How rainbow() works? Let's see:
```R
w = 50
plot(0, 0, xlim = c(0, 100), ylim = c(0, w), ylab = 'Number of colors')

for(j in 1:w){
  for(i in 1:j){
    rect(((100/j)*i)-(100/j), j-1, (100/j)*i, j, col = rainbow(j)[i], border = NA)
    }
  }
```
Creates some data to play with:
```R
X = matrix(runif(32, 0, 340), nrow = 16, 2)
X = data.frame(X)
X$sex = as.factor(c(rep(1:3, each = 5), 3))
levels(X$sex) = c('Male', 'Female', 'Unknown')
```
Useful parameters on plot function are:
xlab = labels for x axis
ylab = labels for y axis
cex = Size for inside the plot
cex.lab = Size for labels
cex.axis = Size for axis
pch = Type of points
col = Colors for points
xaxt = Draw x axis? no = 'n', yes by default
yaxt = Draw y axis? no = 'n', yes by default 
xlim = Limits for x axis
ylim = Limits for y axis

For generate colors (they could have transparency and saturation) use:
```R
2 #Any number would be read as a color
'red' #Some key words are also colors
rgb()
rainbow()
terrain.colors()
heat.colors()
topo.colors()
cm.colors()
```
Lets do a simple plot of the data:
```R
ColorsPlot = X$sex
levels(ColorsPlot) = rainbow(5)[c(1,3,5)]
ColorsPlot = as.character(ColorsPlot)
plot(x = X$X1, y = X$X2, xlab = 'x', ylab = 'y', cex.lab = 2, cex.axis = 2, cex = 2, pch = 19, col = ColorsPlot, xaxt = 's', xlim = c(0, 340), ylim = c(0, 340))
```
Functions to do on top of a plot are (use function help() for more details):
```R
axis() #Adds an axis. It could be use to change options in an axis after using xaxt = 'n' or adding a right-side axis.  
points() #Adds points on a plot.
lines() #Adds lines on a plot. Useful to draw lines.
abline() #Adds an horizontal or vertical line in a plot.
legend() #Adds the legend.
rect() #Draw a rectangle.
polygon() #Draw a polygon. Hard to use, but very useful function to plot confident interval on lines.
draw.circle() #Draw a circle. Needs package 'plotrix'
```
Other functions to do plots:
```R
barplot()
image()
heat.map()
pie()
```

