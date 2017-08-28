 Distributed FW
===========================

It is the implementation of the distributed Frank-Wolfe algorithm. Please cite this if you intend to use this code for your research.

Usage
-----
An example execution is as follows:

	spark-submit --master spark://10.100.9.166:7077   --executor-memory 90g --driver-memory 10g /modularDistFW.py --optgam 1 --inputfile "input" 
	--outfile 'output'   --npartitions 100  --niterations 100  --sampmode 'No drop' --ptr 0.5   --remmode 0   &>log


This solves the D-Optimal Design problem. Its dataset is loaded from "input". Maximum number of iterations is 100. The level of parallelism is 100. The result will be stored in 'output'
 

Algorithm  Overview
------------------


This is a generic implementation of distributed FW. Curretnly it solves D-Optimal Design, A-Optimal Design, Convex Approximation, and Adaboost. In order to solve any problem you need to define the gradient function, common information function, and the common information adaptation function, as discussed in the paper. 

It reads the dataset from an input file. It is passed through inputfile in the command line to the code. 



Command-line arguments
----------------------
usage: modularDistFW.py [-h] [--optgam OPTGAM] [--inputfile INPUTFILE]
                        [--outfile OUTFILE] [--npartitions NPARTITIONS]
                        [--niterations NITERATIONS] [--beta BETA]
                        [--sampmode SAMPMODE] [--ptr PTR]
                        [--randseed RANDSEED] [--problem PROBLEM]
                        [--remmode REMMODE] [--remfiles REMFILES] [--K K]
                        [--inputP INPUTP] [--C C] [--verbose | --silent]

optional arguments:
  -h, --help            show this help message and exit
  --optgam OPTGAM       If this value is 1 the setp size is found from Eq (6),
                        otherwise it is set to a diminshing step size.
  --inputfile INPUTFILE
                        The directory which holds the input files in text
                        form.
  --outfile OUTFILE     The iutput file, which stires the objective value,
                        duality gap, and running time of the algorithm for
                        each iteration.
  --npartitions NPARTITIONS
                        Number of partitions
  --niterations NITERATIONS
                        Number of iterations
  --beta BETA           beta used in Smoothened FW.
  --sampmode SAMPMODE   It specifies the type of the algorithm. The options
                        are, No Drops, non smooth, smooth, and Lasso which run
                        parallel FW, Sampled FW, Smoothened FW, and parallel
                        FW for LASSO problem, respectively.
  --ptr PTR             Sampling ratio used in Sampled FW and Smoothened FW.
  --randseed RANDSEED   Random seed
  --problem PROBLEM     The type of the problem. Give DoptimalDist,
                        AoptimalDist, CVXapprox, or Adaboost, to solve
                        D-optimal Design, A-optimal Design, Convex
                        Approximation, or AdaBoost, respectively.
  --remmode REMMODE     If it is 0 then the algorithm starts from the
                        beginning. Otherwise it will continue the algorithm
                        from where the algorihtm stopped. It is helpful when
                        the job crashes.
  --remfiles REMFILES   The input file that keeps the RDD, It will continue
                        from this point. Use when remmode is 1.
  --K K                 The budget K for the l1 constraint. Use when sampmode
                        is LASSO.
  --inputP INPUTP       The vector P in Convex Approximation and the vector r
                        in Adaboost. Must be in .npy form.
  --C C                 The parametr C in Adaboost.
