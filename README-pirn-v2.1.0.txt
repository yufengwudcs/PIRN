PIRN: Tool to reconstructing the minimum hybridization networks for multiple
phylogenetic trees

Yufeng Wu

Version 2.1.0
Released at October 19, 2022 

**************************************************************************
* WHAT IS PIRN?
**************************************************************************

PIRN was first developed in my paper:

Yufeng Wu, "Close Lower and Upper Bounds for the Minimum Reticulate Network 
of Multiple Phylogenetic Trees", in Proceedings of ISMB 2010, 2010.

For the version 2.0.1, a new module of PIRN is added, which can find the
exact minimum hybridization networks for multiple trees. It is in this paper:

Yufeng Wu, "An Algorithm for Constructing Parsimonious Hybridization Networks 
with Multiple Phylogenetic Trees", in Proceedings of RECOMB 2013, 2013.


First, where does the name PIRN come from? It stands for Parsimonious
Inference of Reticulation Network. We also call such network hybridization networks. 

The objective of PIRN is, given a set of correlated phylogenetic
trees (say each for some genes), reconstruct the minimum hybridization network
that combines these trees and is the simplest among all possible networks.

For background on hybridization networks, refer to the following:

[*] D. Morrison, Introduction to Phylogenetic Networks 
(http://www.rjr-productions.org/Networks/), 2011.

[*] D. Huson, et al., Phylogenetic Networks: Concepts, 
Algorithms and Applications, 2011.

Something you need to know before using the program: the trees are ROOTED 
and need to be binary. The running time varies. I have been able to solve
three or more trees with 30 leaves or more for some data. If you have better ILP
solver (like CPLEX), that will speedup the program greatly. To use
CPLEX, you need to rebuild the program with CPLEX option (after
making changes to Makefile for the path of the CPLEX library). 


**************************************************************************
What is new in PIRN v.2.1.0?

This version has improved version mainly for lower bound computation for hybridization networks.

Previously, lower bound is computed as part of the overall computation. In this version, user can choose to only compute the lower bounds. Moreover, user can choose what kind of lower bound to compute:
* General lower bound: this is the bound apply for any kind of hybridizaiton networks. This lower bound method was published in Wu (ISMB 2010).
* Tree-child lower bund: this bound is only applicable to tree-child networks. That is, the bound provides an estimate on the minimum number of reticulations for tree-child networks displaying the given trees (in the input file). This method is not yet published.

To learn how to use these new features, try the following:
(i)   ./pirn        				This gives a list of command line options.
(ii)  ./pirn -lb example-trees.txt          	This computes both the general and the tree-child lower bounds (and stop).
(iii) ./pirn -lbc example-trees.txt		Only compute the general lower bound
(iv)  ./pirn -lbu example-trees.txt		Only compute the tree-child lower bound

We are working on a publication related to the lower bounds. Check back for information on the proper citations for the lower bound computation.

**************************************************************************
HOW IT WORKS?
**************************************************************************

PIRN implements novel ways in estimating the minimum hybridization
networks. The key approach is to compute a lower and an upper bound
for the needed reticulation number (i.e. the minimum number of
reticulation events needed). In the best situation, when the
lower and upper bounds match, we already solve the general optimization
problem. Even when the bounds do not match, the bounds also
give information on the range of the optimal solutions.

The technical details of the bounds are presented in my ISMB 2010 paper.

**************************************************************************
* WHAT IS NEW IN PIRN v2.0.1?
**************************************************************************

Released on 12/28/2012, PIRN v2.0.1 now allows construction of the exact
most parsimonious hybridization networks. Well, this is a very hard problem
in general. Nonetheless, if your input trees are not too complex, PIRN v2.0
may be able to obtain optimal networks. Here are main options for v2.0.

-C: turn on the exact network search. That is, ./pirn-linux -C <trees.trees>.
-r <k>: you need to tell PIRN how much it should look. k = maximum number of
        reticulation events allowed. For example, ./pirn-linux -C -r 4 <trees.trees>
        will only search for networks using at most 4 reticulations.
-n <m>: sometimes you may want to run v2.0 but find it is too slow. In this case,
        you can try to turn on heuristic mode by issuing commands like:
        ./pirn-linux -C -r 8 -n 10000 <trees.trees>. This option allows PIRN v2.0
        to limit the search to the level specified by <m>: the larger m is, the
        more accuarate but also slower PIRN v2.0 will be.
-lb<u/c>: compute the lower bounds (see above).

In my experience, PIRN v2.1.0 usually can produce optimal networks with 4 (or even
5) reticulation events for medium sized data (say 30 leaves and 5 trees).
For lower bound computation, the GLPK-version can compute the lower bound for moderate
sized data. If the data is large (say 50 trees with 50 taxa), the tree-child lower bound
can be computed using more powerful solver (e.g., CPLEX or Gurobi). 

Note that if you invoke -n option, PIRN will run in the heuristic mode. 
That is, it will no longer ensure to find the best networks. But for the sake of
computational efficiency, you may need to use this option for more complex data.
My experience shows that PIRN works well even with this option in practice.

Also, the version 2.1.0 also outputs the found networks in the extended
newick format (see the book by D. Huson, et al. for an introduction to this format)

**************************************************************************
HOW TO PREPARE INPUT, RUN AND GET THE RESULTS??
**************************************************************************

The trees file contains multiple lines, one for each tree.
The trees should be in the popular Newick format.
A simple example is as follows:

(0,(3,(2,1)))
(2,(3,(1,0)))
(3,(2,(1,0)))

PREPROCESSING
One thing to note is that PIRN assumes only integer
leaves starting either from 0 or 1. Note: the integer
labels need to be consecutive. Also PIRN currently 
will re-label the leaves if labelst start from 1 to
start from 0 (that is, each leaf label is reduced by one).
Then, PIRN will attach an outgroup label as n, where
n is the number of original taxa. That is, labels
0 to n-1 are for the original n taxa, and the leaf
labeled with n is the unique outgroup taxa.
Then, after the above change, PIRN will first 
attempt to reduce the size of trees by shrinking the
identical subtrees common in all the trees.
Thus the tree solved by PIRN may be different from
the original trees. The output graphical network
(in GML format) is for the trees after the preprocessing.
We will fix this later by outputting the original
taxa in our next release.

The basic operation this program supports is to simply run:
./pirn-linux  <trees-file-name>

This basic operational mode is faster but may not be very accurate.
Also, different run of the basic mode (called coarse mode) can
give you different results (due to some stochastic choices).
To obtain better results, use:
./pirn-linux  -a <trees-file-name>
This is called full mode, which is deterministic: different
runs of full-mode give the same result. But the downside is that
this mode can be a little slower, which gets more severe
when the number of trees increases.

The output contains the computed lower and upper bounds. 
Also, the best reconstructed reticulate network is outputted
in a file called rnbest.gml. 
Since PIRN performs some simple preprocessing, the input trees
after these preprocessing steps are also output (in rtree?.gml,
where ? is from 0 t K-1).

For v2.0, a file called HybridizationNetwork.gml is output.
You can use the following viewer to visualize it.
Alternatively, the extended Newick format of the network 
is also dumped to the terminal.

**************************************************************************
GRAPH VIEWING      
**************************************************************************

The GML file generated by the program can be viewed using
a software of your choice.  We recommend VGJ (Visualizing
Graphs with Java), which can be downloaded free of charge 
from the following web page:

http://www.eng.auburn.edu/department/cse/research/graph_drawing/graph_drawing.html
                        
1) Go to the main VGJ directory. (default: graph_drawing)
2) Start VGJ. (java EDU.auburn.VGJ.VGJ)
3) Click on "Start a Graph Window" to bring up a
   viewing window.
4) On the menu bar, choose  "File" and then "Open (GML)."
5) When you first open a GML file generated by PIRN
   everything looks clustered at a point.
   To expand the graph, choose
   "Algorithms" --> "Tree" --> "Tree Down"
6) Although step (4) makes the graph viewable, you
   might still wish to rearrange certain things.  For
   example, some directed edges might point upward
   instead of downward.  To rearrange the graph
   manually, click on "Select Nodes" under "Mouse
   Action," located to the left of the graph.  Using
   the mouse, try rearranging the nodes to your liking. 

**************************************************************************
Integer Linear Programming     
**************************************************************************
This program has built-in GNU GLPK package.
As stated before, if you want to solve your problem for larger data,
you probably need a commercial ILP solver like CPLEX.
The current version with GLPK solver is mainly a demo of functionality.

Moreover, it has been reported that sometimes the pre-compiled executable
is not very stable: it crashes before finishing. My experience is that
this can happen when data size is large, or it can also be due to some
system incompatibility in say Linux. If you experience such situations,
I would recommend two things: (1) Compile PRIN from the source code. 
The source code along with instructions can be found in the web link of PIRN.
(2) Consider using CPLEX. CPLEX version is much more stable and can handle
larger data. To use the CPLEX version, you need to (a) have CPLEX licenses
(CPLEX is not free: it is a commerical product by IBM), and (b) compile
from the source code. 

* Gurobi
Gurobi is a popular ILP solver. We haven't implemented interface for Gurobi yet. 
At present, to use Gurobi, you can take the ILP formulation file and manually run
with Gurobi. PIRN outputs the ILP formulation for the tree-child lower bound before starting solve it. 
The file name of the ILP file is: <tree-file-name>.tmp.treechild.LB.ilp.
Check out the output message when running PIRN to verify this file has been computed.
At present, only the (stronger version of) tree-child lower bound ILP formulation is output.

**************************************************************************
BUILD FROM SOURCE
**************************************************************************
To build PIRN v2.1.0 from source, do the following:

(i) Download the source file (in .zip format)

(ii) Decompress the source file:  PIRN-v2.1.0-Oct192022.zip

(iii) Go to the source folder:
cd PIRN-v2.1.0-Oct192022

(iv) Extract GLPK:
tar -xf glpk-4.34.tar

(v) Build GLPK first
cd glpk-4.34
make

(vi) Go back to PIRN's folder:
cd ../..
make

(vii) Verify prin can run:
./pirn

You should see pirn runs and output the command line options. If not, check to see what steps have errors.

**************************************************************************
CONTACT      
**************************************************************************
Please send bug reports and technical questions to Yufeng Wu at 
<yufeng.wu@uconn.edu>.

