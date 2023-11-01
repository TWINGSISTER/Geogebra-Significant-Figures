# Geogebra-Significant-Figures
A collection of GGB tools to switch between a number and its textual representation preserving the number of significant figures.  More on defining tools can be found [here](https://primi235711.altervista.org/moodle/course/view.php?id=5#section-4)

To use them take the basic-empty-library.ggb  This file has all the tools and no construction. File empty-library.ggb  has all the tools and some basic construction. The file example-with-library.ggb contains an example using these primitives.

In GGB numbers receive an internal representation that do not care about significant figures. If you put in the inpput line in GGB either a=2.0 or a=2.00 you will obtain the same internal representation for the number a. 
It would be nice to have manipulation primitives for textual representation of 
numbers taking care of significant figures. 

The first step will be to have an input field (number in the example) that reads a text variable (numbertxt).

The set of GGB tools are created with the following inputs to the command line in GGB.
