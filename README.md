
# SAS Data Analysis Project

## Introduction
This project involves the analysis of health data using SAS. The dataset includes variables related to demographics, health habits, and cholesterol levels.

## Data Preparation
The data is read in from a file named "sas13.dat" using SAS `infile` statement. Then, new variables are created based on existing variables to facilitate analysis.

```sas
/* Read in data */
data DataHealth;
infile "sas13.dat";
input ID 1-4
SEX 6
AGE1 8-9
MARITAL1 12
CIGARET1 13
CHOLESTEROL1 15-18
WTKG1 20-23
HTCM1 24-27
CHOLESTEROL2 29-32;
run;

/* Create new variables */
data DataHealthNew;
set DataHealth;

/* Variable creation code */
...

run;
```

## Descriptives & Frequencies
Descriptive statistics and frequency analysis are conducted to understand the distribution of variables.

```sas
/* Descriptives & frequencies */
proc means data=DataHealthNew maxdec=1; run;

proc freq data=DataHealthNew;
tables SEX CIGARET1 MARSTAT1 NEVMAR MARRIED WIDDIVSEP HICHOL1 HICHOL2;
run;
```

## Scatterplot Matrix & Boxplots
Scatterplot matrix and boxplots are created to visualize relationships between variables.

```sas
/* Scatterplot Matrix & Boxplots */
proc sgscatter data=DataHealthNew (keep=CHOLESTEROL1 AGE1 BMI1);
matrix CHOLESTEROL1 AGE1 BMI1 /diagonal=(histogram normal);
run;

proc sgplot data=DataHealthNew (keep=CHOLESTEROL1 SEX);
vbox CHOLESTEROL1 /category=SEX;
run;

proc sgplot data=DataHealthNew (keep=CHOLESTEROL1 MARSTAT1);
vbox CHOLESTEROL1 /category=MARSTAT1;
run;
```

## Statistical Tests
T-tests, contingency tables, correlation analysis, multiple linear regression, logistic regression, and comparison of means are performed to analyze relationships and make inferences.

```sas
/* T-test of CHOL1 between male and female */
proc ttest data=DataHealthNew (keep=CHOLESTEROL1 SEX) ci=equal;
class SEX;
var CHOLESTEROL1;
run;

/* Contingency Tables */
proc freq data=DataHealthNew (keep=AGEDECADE HICHOL1);
tables AGEDECADE * HICHOL1 / expected nocol chisq;
run;

/* Correlation, MLR, and Plots */
proc corr data=DataHealthNew (keep=CHOLESTEROL1 AGE1 BMI1) nomiss;
var CHOLESTEROL1 AGE1 BMI1;
run;

proc reg data=DataHealthNew;
model CHOLESTEROL1 = AGE1 FEMALE BMI1 MARRIED WIDDIVSEP;
run;

/* Compare the means for matched samples */
proc ttest data=DataHealthNew (keep=CHOLESTEROL1 CHOLESTEROL2);
paired CHOLESTEROL2 * CHOLESTEROL1;
run;

proc ttest data=DataHealthNew (keep=CHOLESTEROL1 CHOLESTEROL2 AGEDECADE);
where AGEDECADE = 5;
paired CHOLESTEROL2 * CHOLESTEROL1;
run;

/* Comparison of proportions for matched samples */
proc freq data=DataHealthNew (keep=HICHOL1 HICHOL2);
tables HICHOL1 * HICHOL2 /norow nocol agree;
run;

/* Logistic Regression */
proc logistic data=DataHealthNew;
class MARSTAT1 (ref='1') /param=reference;
model HICHOL1 (event='1') = FEMALE AGE1 BMI1 MARSTAT1;
run;
```

## Conclusion
The analysis provides insights into the relationship between various factors and cholesterol levels in the dataset.

