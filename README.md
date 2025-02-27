# GCIM
The genetic causality inference model(GCIM) is a statistical method for detecting the direction of causation in GxE interaction studies. 

- 
 Authors: Zinabu Fentaw, Dovini Jayasinghe, S.Hong Lee
-

NB: The proposed direction of causation refers to the causal directions of GxE interactions that is the primary focus of the researcher's interest, while the reverse direction of causation examines the opposite directions of GxE interactions to test its validity.
   
Package installation

~~~
library(devtools)
install_github("zinabuf/GCIM")
~~~

Load the library

~~~
library(GCIM)
~~~

Data preparation for input data files

The data preparation follows: All data files should be split into two files for discovery and target data including Genetic data, Outcome(phenotype data), exposure(environmental data), and confounder variables. 

Genetic data 

The genetic data should be in Plink binary format(.bed, .bim, and .fam). Then it should be split into the discovery dataset(ideally 80% of the data ) and the target dataset(the remaining 20% of the data). 

Outcome data, exposure or environmental variables, and confounder variables are also split in proportions similar to those above and are compatible with the Plink data format. 
    #outcome should contain three columns ( FID, IID, and phenotype Value) and the phenotype value for case-control data should be 1 for control and 2 for the case and in the discovery dataset, while in the target dataset, it should be 0 for controls and 1 for cases.
    #the exposure should contain at least 19 columns (FID, IID, and exposure values, confounder 1 confounder1, ...confounder16)
Depending on the type of outcome variables, the outcome, exposure, and confounder variable or other covariate data should be split into the discovery dataset(ideally 80% of the data ) and the target dataset(the remaining 20% of the data). 


A quick guide

GCIM analysis uses PLink2 to analyze discovery data, and the package is compatible with the Linux operating system. 
1. download the plink2 from the Plink website and specify the executable Plink file path.
   
~~~
plink_path <- "<plink_path>/plink2"
~~~

Set the proposed directions and the reverse direction based on the type of outcome variables. Depending on the type of outcome variable, whether binary or quantitative, step-by-step tests can be conducted as a one-time process for one-direction tests.
for instance, if you have a binary outcome with a quantitative exposure variable do the following steps.

**A. Proposed causal directions**

- Step 1: Conduct b_gweis
- Step 2: Conduct q_gwas
- Step 3: Compute prs_score
- Step 4: Compute a regression using gcim_b
  
**B. The reverse causal direction**

- Step 1: Conduct q_gweis
- Step 2: Conduct b_gwas
- Step 3: Compute prs_score
- Step 4: Compute a regression using gcim_q
Compare the statistical test results between the proposed and reverse causal directions and declare the correct directions.
Input data

All the input data should be prepared with the following data dimensions

**Start analysis**

**Performing GWEIS**

   ~~~
 a <- b_gweis(plink_path, dis_snp, bp_dis_phen, bp_dis_cov)
   ~~~

   ~~~
 b <- q_gweis(plink_path, dis_snp, qp_dis_phen, qp_dis_cov)
   ~~~

**Performing GWAS**

   ~~~
 c <- b_gwas(plink_path, dis_snp, bp_dis_cov)
   ~~~

   ~~~
 d <- q_gwas(plink_path, dis_snp, qp_dis_cov)
   ~~~

**Compute PRS**

   ~~~
 e <- prs_score(plink_path, tar_snp)
   ~~~

**Compute GCIM for the required directions**

   ~~~
f <- gcim_b(bp_tar_phen, bp_tar_cov, Add_PRS, Int_PRS, Cov_PRS, confounders)
   ~~~

   ~~~
g <- gcim_q(qp_tar_phen, qp_tar_cov, Add_PRS, Int_PRS, Cov_PRS, confounders)
   ~~~

$red[zinabu]
