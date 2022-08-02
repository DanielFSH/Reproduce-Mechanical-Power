# Conclusion for reproduction

The following is a logbook/ultimate conclusion for a reproduction of a published scientific study. Feel free to add/remove sections as you find them useful.

## Title
Mechanical power of ventilation is associated with mortality in critically ill patients: an analysis of patients in two observational cohorts (https://doi.org/10.1007/s00134-018-5375-6)

## Known differences
For each difference, describe: (1) the original study approach, (2) the reproduction approach, (3) the justification for the change. If possible, classify the differences as major (could impact the result of the study) or minor (unlikely to change the result of the study).

The patients in MIMIC-III are those admitted from 2001-2012, where as those in MIMIC-IV are from 2008-2019.

Unable to extract data regarding smoking status of participants.

In MIMIC-III there was a derived table with the Elixhauser comorbidity score, or at least there is code in the MIMIC-III repo on how to obtain it. I used the same code to try to extract the Elixhauser comorbidity scores from MIMIC-IV with some alterations. They don't mention which version of Elixhauser they use, but as they reference the paper by Walraven, I assmued that was the version they used. The results of this extraction vary widely from what appears to have been attained in the original work. This is a Major difference that is likely to affect the results of the study.

In the supplementary they state that outliers, defined as an observation that lies outside 1.5 x Interquartile Range (IQR), were checked and substituted by the 5th or 95th percentile. Something that was not done in this reproduction which could have majorly affected the results.
## Unknown differences
Specify changes to the data processing and/or methodology which are known to y

Specify changes to the data processing and/or methodology which *may* have occurred, but you are unable to confirm due to ambiguity in the original material studied. For each difference, describe (1) the most specific reference to the approach in the original study, if possible, and (2) the approach taken in the reproduction.

The paper writes about the "first 48 hours of ventilation""

The formula for Mechanical ventilation with the conversion factor they used requires the use of tidal volume in Liters, however everywhere in the paper they used tidal volume as ml/kg PBW. The paper makes no mention of them using the latter tidal volume with a different conversion factor, so I assume they used tidal volume in liters and used the ml/kg to report the tidal volume in Table 2.

In Table 1 the paper lists different sources of admission: Ward or step-down unit, Emergency room, Office or operating room, Transferred from other hospital, and Other. When compared to the list of admission sources listed in MIMIC-IV, only Emergency Room and Transfer from another hospital were present in both. I used transfers from the Psych ward as admissions from Ward or step-down, and ambulatory surgical admissions as admissionf from Office or operating room.

The text states as exclusion criteria that they excluded "Patients receiving ventilation through a tracheostomy 
cannula at any time during the frst 48  h of ventilation." But in MIMIC-IV invasive ventilation and tracheostomy are separate ventilation statuses, so I excluded all non-invasive ventilations.

The MP formula was given, however they stated that the patients had several measurements available, 
the mean between the highest and the lowest value in the second 24 h was used. I took this to mean that MP was calculated at all possible time points and the average of the highest and lowest MP in the second 24 hours was used. They might have meant that the average of the highest and lowest values of the measurements were used to calculate the MP instead.

In the paper they do not state how they defined pressence of ARDS, just that they scored it based on the Berlin definiton. The Berlin definition of ARDS requires Chest imaging to show Bilateral opacities and an objective assesment of origin of edema, two things which I was unable to extract. I used the Oxygenation criteria to score the PaO2/FIO2 ratio and PEEP of all individuals in the cohort.

In the multivaraible model they adjust for some variables which in their supplementary they list as having some missing values. I ran into the same issue during the reproduction. They make no mention of what approach they took in such a case, I chose to impute with the mean of the variable.

They do not state anything about the model beyond that it is a multivariable model they used, beyond which variables they adjusted for. No mention of any scaling or preprocessing, or of treating any variables as categorical. For the dichotomous outcome I chose to perform logistic regression after imputing all the missing variables, and treating them all as continuous. For the days without ventilation and lenght of stay outcomes, multivariable linear regression was performed in a similar manner.

## Comparison of population

A table comparing the population measures between the original and the reproduction.

Population measure | Original Study | Reproduction
--- | --- | ---
Age (years)| 64.6 (50.7-76.7) | 61.0 (48.0-73.0)
Male (gender)| 2161/3846 (56.2) |1819/3117 (58.4)
Weight (kg) |80.0 (66.6-96.0)|83.0 (69.5-100.0)
Height (cm) |170 |(163-178)|170.0 (163.0-178.0)
BMI (kg/m2) |27.8 (24-32.9)|28.6 (24.4-34.3)
PBW (kg) |64.0 (54.7-73.1)|52.3 (41.4-59.6)
Admission type||
 Surgical elective |290/3846 (7.5)|26/3117 (0.8)
 Surgical urgency |154/3846 (4.0) |1129/3117 (36.0)
 Medical |3402/3846 (88.5)|1962/3117 (62.9)
Source of admission||
 Ward or step-down unit |564/3846 (14.7)|8/3117 (0.3)
 Emergency room |1888/3846 (49.1)| 1060/3117 (34.0)
 Office or operating room |403/3846 (10.5)|3/3117(0.1)
 Transferred from other hospital |965/3846 (25.1)|1136/3117 (36.4)
 Other |26/3846 (0.7)|910/3117 (29.2)
Ethnicity||
 Black |256/3846 (6.7)|270/3117 (8.7)
 Hispanic |128/3846 (3.3)|105/3117 (3.4)
 White |2582/3846 (67.1)|1837/3117 (58.9)
 Other |880/3846 (22.9)| 905/3117 (29.0)
Comorbidities||
 COPD |208/3846 (5.4)|285/3117 (9.1)
Elixhauser comorbidity score |6 (1-12)|
ARDS at baseline |443/3846 (11.5)|678/3117 (21.8)
 Mild |43/443 (9.7)|504/678 (75.2)
 Moderate |230/443 (51.9) |159/678 (23.5)
 Severe |170/443 (38.4)|15/678 (2.2)
Need of support in the frst 24 h ||
 Vasopressor |1959/3846 (50.9)|587/3117 (18.8)
 Renal replacement therapy |204/3846 (5.3)|292/3117 (9.4)
Severity of illness	||
 SAPS II |43 (33-54)|44.0 (34.0-56.0)
 OASIS |38 (33-44)	|42.0 (37.0-49.0)
 SOFA |6 (4-9)	|10.0 (6.0-13.0)
Vital signs at the beginning of ventilation ||
 Heart rate (bmp)	|92 (80-104) |94.0 (83.5-104.0)
 MAP (mmHg)	| 80 (73-89)  |88.5 (76.0-122.0)
 SpO2 (%)	|96 (94-98)|94.0 (90.5-95.5)
 Temperature (C)|37.1 (36.6-37.6)|	36.9 (35.8-37.7)
Laboratory data at the beginning of ventilation ||
 pH |7.36 (7.31-7.41)	|7.36 (7.31-7.41)
 PaO2/FiO2 (mmHg)	|255 (183-357) |255.0 (197.5-3366)
 PaCO2 (mmHg)	| 39 (35-44)|42.0 (37-48)
## Comparison of results

When attempting to reproduce the results of the analysis of the study, the results of the multivariable analysis did not show the same results as that of the study. The 95% confidence intervals of the Odds ratio of MP for the primary and most secondary outcomes failed to exclude 1 in the reproduction, where as they always excluded 1 in the original dataset. The only exception was the C.I for the odds ratio of MP and ICU-legnth of stay, which excluded 1 and seemed to be fairly similar to that of the original paper.
## Conclusion(s) regarding reproducibility

Highlight specific challenges faced during the reproduction attempt which could be improved upon in the future.

The paper has an "Initial diagnosis" section on Table 1, but it is not described anywhere what they consider and initial diagnosis. I chose to only consider the first ICD code in the diagnosis table as an 'initial' diagnosis. I attempted to do so using ICD codes of conditions that mentioned heart diseases, sepsis and pneumonia, and respiratory conditions.

Table 1 makes reference to a "Limitation of Support", and the supplementary state that it refers to individuals not having a full code upon discharge fromt the ICU. I was not sure how to extract this.

