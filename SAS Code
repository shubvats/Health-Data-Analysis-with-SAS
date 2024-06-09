Read in data;
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
proc contents varnum; run;
/* 2. Create new variables */
data DataHealthNew;
set DataHealth;
**create FEMALE;
if SEX = 2 then FEMALE = 1;
else if SEX = 1 then FEMALE = 0;
*create CIG1;
if CIGARET1 = 1 then CIG1 = 1;
else if CIGARET1 in (0,2) then CIG1 = 0;
*create MARSTAT1;
if MARITAL1 = 2 then MARSTAT1 = 1;
else if MARITAL1 = 1 then MARSTAT1 = 2;
else if MARITAL1 in (3,4,5) then MARSTAT1 = 3;
*create NEVMAR, MARRIED, WIDDIVSEP;
if MARITAL1 = 2 then do;
NEVMAR = 1; MARRIED = 0; WIDDIVSEP = 0; end;
else if MARITAL1 = 1 then do;
NEVMAR = 0; MARRIED = 1; WIDDIVSEP = 0; end;
else if MARITAL1 in (3,4,5) then do;
NEVMAR = 0; MARRIED = 0; WIDDIVSEP = 1; end;
*create AGEDECADE;
if AGE1 in (20:29) then AGEDECADE = 1;
else if AGE1 in (30:39) then AGEDECADE = 2;
else if AGE1 in (40:49) then AGEDECADE = 3;
else if AGE1 in (50:59) then AGEDECADE = 4;
else if AGE1 in (60:69) then AGEDECADE = 5;
*create BMI1;
BMI1 = WTKG1 / (HTCM1/100)**2;
*create HICHOL1;
if CHOLESTEROL1 > 240 then HICHOL1 = 1;
else if CHOLESTEROL1 in (0:240) then HICHOL1 = 0;
create HICHOL2;
if CHOLESTEROL2 > 240 then HICHOL2 = 1;
else if CHOLESTEROL2 in (0:240) then HICHOL2 = 0;
run;
/ 3. Descriptives & frequencies */
*a) Descriptive Statistics;
proc means data=DataHealthNew maxdec=1; run;
b) Frequency analysis;
proc freq data=DataHealthNew;
tables SEX CIGARET1 MARSTAT1 NEVMAR MARRIED
WIDDIVSEP HICHOL1 HICHOL2;
run;
/ 4. Scatterplot Matrix & Boxplots */
*a) scatterplot matrix for CHOLESTEROL1, AGE1, and BMI1;
proc sgscatter data=DataHealthNew
(keep=CHOLESTEROL1 AGE1 BMI1);
matrix CHOLESTEROL1 AGE1 BMI1
/diagonal=(histogram normal);
run;
*b) boxplot of CHOLESTEROL1 for each level of SEX;
proc sgplot data=DataHealthNew
(keep=CHOLESTEROL1 SEX);
vbox CHOLESTEROL1 /category=SEX;
run;
c) boxplot of CHOLESTEROL1 for each level of MARSTAT1;
proc sgplot data=DataHealthNew
(keep=CHOLESTEROL1 MARSTAT1);
vbox CHOLESTEROL1 /category=MARSTAT1;
run;
/ 5. T-test of CHOL1 between male and female /
proc ttest data=DataHealthNew
(keep=CHOLESTEROL1 SEX) ci=equal;
class SEX;
var CHOLESTEROL1;
run;
/ 6. Contingency Tables /
proc freq data=DataHealthNew
(keep=AGEDECADE HICHOL1);
tables AGEDECADE * HICHOL1 / expected nocol chisq;
run;
/ 7. Correlation, MLR, and Plots */
*a). Create a correlation matrix;
proc corr data=DataHealthNew (keep=CHOLESTEROL1 AGE1 BMI1) nomiss;
var CHOLESTEROL1 AGE1 BMI1;
run;
b). Multiple Linear Regression;
proc reg data=DataHealthNew;
model CHOLESTEROL1 = AGE1 FEMALE BMI1 MARRIED WIDDIVSEP;
run;
/ 8. Compare the means for matched samples */
*a). compare paired CHOLESTEROL1 & CHOLESTEROL2;
proc ttest data=DataHealthNew (keep=CHOLESTEROL1 CHOLESTEROL2);
paired CHOLESTEROL2 * CHOLESTEROL1;
run;
e). compare paired CHOLESTEROL1 & CHOLESTEROL2 for AGEDECADE=5;
proc ttest data=DataHealthNew (keep=CHOLESTEROL1 CHOLESTEROL2 AGEDECADE);
where AGEDECADE = 5;
paired CHOLESTEROL2 * CHOLESTEROL1;
run;
/ 9. Comparison of proportions for matched samples */
proc freq data=DataHealthNew (keep=HICHOL1 HICHOL2);
tables HICHOL1 * HICHOL2 /norow nocol agree;
run;
/*10. Logistic Regression */
proc logistic data=DataHealthNew;
class MARSTAT1 (ref='1') /param=reference;
model HICHOL1 (event='1') = FEMALE AGE1 BMI1 MARSTAT1 /rsq;
run;
