# Geogebra-Significant-Figures
A collection of GGB tools to switch between a number and its textual representation preserving the number of significant figures.  More on defining tools can be found [here](https://primi235711.altervista.org/moodle/course/view.php?id=5#section-4)

To use them take the `example-with-library.ggb`.  This file has all the tools and no construction. The package deals only with POSITIVE numbers. Numbers like -1.0  or 0.0 are not allowed.

In GGB numbers receive an internal representation that do not care about significant figures. If you put in the input line in GGB either a=2.0 or a=2.00 you will obtain the same internal representation for the number a. 
It would be nice to have manipulation primitives for textual representation of 
numbers taking care of significant figures. 

In the following we first forget about the sign and talk about positive numbers. Later on we will add the sign.
There are two types of representations for a Finite decimal number: the standard representation and the exponential or scientific notation.  
In the standard representation there are two positive numbers: the  integer part (intp) and a mantissa (mant) or decimal part. They are separated by a dot or a comma. Here we use the dot. In the scientific notation there is a finite decimal number called the coefficient, represented in the standard notation plus an exponent magnitude that is a signed integer number. the coefficient part is always strictly greather than one and strictly smaller than ten. Therefore, equivalently we can say that the integer part of the coefficient is always a natural number made up of a  non single nonzero digit i.e. 1 to 9.   

Having said this we can add that the number of significant figures (nsf) in a number representation is the number of digits in its scientific notation, so 
2 and 2.0 are two different representations of the natural number two. If these are the outcome  of an experiment  they convey a different amount of information and we cannot say that they are equal.

The first step will be to have an input field (number in the example) that reads a text variable (numbertxt).

The set of GGB tools are created with the following inputs to the command line in GGB.

`ntxt` is the textual string used for the floating point number.

`nsf(ntxt)` is the number of significant figures in `ntxt` and it is given as: 

`If(FromBase(ntxt,10) > 1`

>`Length(Remove(TextToUnicode(ntxt),{32, 32, 32, 32, 32, 32, 32, 46, 44})),`

>`Length(Remove(TextToUnicode(ntxt),{32, 32, 32, 32, 32, 32, 32, 46, 44}))+floor(log(10,FromBase(ntxt,10))))`

`intps(ntxt)` is the string giving the integer part `intp(ntxt)` of the number whose textual standard representation is given by the string  `ntxt` and it is given as: `floor(FromBase(ntxt, 10))+""` being  `intp(ntxt)` given as: `floor(FromBase(ntxt, 10))` 

Similarly the mantissa `mants(ntxt)` is the string for the textual representation of `mant(ntxt)` (given as `FromBase(ntxt, 10) - floor(FromBase(ntxt, 10))`) .

In conclusion `ns(ntxt)` given as `""+intps(ntxt)+"."+mants(ntxt)+""` is the string representation of the number given by the string `ntxt` and is always equal to `ntxt` where leading zeros in the integer part are deleted. So `ns("0600.001")` is `"600.001"`

The functions to obtain the textual representation of the elements of the scientific notation of a number whose standard notation is `ntxt` are prefixed by  `sn` and therfore we will have  `snintps(ntxt)` `snmants(ntxt)`. The exponent is given, as a number, by the function `snexp(ntxt)`.

The code for `snexp(ntxt)` could be:
`If(FromBase(ntxt, 10) == 0, 0, floor(log(10, FromBase(ntxt, 10))))`

The code for `snmants(ntxt)` could be:

`If[FromBase[ntxt, 10] ≥ 1,`
>`If[FromBase[ntxt, 10] ≥ 10, Take[intps, 2] + mants, mants],` 
>>`If[Length[TextToUnicode[mants]] ≥ 2, Take[mants, floor(abs(log(10, FromBase[ntxt, 10]))) + 2], ""]]`

The code for `snintps(ntxt)` could be:

`If(FromBase(ntxt, 10) == 0, "0",`
>`If(FromBase(intps(ntxt), 10) >=1, UnicodeToLetter(Element(TextToUnicode(intps(ntxt)), 1)),`
>>`UnicodeToLetter(Element(TextToUnicode(mants(ntxt)), 1 + abs(log(10, FromBase(ntxt, 10)))))`
`))` 

Another Tool is `FromBaseSci` that takes a string and returns a number. The string could be a number in decimal (e.g. "-11.23") or scientific notation (e.g. "-1.123E1"). 
The tool with other two tools that are building blocks (FromBaseDec and FromBaseSgn) and an example can be found in file `example-with-library-FromBase-text.ggb`

Finally the tool `sfround(ntxt,sf)` takes a string `ntxt` that denotes a number in STANDARD NOTATION (not EXPONENTIAL NOTATION) and returns a list `{Coeff,Exp}` for its scientific notation rounded to `sf` digits.  We assume that sf is smaller or equal to  the number of significant figures in ntxt. If not trailing zeros are added but this is not correct. 
This tool follows these steps: first the scientific notation is calculated with sncs and snexp.  Then, sncs is rounded to sf digits. This is done via the `round` built in function that operates on numbers.  So 0.999 rounded to 2 significant digits gives the number 1 that is the same as 1.0. So the result of round is converted back into a string and a string of zeros is added (0000 or 0.000 actually doing what is needed). From this restored representation a string of sf significant digits is extracted. 
Note that we are working on numbers like n.dd or   or n.d  or n. with non zero n, so a string of length 2 never shows. 
This number is passed thru sn tool so that results like 9.99 rounded to 2 digits (i.e. 10.) is normalized to 1.0E1. The exponent rising from this last step is added to the original one to yeld a correct exponent.   