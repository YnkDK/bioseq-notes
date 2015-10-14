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
In the context of molecular biology, multiple string comparison (of DNA, RNA, or pretein strings) is the most critical cutting-edge tool for _extracting and representing_ biologically important commonalities from a set of strings. These (faint) commonalities may reveal evolutionary history. Because many important commonalities are faint or widely dispersed, they might not be apparent when comparing a set of related strings.

>**Definition (global multiple-alignment):** A global multiple alignment of _k > 2_ strings _S = {S1, S2, ..., Sk}_ is a natural generalization of alignment for two strings. Chosen spaces are inserted into (or at either end of) each of the _k_ strings so that the resulting strings have the same length, defined to be _l_. Then the strings are arrayed in _k_ rows of _l_ columns each, so that each character and space of each string is in a unique column

>**Fact:** Evolutionarily and functionally related molecular string can _differ significantly_ throughout much of the string and yet perserve the same three-dimensional structure(s), or the same two-dimensional substructure(s), or the same active sites, or the same or related dispersed residues.

Two strings specifying the ''same'' protein in different species may be so different that the few observed similarities may just be due to chance. When doing two-string comparison to find critical common patterns, the challenge is to pick species whose level of divergence is ''most informative'' (a vague and difficult task), this is not the case for multiple string comparison. Often, biologically important patterns that connot be revealed by comparison of two strings alone become clear when many related strings are simultaneously compared.

> **Definition (local multiple-alignment):** Given a set of _k > 2_ strings _S = {S1, S2, ..., Sk}_, a _local_ multiple alignment of _S_ is obtained by selecting one substring _Si'_ from each string _Si in S_ and then globally align those substrings.

> **Definition (induced pairwise alignment):** Given a multiple alignment _M_, the _induced pairwise alignment_ of two strings _Si_ and _Sj_ is obtained from _M_ by removing all rows except the two rows for _Si_ and _Sj_. That is, the induced alignment is the multiple alignment _M_ restricted to _Si_ and _Sj_. Any two opposing spaces in that induced alignment can be removed if desired.

> **Defintion (score):** The _score_ of an induced pairwise alignment is determined using any chosen scoring scheme for two-string alignment in the standard manner.

> **Definition (Sum of Paris):** The _sum of pairs (SP)_ score of a muliple alignment _M_ is the sum of the scores of pairwise global alignments induced by _M_

> ** The SP alignment problem:** Compute a global multiple alignment _M_ with minimum sum-of-pairs score.

The exact _SP_ alignment problem has been proved to be NP-complete and to sole it via dynamic programming for _k_ strings of length _n_ it takes _theta(n^k)_ and hence only practical for a small number of strings.

![Global Alignment Recursion](http://cs.au.dk/~mys/bioseq/sp-exact.png "SP exact 3-MSA")


**SPEEDUP FOR THE EXACT SOLUTION**
In the alternative approach of forward dynamic programming, when _D(i,j,k)_ is set, _D(i,j,k)_ is then sent forward to the seven (at most) cells whose _D_ value can be influenced by cell _(i,j,k)_. 
Another way to say this is to view optimal alignment as a shortest path problem in the weighted edit graph corresponding to a multiple alignment table. In that view, when the shortest path from the source _s_ (cell _(0,0,0)_) to a node _v_ (cell _(i,k,k)_) has been computed, the best-yet distance from _s_ to any out-neighbor of _v_ is then updated. In more detail, let _D(v)_ be the shortest distance from _s_ to _v_, let _(v,w)_ be a directed edge of weight _p(v,w)_, and let _p(w)_ be the shortest distance yet found from _s_ to _w_. After _D(v)_ is computed, _p(w)_ is immediately updated to be _min[p(w), D(v)+p(v,w)]_. Moreover, the true shortest distance from _s_ to _w_, _D(w)_, must be _p(w)_ after _p(w)_ has been updated from every node _v_ having an edge pointing into _w_. The forward dynamic programming implementation will keep a queue of nodes (cells) whose final _D_ value has not yet been set. The algorithm will set the _D_ value of the node _v_ at the head of the queue and remove that node. When it does, it updates _p(w)_ for each out-neighbor of _v_, and if _w_ is not yet in the queue, it adds _w_ to the end of the queue. It is easy to verify that when a node comes to the head of the queue, all the nodes that point to it in the graph have already been removed from the queue.
For aligning three strings of _n_ characters each, the edit graph has roughly _n^3_ nodes and _7n^3_ edges. The Carillo and Lipman speedup excludes some nodes before the main dynamic programming computation is begun, and forwards and backwards dynamic programming is equally convenient.
> **Definition:** Ket _d1,2(i,j)_ be the edit distance between suffixes _S1[i..n]_ and _S2[j..n]_ of strings _S1_ and _S2_. Define _d1,3(i,j)_ and _d2,3(j,k)_ analogously.

These _d_ values can be computed in _O(n^2)_ time by reversing the strings and computing three pairwise distances. Also, the shortest path from node _(i,j,k)_ to node _(n,n,n)_ in the edit graph for _S1_, _S2_, _S3_ must have distance at least _d1,2(i,j) + d1,3(i,k) + d2,3(j,k)_.

Now suppose that some multiple alignment of _S1, S2,_ and _S3_ is known (perhaps from a bounded error heuristic) and that the alignemnt has _SP_ score of _z_. If _D(i,j,k) + d1,2(i,j) + d1,3(i,k) + d2,3(j,j)_ is greater than _z_, then node _(i,j,k)_ cannot be on any optimal path and so _D(i,j,k)_ need not be sent forward to any cell.

If no initial _z_ value is known, then the heuristic can be implemented as an A* heuristic so tha the most ''promising'' alignments are computed first.

Running time for fast exact MSA algorithm: _O(''number of cells visited'' x ''time for an operation on Q'')_
Space: _O(k^2n^2 + ''maximum size of Q'')_

![Speedup exact](http://cs.au.dk/~mys/bioseq/exact-speedup.png "Speedup exact")

Time and space consumption depends on the quality of the upper bound on OPT and the lower bounds on alognemtns of suffixes. In order to reduce space consumption, you might want to first run the algorithm where you do not store the graph. This gives an optimal score OPT and then rerun the algorithm with this score. This should minimize the part of the alignment graph you visit, i.e. reduce space consumption.

**A bounded-error approximation method for _SP_ alignment**

![Speedup exact](http://cs.au.dk/~mys/bioseq/approx.png "Speedup exact")

###CLUSTAL W: improving the sensitivity of progressive multiple sequence alignment through sequence weighting, position specific gap penalties and weight matrix choice
#####J. D. Thompson, D.G. Higgins and T.J. Gibson.

TODO:

###The Four-Russians speedup
##### D. Gusfield

TODO:

# Question 4: Multiple alignment – approximate score
- Computing an optimal multiple alignment using SP score.
- Approximating an optimal multiple alignment using SP score.