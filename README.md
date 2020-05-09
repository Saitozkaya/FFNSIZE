# FFNSIZE
VPA package in Excel for identification and quantification of finite fracture networks

'**************************************************

NAME OF CODE:			FFNSIZE

DEVELOPER:				Sait I. Ozkaya

email: 			     	saitozkaya@ gmail.com

mob: 			      	90 537 283 7243

mob               273 3 943 7071
                  
HARDWARE:			  	PC

SOFWARE:			  	WINDOWS, Microsoft Excel platform

PROG. LANGUAGE:		VBA

YEAR:				    	2020

TEMPLATE    			SFM, TRUNCATE, CONNECT, HULLX, AND HULLCOMP


****************************************************

DESCRIPTION

The main tool is an Excel macro package, FFNsize, to generate stochastic 2D fracture models; identify FFNs, and determine and tabulate attributes of all FFNs. A wide variety of approaches are utilized to generate DFN models. FFNsize uses the conventional method which is based on generation of random variables with selected probability distributions using Monte Carlo simulation for location, length and orientation  of each fracture The main modules of FFNsize are briefly are (i) Sfm: stochastic fracture modeling (SFM); (ii)Truncate: fracture truncation; (iii) Connect: Extraction of FFNs; (iv) Hullx: calculation of total fracture drainage surface and  area of convex hull around a FFN and (v) Hullcomp: tabulation of all  extracted FFN attributes. These tools are used in the same sequence as listed above. A SFM is created from subsurface fracture data as the first step. The second step is fracture truncation.  Truncated fractures are then input to Connect module, which extracts each and every FFN.  Extracted FFNs are transferred to Hullx module to calculate drainage area, convex hull volume and other related parameters, which are collected averaged and tabulated by the Hullcomp module. 


1 Stochastic Fracture Modeling (SFM)

This is the first module of the FFNsize toolkit which generate a 2D stochastic fracture model. The Sfm module can generate a single set or multiple sets of fractures. The coordinates of the center of a fracture, x and y, are calculated using uniform distribution within a cell.  The number of fractures within each cell is given by Poisson distribution. 
Strike varies between 0 and 180 degrees and hence it is treated simply as a scalar quantity with normal distribution. The “Norm.Inv” function of Excel is used to obtain a random strike with Normal distribution.  Truncated Power (Pareto) and Log normal probability distribution options are available for fracture length. Log.Inv function of Excel is used to obtain a random length with Log Normal distribution.


1.1 Parameters

Parameters that must be provided to the SFM module are (i) side length of cells, (ii) total number of cells in x direction and y directions and (iii) number of fractures sets.  Up to 5 sets can be modeled by FFNsize but only single and double sets are considered in this study.
For each fracture set the following parameters must be supplied (i) average fracture length, (ii) standard deviation of length, (iii) fracture length probability distribution option: log normal or truncated power distribution (iv) average fracture strike and (v) strike standard deviation. 
The average number of fractures per cell for Poisson distribution is equal to fracture areal density, P21 multiplied by the area of the cell. The area density is given by scanline fracture density, P11 divided by average fracture length, Lav).
Standard deviation and average of log normal distribution are function of given average length, Lav and standard deviation. For truncated Power distribution, a cut off must be specified such that fractures shorter than the cut off value are not considered. The cut off value and average fracture length determine the standard deviation of truncated Power distribution. The closer the cut off value to the average, the smaller is the standard deviation and vice-versa. It should be noted that the cut off value, must be larger than half of the average, Lav for a positive variance. Otherwise, there is no restriction on the lower cutoff value.


2 Truncation

Fracture truncation has a significant bearing on fracture connectivity. Therefore FFNsize provides a module; Truncate to simulate fracture truncation in nature.  Tensile fractures are commonly truncated by pre-existing open tensile fractures. Shear fractures may displace existing shear fractures that they intersect or merge with the existing fracture and restart its propagation. Long shear fractures have a tendency to stop short shear fractures or displace them.  
Conventional approach runs into difficulty in fracture truncation sequence because fractures are generated randomly with no time perspective. The Truncate module tries to circumvent the problem by adopting some conventions. If there are two sets of conductive fractures, the younger set gets truncated by the older set. If there is only one set of fractures which formed at the same geological episode, early formed fractures truncate later ones. If we assume older fractures are longer than younger ones, longer fractures should truncate shorter ones within the same set. This is accomplished by sorting fractures in each set according to length from longest to shortest. All fractures are trimmed by the first (longest) fracture and next by the second longest fracture etc.
The Truncate module accepts two parameters: (i) number of sets, n, in truncation sequence and an array of set numbers for n sets in hierarchical age order from oldest to youngest, (ii) probability of truncation.  Fractures are placed row by row with x and y coordinates of the two tips and set number. The older sets truncate the younger ones.  The youngest set is truncated by all. The set with smaller set number is regarded as the older set in the fracture data from SFM. Longer fractures within the same set truncate shorter fractures.

The probability of truncation is normally set to 1 for tensile fractures. For shear fractures it can be adjusted between 0 and 1. If the truncation probability is zero, no fracture truncation takes place. If probability is 1, one of the intersecting fractures gets truncated. Values in between decides whether a truncation is going to take place or not.  Values close to 1 increase chances of truncation.

Truncation alters average fracture length and fracture density. The Truncate module recalculates average fracture length, P21 and P11 for all fractures and for each fracture set. The module also calculates total number of fracture intersections, fracture area density and scan line density, modified fracture length after truncation. These modified values of density and length are taken into consideration in the statistical analysis. The Truncate module also calculates the number of fracture intersections per fracture, Nint, and fracture connectivity index, Eq. (1) For two sets of fractures the modified form of fracture connectivity for two sets are calculated. For two sets, the module calculates the modified length and fracture density for each set. 


3 Facture Connectivity

Fracture connectivity, Connect is the main module of FFNsize toolkit. It extracts all FFNs that exist within the SFM and have number of interconnected fractures larger than a specified value. The concept is simple. Imagine a large number of match sticks on a table. Those that are connected are glued together. In order to extract a particular FFN all one has to do is to hold one stick and lift it up. All the fractures that interconnected will be lifted as a bundle. This may be repeated several times until all bundles are identified.  The minimum number of fractures can be specified to the Connect routine. FFNs with number of fractures less than the minimum are skipped. In the current analyses, a minimum number of fractures is set to 5.


4 Convex Hull and Drainage area

This module, Hullx calculates the total length of fractures within a FFN and also the area of a convex hull enveloping the FFN. Calculation of total fracture length is straightforward. The convex hull is calculated following a modified form of Graham’s algorithm. The module also calculates matrix block size in x and y directions and fracture scan line density for the bundle. Input to Hullx is any one of the FFNs identified and listed by the Connect module. 


Tabulation of FFN attributes

The last module Hullcomp calls the Hullx module repeatedly for all extract FFNs by Connect module and extract attributes such as number of fractures and area of each FFN. The module also calculates the maximum and average values of these attributes, which constitute the basis for further statistical analysis.

