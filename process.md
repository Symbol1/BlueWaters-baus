
# Process Overview

The entire process of computing the Tutte polynomial of `RM(2, 6)`
can be summarized as follows.

* Compute the RREF-signature polynomial of RM32 from scratch (hard).
* Compute the pivot-signature polynomial of RM64
    from the result of the last step (VERY DIFFICULT).
* Reduce the result of the last step to the Tutte polynomial (easy).

Before we dive into each bullet point,
let me clarify some terminologies and concepts.

## Reed--Muller codes

[Reed--Muller code](https://en.wikipedia.org/wiki/Reed%E2%80%93Muller_code)
is a series of linear block codes that possess
rich symmetric and recursive structures.
In the usual notation, `RM(0, 5)` is the repetition code;
`RM(1, 5)` consists of `RM(0, 5)` plus linear monomials (z₁, z₂, etc);
`RM(2, 5)` consists of `RM(1, 5)` plus quadratic monomials (z₁z₂, z₁z₃, etc).
And so on.

More figuratively, the recursive structure is a *Pascal's rule*.

```text
                                                RM(6, 6)
                                        RM(5, 5)
                                RM(4, 4)        RM(5, 6)
                        RM(3, 3)        RM(4, 5)
                RM(2, 2)        RM(3, 4)        RM(4, 6)
        RM(1, 1)        RM(2, 3)        RM(3, 5)
RM(0, 0)        RM(1, 2)        RM(2, 4)        RM(3, 6)
        RM(0, 1)        RM(1, 3)        RM(2, 5)
RM(-1,0)        RM(0, 2)        RM(1, 4)        RM(2, 6)
        RM(-1,1)        RM(0, 3)        RM(1, 5)
                RM(-1,2)        RM(0, 4)        RM(1, 6)
                        RM(-1,3)        RM(0, 5)
                                RM(-1,4)        RM(0, 6)
                                        RM(-1,5)
                                                RM(-1,6)
```

For any vertical pair, the upper `RM(r-1, m)` contains the lower `RM(r, m)`.
For any triangle

```text
RM( r , m)
          RM(r, m+1)
RM(r-1, m)
```

the direct sum

```text
⌈RM(r, m)⌉   ⌈RM(r-1, m)⌉
|        | ⊕ |          |
⌊RM(r, m)⌋   ⌊     0    ⌋
```

is the right one.
That is, `[1;1]` ⊗ `RM(r, m)` ⊕ `[1;0]` ⊗ `RM(r-1, m)` = `RM(r, m+1)`.

It is sometimes desired to focus on the *increments*:
`rm(r, m)` ⊕ `RM(r-1, m)` = `RM(r, m)`

```text
                                                rm(6, 6)
                                        rm(5, 5)
                                rm(4, 4)        rm(5, 6)
                        rm(3, 3)        rm(4, 5)
                rm(2, 2)        rm(3, 4)        rm(4, 6)
        rm(1, 1)        rm(2, 3)        rm(3, 5)
rm(0, 0)        rm(1, 2)        rm(2, 4)        rm(3, 6)
        rm(0, 1)        rm(1, 3)        rm(2, 5)
                rm(0, 2)        rm(1, 4)        rm(2, 6)
                        rm(0, 3)        rm(1, 5)
                                rm(0, 4)        rm(1, 6)
                                        rm(0, 5)
                                                rm(0, 6)
```

Then the dimension of `rm(r, m)` is m choose r.
And

```text
⌈rm(r, m)⌉   ⌈rm(r-1, m)⌉
|        | ⊕ |          |
⌊rm(r, m)⌋   ⌊     0    ⌋
```

is `rm(r, m+1)`.

## Pivot-signature polynomial

We invent this term as an invariant that captures more information than
[Tutte polynomial](https://en.wikipedia.org/wiki/Tutte_polynomial) does.

Fix a code length one is interested in, say n = 2^6 = 64.
Choose a generator matrix such that

* its first column generates `RM(0, 6)`,
* its first seven columns generate `RM(1, 6)`,
* ...
* its first 63 columns generates `RM(5, 6)`, and
* it generates `RM(6, 6)`.

In other words, we are augmenting the generator matrices
of `rm(0, 6)`, `rm(1, 6)`, ..., and `rm(6, 6)`.
Note that we are using thin and tall generator matrices
(and message vectors are multiplied from the right).
For the record, the augmented matrix looks like

```text
################################################################
#-#####--#-##-###-####---#--#-##--#-##-###----#---#--#-##-----#-
##-####-#-#-##-###-###--#--#-#-#-#-#-##-##---#---#--#-#-#----#--
#--####-----#--##--###---------#-----#--##--------------#-------
###-####--##-##-###-##-#--#--##-#--##-##-#--#---#--#--##----#---
#-#-###----#--#-#-#-##--------#-----#--#-#-------------#--------
##--###---#--#--##--##-------#-----#--#--#------------#---------
#---###---------#---##-------------------#----------------------
####-#####---###-###-##---###---###---###--#---#---###-----#----
#-##-##--#----##--##-#------#-----#----##------------#----------
##-#-##-#----#-#-#-#-#-----#-----#----#-#-----------#-----------
#--#-##--------#---#-#------------------#-----------------------
###--###-----##--##--#----#-----#-----##-----------#------------
#-#--##-------#---#--#-----------------#------------------------
##---##------#---#---#----------------#-------------------------
#----##--------------#------------------------------------------
#####-#######----####-####------######----#----####-------#-----
#-###-#--#-##-----###----#--------#-##------------#-------------
##-##-#-#-#-#----#-##---#--------#-#-#-----------#--------------
#--##-#-----#------##----------------#--------------------------
###-#-##--##-----##-#--#--------#--##-----------#---------------
#-#-#-#----#------#-#---------------#---------------------------
##--#-#---#------#--#--------------#----------------------------
#---#-#-------------#-------------------------------------------
####--####-------###--#---------###------------#----------------
#-##--#--#--------##--------------#-----------------------------
##-#--#-#--------#-#-------------#------------------------------
#--#--#------------#--------------------------------------------
###---##---------##-------------#-------------------------------
#-#---#-----------#---------------------------------------------
##----#----------#----------------------------------------------
#-----#---------------------------------------------------------
######-##########-----##########----------#####----------#------
#-####---#-##-###--------#--#-##--------------#-----------------
##-###--#-#-##-##-------#--#-#-#-------------#------------------
#--###------#--##--------------#--------------------------------
###-##-#--##-##-#------#--#--##-------------#-------------------
#-#-##-----#--#-#-------------#---------------------------------
##--##----#--#--#------------#----------------------------------
#---##----------#-----------------------------------------------
####-#-###---###------#---###--------------#--------------------
#-##-#---#----##------------#-----------------------------------
##-#-#--#----#-#-----------#------------------------------------
#--#-#---------#------------------------------------------------
###--#-#-----##-----------#-------------------------------------
#-#--#--------#-------------------------------------------------
##---#-------#--------------------------------------------------
#----#----------------------------------------------------------
#####--######---------####----------------#---------------------
#-###----#-##------------#--------------------------------------
##-##---#-#-#-----------#---------------------------------------
#--##-------#---------------------------------------------------
###-#--#--##-----------#----------------------------------------
#-#-#------#----------------------------------------------------
##--#-----#-----------------------------------------------------
#---#-----------------------------------------------------------
####---###------------#-----------------------------------------
#-##-----#------------------------------------------------------
##-#----#-------------------------------------------------------
#--#------------------------------------------------------------
###----#--------------------------------------------------------
#-#-------------------------------------------------------------
##--------------------------------------------------------------
#---------------------------------------------------------------
```

Let `AccessPatterns` be the power set of the rows of the generator matrix.
That is, an element `A` of `AccessPatterns` is a subset of rows.
`A` is treated as a standalone matrix; compute its RREF.
Read off the positions of the pivots, and call it the *pivot-pattern* of `A`.
Define the *pivot-signature polynomial*
of the generator matrix as the collection of all pivots-patterns.
More precisely,

```python
sum(
    product(
        z_p for p in PivotPattern(A)
    )
    for A in AccessPatterns
)
```

where z₁, z₂, ... are variables.

This polynomial encodes the answer to the question,
How many pivots lies in the `rm(r, 6)` region for each r?
And how does the pattern of the pivots behave statistically?

Note that there is an easy way to represent. pivot-patterns.
Simply write a binary number `11100101000.....` to represent the case
where the first three columns have pivots, but not the next two,
yet the next is a pivot, followed by a non-pivot, et seq.
To shorten the representation, use hexadecimal;
`1110` becomes `E`, `0101` becomes `5`, `000.` becomes `0` or `1`, et seq.

Therefore, the pivot-signature polynomial can be encoded by a text file
with many lines, each line corresponding to a monomial (i.e., a pivot-pattern)
and the associated coefficient (i.e., the multiplicity).
For example, a file may contain these lines:

```text
fff0ff0001400000         26dd8000
fff0ff0001800000         b43f0000
fff0ff0001c00000          e5ec000
fff0ff0002800000         a0c2c000
fff0ff0002c00000         161f4000
...
```

Signature-patterns are printed with `%016x` and multiplicities with `%16x`.
No need to prefix `0x` as we later read the file by `%x`.

## RREF-signature polynomial

One could go one step further and ask, in addition to pivot-patterns,
How does the RREF itself behave?
This is not necessary a wise question to ask because the RREFs are all distinct;
there is no *statistics* if all samples are unique in their own.

However, there is a relief---instead of the full RREF,
we are interested in RREFs in certain blocks.
For instance, consider a pivot-pattern `11100101000.....`,
It may come from an RREF like this:

```text
 1 0 0 a d 0 g 0 k p u . . . . .
   1 0 b e 0 h 0 l q v . . . . .
     1 c f 0 i 0 m r w . . . . .
           1 j 0 n s x . . . . .
               1 o t y . . . . .
                       . . . . .
                             . .
                               .
```

Instead of the full RREF, we are interested in
the *pivotal blocks* w.r.t. the block decomposition `1 + 4 + 6 + 4 + 1`.
The figure explains it better.

```text
[1]
  ⌈1 0 b e⌉
  ⌊  1 c f⌋
          ⌈1 j 0 n s x⌉
          ⌊    1 o t y⌋
                      ⌈. . . .⌉
                      ⌊      .⌋
                              [.]
```

Everything outside the blocks are dropped.

What, you ask, is all of these about?
It turns out that, if we define the *RREF-signature polynomial* properly,
the RREF-patterns of RM32 will determine the pivot-patterns of RM64.
Notationally,

```python
PivotSign(RM64) = Simplify( RREFSign(RM32)^2 )
```

The magic is behind the squaring operation.

## Squaring the RREF

One might ask, Why the pivot-patterns of RM64
is ever related to the RREF-patterns of RM32?
Essentially, this is because
RM64 is related to RM32 via the recursive structure.

In fact, we see RREF-patterns as whatever that is (necessary and) sufficient
to understand the pivot-patterns of the next level.
We obtain a proper definition of RREF-patterns by
reverse-engineering the recursive structure of Reed--Muller codes.

Here is a imaginative demonstration of how squaring works.
We take two RREF-patterns from RM16:

```text
[A]
  ⌈B B B B⌉
  ⌊    B B⌋
          ⌈C C C C C C⌉
          |    C C C C|
          ⌊        C C⌋
                      ⌈D D D D⌉
                      ⌊    D D⌋
                              [E]
```

and

```text
[F]
  ⌈G G G G⌉
  |  G G G|
  ⌊      G⌋
          ⌈H H H H H H⌉
          |  H H H H H|
          ⌊          H⌋
                      [  I I I]
                              [ ]
```

Now apply the recursive rule:

```text
⌈A  ⌉
⌊F F⌋
    ⌈B B B B        ⌉
    |    B B        |
    |G G G G G G G G|
    |  G G G   G G G|
    ⌊      G       G⌋
                    ⌈C C C C C C            ⌉
                    |    C C C C            |
                    |        C C            |
                    |H H H H H H H H H H H H|
                    |  H H H H H   H H H H H|
                    ⌊          H           H⌋
                                             ⌈D D D D        ⌉
                                             |    D D        |
                                             ⌊  I I I   I I I⌋
                                                             [E  ]
```

Now take RREF and read off pivots of RM32.
For the actual computation, we take two RREF-patterns from RM32
to obtain a pivot-pattern of RM64.

Note that `RREFSign(RM32)` does not determine `RREFSign(RM64)`;
this method is not inductive.
As a result, if we want to understand `PivotSign(RM128)`,
we will have to build the understanding of `RREFSign(RM64)` from scratch.

## Scale and parallelism

The RREF-signature polynomial of RM32 is easy.
We simply

* enumerate all 2^32 subsets,
* compute RREF,
* crop the RREF-pattern according to the block decomposition
    `1 + 5 + 10 + 10 + 5 + 1`,
* write the resulting RREF-signature polynomial to a text file.

This can be done on my laptop or using only one Blue Waters node.
(Need openmp but not MPI.)

The result is a polynomial with 17,818,745 ≈ 2^24 terms.
Squaring this polynomial is not a joke,
because it means we have to do ≈ 2^48 multiplications.

To remediate, we divide the 1.7m-term polynomial into 311 *sub*-polynomials.
We then compute the products of all pairs of these 311 *sub*-polynomials.
There are 48,516 pairs/products to be computed,
each product costs a Blue Water node 3 minutes.
Hence the entire job cost ≈ 2,000 node-hours.
(Both openmp and MPI are used in this step.)

## Implementation details

Computation wise, see [RREF source code](rm34rref) for
how to compute RM32's RREF-signature polynomial.
See [squaring source code](rm71square) for
how to compute RM64's pivot-signature polynomial given the former.

Storage wise, see [data format](format.md)
for more on how we store RREF-patterns.
For details concerning folder and file names on Blue Waters,
see [directories on BW](directory.md).

We encourage you to [challenge the computation](challenge.md).
