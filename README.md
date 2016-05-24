# decompose-jacobians

These files contain code used for the paper *"Completely decomposable Jacobian varieties in new genera"*  by Paulhus and
Rojas.

Code
----

There are three functions/procedures in the main file "decompose.mag".<br>
* `GADecomposition(G,signature)`  Determines the decomposition of the Jacobian variety of curves with automorphism group G and given signature.<br>
* `ICDecomposition(G,signature,order)`  Determines the decomposition of the Jacobian variety of all intermediate covers of curves with automorphism group G and given signature by a subgroup of given order.<br>
* `PrettyPrint(decomposition)`  Prints the decompositions in a "human friendly" way.<br>




Setup
-----

To run the code, first load the main file "decompose.mag" into Magma. <br>
`load "decompose.mag";`

This will also load another file called "genvectors.mag", so make sure the file "genvectors.mag" is in the same folder on your computer.



Verification
------------

Suppose we want to verify examples as in Section 2.1. Take the group SmallGroup(324,69) with signature [0;2,6,18]. In
the paper we claim the corresponding genus 46 curve decomposes completely. To check this we run the following code.

`G:=SmallGroup(324,69);`<br>
`sign:=[0,2,6,18];`<br>
`Decomp:=GADecomposition(G,sign);`<br>
`PrettyPrint(Decomp);`<br>

This code outputs:<br>
`E x E x E^2 x E^6 x E^6 x E^6 x E^6 x E^6 x E^6 x E^6` <br>
`E x E x E^2 x E^6 x E^6 x E^6 x E^6 x E^6 x E^6 x E^6`


Next consider examples produced as in Section 2.2. Suppose we want to deterime the genus 34 example which is found from
an intermediate cover of a genus 73 curve with group SmallGroup(432,682) and signature [0;2,2,2,6] by a subgroup of
order 2. We again claim this decomposes completely. To verify this example, run the following.

`G:=SmallGroup(432,682);`<br>
`sign:=[0,2,2,2,6];`<br>
`order:=2;`<br>
`Decomp,subgps:=ICDecomposition(G,sign,order);`<br>
`PrettyPrint(Decomp);`<br>

This code outputs:<br>
`E x E^2 x E^2 x E^2 x E^2 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4`<br>
`E x E^2 x E^2 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4`<br>
`E x E^2 x E^2 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4`<br>
`E x E^2 x E^2 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4 x E^4`<br>
`E^2 x E x E x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2`<br>
`E x E x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2`<br>
`E x E x E x E x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2 x E^2`<br>

Notice that the function ICDecomposition outputs two values. The first is the decomposition, as a list of numbers
(details described in the next section).  The second is the list of positive integers which represent which subgroup H
of G produces the given decompositions. 

`subgps;`<br>
`[ 2, 3, 4, 5, 6, 7, 8 ]`

 This means in the example above, the second, third, and fourth decompositions give examples of genus 34.  If we look at the same positions in the variable `subgps` we see that this is from subgroups numbered 3, 4, and 5. (See the warning section below: we are converting all groups to permutation groups first so the ordering of subgroups is for the group represented that way.)



Interpretting output
--------------------

If you would prefer to have the  decompositions in a more "computer friendly" way  here is what the decompositions
look like before the PrettyPrint function is run on them. This is the first example above where the group is
SmallGroup(324,69) with signature [0;2,6,18]. 


`[`<br>
`[ [* 1, 1, 5 *], [* 1, 1, 10 *], [* 1, 2, 15 *], [* 1, 6, 20 *], [* 1, 6, 21 *], [* 1, 6, 22 *], [* 1, 6, 23 *], [* 1, 6, 24 *], [* 1, 6, 25 *], [* 1, 6, 26 *] ],`<br>
`[ [* 1, 1, 6 *], [* 1, 1, 10 *], [* 1, 2, 15 *], [* 1, 6, 20 *], [* 1, 6, 21 *], [* 1, 6, 22 *], [* 1, 6, 23 *], [* 1, 6, 24 *], [* 1, 6, 25 *], [* 1, 6, 26 *] ]`<br>
`]`<br>

This is a list of two elements, representing each of two decompositions. For each decomposition, the individual
factors (from (2) in the paper)  are coded as three numbers.  The first number is the dimension of the variety Bi, the second number is the exponent ni, the third number is which characters provide this factor (again, see the Warnings below).

You may wonder why, for a given group and signature, the code outputs more than one decomposition sometimes.  This is because the code used to deteremine the generating vectors (in the file `genvectors.mag`) is  

Warnings
--------

The program will return an error if a group not represented as a permutation group is entered as input and if the
internal code cannot convert the group to a permutation group. The user will be prompted  to enter it as a permutation group. 

All subgroups and conjugacy classes are labeled as Magma labels them for the permutation group (either as entered by the user or as computed from the function `ConvertToPerm` which is in the file `genvectors.mag`).

An error will also be returned if the specific group and signature pair entered are not possible pairs for a Riemann Surface.




