# PIRN
Parsimonious construction of phylogenetic networks from gene trees

Software accompaniment to
1. Yufeng Wu, "Close lower and upper bounds for the minimum reticulate network of multiple phylogenetic trees", in Proceedings of ISMB 2010, Bioinformatics 2010 26(12):i140-i148, 2010.  
2. Yufeng Wu, "An Algorithm for Constructing Parsimonious Hybridization Networks with Multiple Phylogenetic Trees", in Proceedings of RECOMB 2013. 

08/02/2020: Due to IT changes at UConn, PIRN is now moved to GitHub.

2/19/2014: A related program called PIRNS is now published. It is related to PIRN but often works faster for larger number of trees.

1/8/2013: v2.0.1: This is a new code release. The main new feature is the ability of constructing the exact most parsimonious hybridization network for multiple rooted binary trees. Note this works only for relatively small number of reticulations (specified by -r option). See the Readme file for more details.

PIRN is a program for reconstructing the most parsimonious phylogenetic networks that contain a set of given phylogenetic trees. One motivation is that the trees are the gene trees and the phylogenetic networks model what may have happen when there is horizontal gene transfer. An example of phylogenetic network produced by PIRN is shown above, which contains five input gene trees (see the Appendix of my ISMB paper for these five trees). The main functions of PIRN is computing a lower bound on the minimum reticulation needed and also reconstructing a network that is usually close to optimum.


Note: Files can be downloaded using "Save Link/Target As..." After downloading the softwares, you may need to change file access permissions (e.g. chmod u+x pirn). Source code is available upon request .

Current version: v. 2.0.1. I provide both executables on Linux and Mac. If you want to build from source code, please check out the distributed source code.
