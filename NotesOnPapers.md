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


# Question 2: Pairwise alignment – space consumption
- Computing the optimal score of a pairwise alignment.
- Computing an optimal alignment (with linear gap cost) in linear space.

# Question 3: Multiple alignment – exact score
- Computing an optimal multiple alignment using SP score.
- Speeding-up the computation of an optimal multiple alignment using SP score

# Question 4: Multiple alignment – approximate score
- Computing an optimal multiple alignment using SP score.
- Approximating an optimal multiple alignment using SP score.