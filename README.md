# PIRN
Parsimonious construction of phylogenetic networks from gene trees

Software accompaniment to
1. Yufeng Wu, "Close lower and upper bounds for the minimum reticulate network of multiple phylogenetic trees", in Proceedings of ISMB 2010, Bioinformatics 2010 26(12):i140-i148, 2010.  
2. Yufeng Wu, "An Algorithm for Constructing Parsimonious Hybridization Networks with Multiple Phylogenetic Trees", in Proceedings of RECOMB 2013. 
3. Sajad Mirzaei and Yufeng Wu, " Fast Construction of Parsimonious Hybridization Networks for Multiple Phylogenetic Trees", IEEE/ACM Transaction on Computational Biology and Bioinformatics, 2016. This is the PIRNs paper.


08/02/2020: Due to IT changes at UConn, PIRN is now moved to GitHub.

2/19/2014: A related program called PIRNS is now published. It is related to PIRN but often works faster for larger number of trees.
PIRNS is a program for reconstructing the most parsimonious phylogenetic networks that contain a set of given phylogenetic trees. Its goal is similar to the program PIRN. PIRNS is written in Java by Sajad Mirzaei. 
PIRNS java executable: to run it, first download to your own machine. To run it, type: java -jar pirns.jar <input gene trees>. The gene tree file should contain only gene tree file in the Newick format. It is the same data format as PIRN. The output network is stored in a file called output.gml. It is in the GML format. See the PIRN's readme for advice on how to view it. 


1/8/2013: v2.0.1: This is a new code release. The main new feature is the ability of constructing the exact most parsimonious hybridization network for multiple rooted binary trees. Note this works only for relatively small number of reticulations (specified by -r option). See the Readme file for more details.

PIRN is a program for reconstructing the most parsimonious phylogenetic networks that contain a set of given phylogenetic trees. One motivation is that the trees are the gene trees and the phylogenetic networks model what may have happen when there is horizontal gene transfer. An example of phylogenetic network produced by PIRN is shown above, which contains five input gene trees (see the Appendix of my ISMB paper for these five trees). The main functions of PIRN is computing a lower bound on the minimum reticulation needed and also reconstructing a network that is usually close to optimum.


Note: Files can be downloaded using "Save Link/Target As..." After downloading the softwares, you may need to change file access permissions (e.g. chmod u+x pirn). Source code is available upon request .

Current version: v. 2.0.1. I provide both executables on Linux and Mac. If you want to build from source code, please check out the distributed source code.

#Build from source code
First, download the source code and de-compress into a proper directory.
PIRN uses integer linear programming (ILP) solvers. You have two options.

By default (if you just type "make"), PIRN will build with GNU GLPK.
In this case, you need to get GLPK from here. I used version 4.34.
So select v.4.34 and download the file. Now follow the installation guide of GLPK
to build GLPK on your machine. After it is done (by typing ./configure and build I believe),
look for the build GLPK callable library (for v.4.34, it is called libglpk.a, and is under
glpk-4.34/src/.libs. Now copy this file (under src directory) along with the include directory
into a directory under the soure code directory of PIRN. That is, suppose the PIRN's main
code directory is pirn. Then, you need to put GLPK library (or create a symboikc link)
pirn/glpk-4.34 (with libglpk.a under pirn/glpk-4.34/src,
and header files under pirn/glpk-4.34/include). Look at the Makefile to see how GLPK is used.
Then simply type make.

If you have CPLEX, you can also compile with CPLEX. To compile with CPLEX,
you will need to modify the path settings in the Makefile to point to the directory
where CPLEX is installed in your machine. Check the Makefile to see where CPLEX
is referenced (near the begining). Then, type "make CPLEX".
