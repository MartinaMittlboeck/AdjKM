# AdjKM
A SAS macro to calculate adjusted Kaplan-Meier curves 

## Overview
AdjKM is a SAS macro to calculate adjusted Kaplan-Meier curves in case of a temporarily unknown binary baseline covariate whose values emerge over time (Mittlböck, Heinzl and Pötschger, 2025, submitted).

## Details
Calculation of adjusted Kaplan-Meier curves (AdjKM) and comparison of survival probabilities at an interesting prespecified time point is motivated by investigating the impact of allogeneic stem cell transplantation on childhood leukaemia. The approach compares long-term survival of two cohorts defined by the availability or non-availability of suitable donors for stem cell transplantation. A patient’s cohort membership becomes known only after completed donor search with or without an identified donor. If a patient suffers a donor-search ceasing event, stem cell transplantation will no longer be indicated. In such a case, donor search will be ceased and cohort membership will remain unknown. 
By considering the probability that a donor might have been identified after ceasing of donor search, weights for the calculation of adjusted Kaplan-Meier curves are defined. 
Estimates at a prespecified time point t* are used to test for differences between both groups (based on a log-log transformation). That is, the cumulative hazard ratio at t* (cHR) and the corresponding confidence interval are provided. 

The Macro call for the simulated example data set named simbsp_a_1000_05 would be
> libname simdat 'Data_Path'; 
> <br> %include "Macro_Path\AdjKM_SAS_Macro.sas"; 
> <br> %AdjKMV(data=simdat.simbsp_a_1000_50, td_time=binary_time, event_time=time, event_status=event, tsearch=5, tstar=5, atrisk=%str(0 to 10 by 1), alpha=0.05, boot_method=all, boot_rep=2000, result_out=result_out);

for an analysis time point at 5 years (t*) for the pseudo-values and for a maximum time to identify the group membership of 5 years. 
<br> Data_Path should be replaced by the path where the data set is stored and Macro_Path has to be replaced by the path where the SAS macro is stored.

## SAS-macro options 
<b>data </b> -- name of input data set
<br>
<br> <b>td_time</b>  -- name of variable containing time until donor identification.
<br> Values between zero and min(tsearch, event_time) indicate an available donor.
<br> In case of an unavailable donor set td_time to a missing or negative value.
<br>
<br> <b>event_time</b> -- name of outcome related survival time variable (e.g. overall survival)
<br>
<br> <b>event_status</b> -- name of outcome related survival status, zero is assumed to indicate censoring
<br>
<br> <b>tsearch</b> -- maximum donor search time
<br>
<br> <b>tstar</b> -- time point for calculation of survival probabilities and cHR
<br>
<br> <b>atrisk</b> -- state time points to show patients at risk in the survival plot, e.g. %str(0 to 10 by 1)
<br>
<br> <b>alpha</b> -- two-sided significance level, that is, 1-alpha is a 2-sided confidence level
<br>
<br> <b>Boot_method</b> -- bootstrap methods for two-sided confidence intervals:
<br> NO   - no Bootstrap (only naïve)
<br> WALD - naive and Bootstrap-Wald
<br> ALL  - naive, Bootstrap-Wald, percentile, BC (bias corrected percentile) and BCa (bias corrected and accelerated percentile)
<br> Default: NO
<br>
<br> <b>boot_rep</b> -- number of boostrap replicates (recommended 200 for Wald boostrap and 2000 for percentile, BC and BCa methods)
<br> Default: 0 for no; 200 for Wald; 2000 for all
<br>
<br> <b>out_data</b> -- (library and) prefix of the name of the two output data sets which are distinguished by the postfixes "_TIME" and "_SURV":
<br> _TIME - for individual survival data and weights where "&#95;td&#95;" is the new variable indicating group membership and "_M_weight" is the new weight variable
<br> _SURV - for survival estimates (standard SAS-output from PROC LIFETEST)  


## References
Mittlböck M., Heinzl H. and Pötschger U.: Adjusted Kaplan-Meier curves for partly unobserved group membership in paediatric stem cell transplantation studies, 2025. (submitted)
<br> Mittlböck M., Pötschger U. and Heinzl H.: Weighted pseudo-values for partly unobserved group membership in paediatric stem cell transplantation studies. SMMR Vol.31(1):76-86, 2022. DOI: 10.1177/09622802211041756.
