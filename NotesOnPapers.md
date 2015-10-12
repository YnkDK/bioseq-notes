# Question 1: Pairwise alignment – gap costs
- Computing the optimal score of a pairwise alignment.
- Computing an optimal pairwise alignment in quadratic space.
- Handling different types of gap costs (general, affine).

### A General Method Applicable to the Search for Similarities in the Amino Acid Sequence  of Two Proteins
##### Saul B. Needleman and Christian D. Wunsch
Direct comparison of two sequences, based on the presence in both of corresponding amino acids in an identical array, is insufficient to establish the full genetic relationships between the two proteins. Allowance for gaps greatly muliplies the number of comparisons that can be made but introduces unnecessary and partial comparisons.

All comparisons can be represented by pathways through an matrix. All diagonal paths indicates no gap, while a horizontal or vertical indicates a gap in the comparion between two amino acids.

The simpelst cost function is binary, i.e. a 1 is assigned if two amino acids match at a given index, and 0 otherwise. The more sophisticated comparison is to assign each cell a value from a function of the composition of the proteins, the genetic code triplets representing amino acids, the neighboring cells in the matrix or any theory concerned with the significance of a pair of amino acids.

__The gap cost__: A penalty factor, a numberor direction of the gap. No gap would be allowed in the operation unless the benefit from allowing the gap would exeed the barrier. The maximum-match pathway then, is the pathway for which the sum of the assigned cell values is largest.

![Global Alignment Recursion](http://cs.au.dk/~mys/bioseq/global-alignment-recursion.png "Global Alignment Recursion")

__Backtracking__: Staring at _(n,m)_ one looks at the three cells nearest, i.e. _(i+1, i+1)_, _(i+1, j)_ and _(i, j+1)_, and determines which of those cells produced the value in the current cell. This is done recursivly until _(0,0)_ is reached.

### The String-to-String Correction Problem
##### Robert A. Wagner and Michael J. Fischer
This paper defines a general notation of ''distance'' between two strings and presents an algorithm for computing the distance in time proportional to the product of the lengths of the strings. The operations considered are:
1. Changing one character to antoher single character
2. Deleting one character from the given string
3. Inserting one character into the given string



__The cost function__: Let _g_ be an arbitrary cost function which assigns to each edit operation _a -> b_ a nonnegative real number _g(a -> b)_. Extend _g_ to a sequence of edit opertaions _S = s1,s2,...,sm_ by letting $$$g(S) = \sum_{i-1}^m(s_i),$$$

if _m = 0_ then _g(S) = 0_. Note that the cost functions which depend on the particular characters affected by an edit operation might be useful in spelling corrections, where for example because of the conventional keyboard arrangement it may be far more likely that a character ''A'' be mistyped as an ''S'' than as a ''Y''.

TODO: Read the rest of the paper

### An improved Algorithm for Matching Biological Sequences
##### Osamu Gotoh
Introduces a new algorithm which allows muliple-sized gaps and runs in essentially _MN_ steps if the gap weight has a special form of _w(k) = uk + v_, with _u_ and _v_ larger than or equal to 0.

![Global Alignment Recursion](http://cs.au.dk/~mys/bioseq/affine.png "Global Alignment Recursion Affine Cost")


### Convex gap weights
##### D. Gusfield
TODO: Read?

# Question 2: Pairwise alignment – space consumption
- Computing the optimal score of a pairwise alignment.
- Computing an optimal alignment (with linear gap cost) in linear space.

###  2.6 Linear space alignments
##### R. Durbin, S. Eddy, A. Krogh and G. Mitchison
There are techniques that give the optimal alignment in limited memory, of order _n+m_ rather than _nm_, with no more than doubling in time. If only the maximum score is needed, the problem is (apparently?) simple. Since the recurrence only depends on entries one row back, we can throw away rows of the matrix that are further than one back from the current point. However, when doing this we also loose the ''direct'' backtracing.

To backtrack we use the divide and conquer principle. Define a cell _(u,v)_ where _u_ is the the middle column and _v_ is the row where the optimal alignment crosses the _i=u_ column of the matrix. Then we can first solve for the matrix in the _(0,0)_ to _(u,v)_ part using the standard method (either recurse down to a single cell or down to an appropriate size) and then solve for _(u,v)_ to _(n,m)_. The optimal alignment is then the concatenation of the optimal alignments for these two seperate submatrices.

>So how do wi find _v_? For _i > u_ let us define _c(i,j)_ such that _(u, c(i,j))_ is on the optimal path from _(1,1)_ to _(i,j)_. We can update _c(i,j)_ as we calculate _F(i,j)_. If _(i',j')_ is the preceding cell to _(i,j)_ from which _F(i,j)_ is derived, then set _c(i,j) = j'_ if _i=u_, else _c(i,j) = c(i',j')_. Clearly this is a local operation, for which we only need to maintain the previous row of _c()_, just as we only maintain the previous row of _F()_. We can now read out from the final cell of the matrix the value we desire: _v = c(n,m)_

Running time: _O(nm)_ and space _O(m)_

![Global Alignment Recursion](http://cs.au.dk/~mys/bioseq/Tij.png "Traceback algorithm")

### A Linear Space Algoirthm for Computing Maximal Common Subsequences
##### D.S. Hirschberg
TODO: Read and understand

### Indification of Common Molecular Subsequences
#####T. F. Smith, M. S. Waterman
TODO: Read and understand

# Question 3: Multiple alignment – exact score
- Computing an optimal multiple alignment using SP score.
- Speeding-up the computation of an optimal multiple alignment using SP score

###Multiple String Comparison - The Holy Grail
#####D. Gusfield

TODO:

###CLUSTAL W: improving the sensitivity of progressive multiple sequence alignment through sequence weighting, position specific gap penalties and weight matrix choice
#####J. D. Thompson, D.G. Higgins and T.J. Gibson.

TODO:

###The Four-Russians speedup
##### D. Gusfield

TODO:

# Question 4: Multiple alignment – approximate score
- Computing an optimal multiple alignment using SP score.
- Approximating an optimal multiple alignment using SP score.