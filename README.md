# Geogebra-Significant-Figures
A collection of GGB tools to switch between a number and its textual representation preserving the number of significant figures.  More on defining tools can be found [here](https://primi235711.altervista.org/moodle/course/view.php?id=5#section-4)

To use them take the basic-empty-library.ggb  This file has all the tools and no construction. File empty-library.ggb  has all the tools and some basic construction. The file example-with-library.ggb contains an example using these primitives.

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

`If(FromBase(ntxt, 10) > 1, Take(intps(ntxt), 2) + mants(ntxt),`
>`Take(mants(ntxt), abs(log(10, FromBase(ntxt, 10))) - 1)`
`)`

The code for `snintps(ntxt)` could be:

`If(FromBase(ntxt, 10) == 0, "0",`
>`If(FromBase(intps(ntxt), 10) >=1, UnicodeToLetter(Element(TextToUnicode(intps(ntxt)), 1)),`
>>`UnicodeToLetter(Element(TextToUnicode(mants(ntxt)), 1 + abs(log(10, FromBase(ntxt, 10)))))`
`))` 