# StELa-Protein-Structure-Clustering-Algorithm
**S**econdary S**t**ructure **E**nsembles with Machine **L**e**a**rning

Published by: Amanda C. Macke - Dima Group @ University of Cincinnati
email for questions (mackeac@mail.uc.edu)

Authored by: Amanda C. Macke, Jacob E. Stump, Maria S. Kelly, Jamie Rowley, Vageesha Herath, Sarah Mullen & Ruxandra I. Dima
bioRxv - 
###################################################################################### 
## A Letter from StELa 
### Developed in the Dima Research Group 
by Amanda C. Macke, Jamie Rowley, Jacob E. Stump & Ruxandra I. Dima
### University of Cincinnati  
##### VERSION 2
######################################################################################

This python code was delevoped on a linux system but can be ran in any python environment provided there is an interactive terminal. In linux, the code is most easily executed from a terminal with the following command: 

> python3 StELa_v1.py 

######################################################################################

Programmed with Python 3.9 & access to python libraries included in Anaconda (numpy, pandas, matplotlib & Biopython) 

Dependencies Used: 

numpy == 1.22.4 

pandas == 1.4.4 

matplotlib == 3.5.2

scipy == 1.7.1 

######################################################################################
### General Description Algorithm Logic: 
This algorithm is a double clustering algorithm used to characterize local regions of proteins found to be particularly flexible in MD simulation. Similar to the CATS algorithm, developed by Cheung & group (https://github.com/Cheung-group/CATS), StELa uses the PHI/PSI backbone angles as a collective variable for describing each frame, extracted with the GROMACS **rama** function (https://manual.gromacs.org/current/onlinehelp/gmx-rama.html). StELa takes advantage of Ramachandran and centroid based clustering (KMeans) to create representative vectors for each frame with the centroid labels. These vectors are checked for biochemistry geometry rules such as an alpha helix must include 4 helical residues in a row and a beta strand must include at least 3 straight residues in a row. The representative vectors are then clustered with complete linkage heriarchical clustering. For this step, StELa provides the heirarchical dendrogram, the Calinski-Harabasz score plot and the Silhouette score plot to assist in identifying an appropriate number of clusters (types of structures). Representative structures are then determined based on a calculated probability table and a search for the most probable vector. The resulting information as well as the associated frame, and cluster population are then provided in the StELa_Clusters.txt output file.

Version 1 of this algorithm was used in the previous publication (https://doi.org/10.1021/acs.jpcb.2c05288)

In this study, StELa was used on MD data of the same protein in the presence and absence of binding cofactors to help identify a ligand dependent transition in an identified local region of the protein. This version only corrected vectors for appropriate alpha helical character.
######################################################################################
### Extract the PHI/PSI backbone angles with RAMA
>gmx rama -f $NAME.xtc -s ../$TPR.tpr -o rama-$NAME.xvg

- Be sure to remove the header of your output .xvg file before input into StELa

For a protein with 12 angles, the file is organized as Phi, Psi, Label for each residue pair for each frame

####-------- Frame 1 --------

-130.807  175.305  GLY-2

-100.138  138.382  LYSH-3

-77.6627  -20.8971  VAL-4

-140.123  115.601  GLN-5

-115.651  93.2969  ILE-6

-79.0894  -34.4587  ILE-7

-131.098  151.165  ASN-8

-66.8139  -25.5225  LYSH-9

-80.3444  -25.0075  LYSH-10

81.1117  37.0612  LEU-11

-128.915  146.13  ASP-12

-143.342  7.581  LEU-13

####-------- Frame 2 --------

-115.556  168.431  GLY-2

-142.016  -22.9915  LYSH-3

-129.29  167.479  VAL-4

-105.211  -8.87905  GLN-5

-80.1814  169.737  ILE-6

-61.2031  127.277  ILE-7

-142.025  108.322  ASN-8

-133.261  -7.2814  LYSH-9

-120.735  86.7835  LYSH-10

-113.063  -18.0431  LEU-11

-146.869  138.434  ASP-12

-102.121  -3.93136  LEU-13

####-------- Frame 1 --------

...

...

...

######################################################################################
### Input file (StELa_Input.csv): 
This input file is used to organize and streamline input provided by the user concerning the individual protein region of interest.
- System_Name: used to name some of the files - in our instance, the input file (indicated in line XXX)
   ^ Consideration may be required as to the name of your particular input file
- Residue_IDs: a list of the labels provided in the rama.xvg file for resiude organization - an arbitrary list may be provided instead if desired
- Total_Angles: the number of angles in your protein region (14 residues = 12 angles) - determines how StELa parses through the rama.xvg input file
- No_Traj: number of trajectories that may be concatenated - corresponds to the number of input files StELa should be looking for
- No_Frames: number of sampled frames - The more frames, the better.
