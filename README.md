# PIRN
Parsimonious construction of phylogenetic networks from gene trees

Software accompaniment to
1. Yufeng Wu, "Close lower and upper bounds for the minimum reticulate network of multiple phylogenetic trees", in Proceedings of ISMB 2010, Bioinformatics 2010 26(12):i140-i148, 2010.  
2. Yufeng Wu, "An Algorithm for Constructing Parsimonious Hybridization Networks with Multiple Phylogenetic Trees", in Proceedings of RECOMB 2013. 
3. Sajad Mirzaei and Yufeng Wu, " Fast Construction of Parsimonious Hybridization Networks for Multiple Phylogenetic Trees", IEEE/ACM Transaction on Computational Biology and Bioinformatics, 2016. This is the PIRNs paper.


08/02/2020: Due to IT changes at UConn, PIRN is now moved to GitHub.

# PIRNs
2/19/2014: A related program called PIRNs is now published. It is related to PIRN but often works faster for larger number of trees.
PIRNs is a program for reconstructing the most parsimonious phylogenetic networks that contain a set of given phylogenetic trees. Its goal is similar to the program PIRN. PIRNs is written in Java by Sajad Mirzaei. My tests show that PIRNs often gives phylogenetic networks with close to the minimum number of reticulations.
PIRNs java executable: to run it, first download pirns.jar (included in this repositary) to your own machine. To run it, type: java -jar pirns.jar <input gene trees>. The gene tree file should contain only gene tree file in the Newick format. It is the same data format as PIRN. Note: only use taxon names in the Newick format; for example: ((a,b),(c,d)); Don't include other information such as branch lengths. The output network is stored in a file called output.txt. This file is in the GML format (a graph representation format). See the PIRN's readme for advice on how to visualize it. Or you can easily draw the network based on the simple graph format (nodes and edges) yourself.

10/19/2022: v2.1.0. The main feature added is enhancing lower bound computation on the minimum reticulation. PIRN can now compute two kinds of lower bounds: general bound (using the approach in Wu, ISMB 2010) and tree-child lower bound (which is only applicable to tree-child networks). The tree-child bound is more efficient to compute than the general bound.

1/8/2013: v2.0.1: This is a new code release. The main new feature is the ability of constructing the exact most parsimonious hybridization network for multiple rooted binary trees. Note this works only for relatively small number of reticulations (specified by -r option). See the Readme file for more details.

PIRN is a program for reconstructing the most parsimonious phylogenetic networks that contain a set of given phylogenetic trees. One motivation is that the trees are the gene trees and the phylogenetic networks model what may have happen when there is horizontal gene transfer. An example of phylogenetic network produced by PIRN is shown above, which contains five input gene trees (see the Appendix of my ISMB paper for these five trees). The main functions of PIRN is computing a lower bound on the minimum reticulation needed and also reconstructing a network that is usually close to optimum.


Note: Files can be downloaded using "Save Link/Target As..." After downloading the softwares, you may need to change file access permissions (e.g. chmod u+x pirn). Source code is available upon request .

Current version: v. 2.1.0. Only source code is provided. You need to build PIRN from source code (see below).

# Build from source code
First, download the source code and de-compress into a proper directory.
PIRN uses integer linear programming (ILP) solvers. You have two options.

By default (if you just type "make"), PIRN will build with GNU GLPK.
For convenience, I included the source code of GLPK version 4.34. Here are the steps to build PIRN based on GLPK. 

(i) Download the source file of PIRN (PIRN-v2.1.0-Oct192022.zip)

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


If you have CPLEX, you can also compile with CPLEX. To compile with CPLEX,
you will need to modify the path settings in the Makefile to point to the directory
where CPLEX is installed in your machine. Check the Makefile to see where CPLEX
is referenced (near the begining). Then, type "make CPLEX".
