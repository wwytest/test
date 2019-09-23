# QC for Methylation Data: Saliva Samples

## Lab Description:

The University of Minnesota Genomics Center (UMGC; Minneapolis, MN, USA) conducted epigenetic profiling in both saliva and blood samples from each individual using the custom-designed Infinium EPIC+ Array (Illumina, Inc., San Diego, CA, USA). 

## Measurement of Methylation Level - Short Protocol:

1. DNA extraction. Quantified the saliva samples using PicoGreen assay and then used UMGC QC1 & 2 assays to evaluate the quality. Excluded samples had < 25 ng DNA.

2. Bisulfite Conversion via Zymo’s EZ-96 DNA Methylation Kit.

3. MethylationEpic Processing followed Illumina’s protocol: for the hybridization and staining steps used a Tecan liquid handling robot, all other steps were performed manually.


## QC Procedures:

**1. Original data before cleaning up:**<br>

Before QC, saliva epigenetic data from 1171 partcipants were recorded.

**2. Beta distributions:**<br>

Definition of beta value:

<img src="https://latex.codecogs.com/svg.latex?\Large&space;\beta=\frac{M}{M+U+100}" />

M and U denotes the methylated and unmethylated signal intensities, respectively. The offset value 100 is added to M+U to stabilize beta values when both M and U are small. 

Definition of M Value: 

<img src="https://latex.codecogs.com/svg.latex?\Large&space;Mval=log_2\left(\frac{M}{U}\right)" />

Both Beta value and M value statistics have been used as metrics to measure methylation levels. By utilizing M value, one can easily impose a minimum threshold of difference to conduct differential methylation analysis. However, The Beta value has a more intuitive biological interpretation, which makes it more appropriate for reporting the results to investigators. (Pan Du et al., 2010) Here we only present evaluation of Beta value in our QC procedures. 

Raw Beta distribution for 1171 participants:

![salivabetadis1](https://github.com/LifeEGX/stp-study-data-catalog/blob/master/methylation/salivaqc/salivabetadis1.png "salivabetadis1")

###### _X-axis is beta value, while Y-axis indicates the density. High quality measurement of methylation level tends to have beta value closed to either 0 or 1. We found many participants have high/peak beta values within 0.1-0.4, which should be excluded._

Beta distribution after excluding 256 participants have beta values of many CpG sites locate around middle area (0.1-0.4) rather than boundaries of the interval (0,1):

![salivabetadis2](https://github.com/LifeEGX/stp-study-data-catalog/blob/master/methylation/salivaqc/salivabetadis2.png "salivabetadis2")

There were 915 participants left.

**3. Multi-Dimensional Scaling (MDS) plot:**<br>

MDS plots are based on principal components analysis (PCA) and are an unsupervised method for looking at the similarities and differences between the various samples. MDS plot illustrates a 2-d projection of distances between samples. Samples that are more similar to each other should cluster together, and samples that are very different should be further apart on the plot. (see https://dockflow.org/workflow/methylation-array-analysis/) Here we used principal component one and two, which captures the greatest two source of variation among STP-1 participants, to figure out those very different individuals.

MDS plot for 915 participants (no sex probes):

![saMDS](https://github.com/LifeEGX/stp-study-data-catalog/blob/master/methylation/salivaqc/saMDS.png "MDS")

###### _Multi-dimensional scaling plots illustrates the relationships between the participants in STP-1. X-axis and Y-axis indicates values of principal component one and two, respectively. We hypothized that all individuals should be similar to each other without considering of gender discrepancy, thus all plots should cluster together._

**4. Density-based spatial clustering of applications with noise (DBSCAN) plots:**<br>

DBSCAN is a density-based data clustering algorithm: given a set of points in some space, it groups together points that are closely packed together (points with many nearby neighbors), marking as outliers points that lie alone in low-density regions (whose nearest neighbors are too far away).

The DBSCAN algorithm basically requires 2 parameters:

A. eps: specifies the maximum distance between two samples for one to be considered as in the neighborhood of the other. It means that if the distance between two points is lower or equal to this value (eps), these points are considered neighbors. 

B. minPoints: the minimum number of points to form a dense region. For example, if we set the minPoints parameter as 5, then we need at least 5 points to form a dense region. 
 
We tested eps in range (5,16) to find a lowest eps value which can extinguish the cluster of majority participants. We found eps=11 met the requirements.

![DBSCAN](https://github.com/LifeEGX/stp-study-data-catalog/blob/master/methylation/salivaqc/DBSCAN.png "DBSCAN")

###### DBSCAN Plot for STP Saliva Methylation Data (no sex probes), based on a frame of MDS plot. Red and purple dots indicate two clusters extinguished by DBSCAN algorithm.

**4. Horvath 353 clock:**<br>

Dr. Steve Horvath discovered the first epigenetic aging clock in year 2014. The Horvath 353 clock can utilize Illumina DNA methylation array datasets to predict individual's methylation age. We adopted the Horvath 353 clock in STP-1 population and compared one's methylation age versus chronological age.

Horvath 353 clock age vs. chronological age in 1105 participants:

![horvath](https://github.com/LifeEGX/stp-study-data-catalog/blob/master/methylation/bloodqc/horvath.png "horvath")

###### _Scatterplot illustrates linear association between Horvath 353 clock age and chronological age. A better linearity indicates a better prediction of age by Horvath 353 clock._

**5. Gender test:**<br>

MDS plot for 1150 participants only used sex probes: 

![mdssex](https://github.com/LifeEGX/stp-study-data-catalog/blob/master/methylation/bloodqc/mdssex.png "mdssex")

###### _The two clusters illustrated gender discrepancy evaluated by sex probes. Several misclassifications were identified: 46 males and 23 females were misclassified as opposite genders, respectively._

Finally there were 1036 participants left.

**7. Final Horvath 353 clock age vs. chronological age (n=1036):**<br>

![horvath2](https://github.com/LifeEGX/stp-study-data-catalog/blob/master/methylation/bloodqc/horvath2.png "horvath2")

**6. Final beta distributions for STP blood methylation data (n=1036):**<br>

![bloodbetadis4](https://github.com/LifeEGX/stp-study-data-catalog/blob/master/methylation/bloodqc/bloodbetadis4.png "bloodbetadis4")

## Reference:

1. Du P, Zhang X, Huang C-C, et al. Comparison of Beta-value and M-value methods for quantifying methylation levels by microarray analysis. BMC Bioinformatics. 2010;11(1):587. doi:10.1186/1471-2105-11-587
