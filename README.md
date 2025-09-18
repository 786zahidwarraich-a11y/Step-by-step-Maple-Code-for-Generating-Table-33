Molecular Property Prediction using Topological Indices
This Maple code implements regression models to predict molecular properties of Valsartan (a coronary artery disease drug) based on topological indices derived from graph theory.

Overview
The code performs the following step-by-step process:

Calculates topological indices for Valsartan based on edge eccentricities

Defines regression models for predicting molecular properties

Makes predictions using the calculated indices

Computes errors by comparing predictions with experimental values

Displays comprehensive results in a formatted table

Topological Indices Calculated
The code computes these seven topological indices for Valsartan:

First Zagreb Index (M¹?)

Second Zagreb Index (M²?)

Geometric Arithmetic Index (GA?)

Atom-Bond Connectivity Index (ABC?)

Eccentric Connectivity Index (??)

Inverse Sum Indeg Index (IST?)

Albertson Index (ABL?)

Molecular Properties Predicted
The regression models predict these eight molecular properties:

Molecular Weight (MW)

Heavy Atom Count (HAC)

Complexity (CO)

Boiling Point (BP)

Enthalpy of Vaporization (ENV)

Molar Refractivity (MR)

Polarizability (PO)

Molar Volume (MV)

Mathematical Models
The code implements these regression equations:

MW = 5.071009 + 14.940646·GA? - 0.051512·GA?²

HAC = 1.633191 + 0.898649·GA?

CO = 1464.609602 - 411.521528·ABC? + 41.322346·ABC?² - 1.136243·ABC?³

BP = -656.715103 + 366.999694·log(GA?)

ENV = 77.561582 - 4.542787·ABL? + 0.286690·ABL?² - 0.003783·ABL?³

MR = -81.059629 + 13.377086·ABL? - 0.333008·ABL?² + 0.003469·ABL?³

PO = -32.110883 + 5.297975·ABL? - 0.131718·ABL?² + 0.001370·ABL?³

MV = -476.702302 + 243.262764·log(ABL?)

Usage
For Valsartan (Default)
Simply execute the code in Maple to see the complete analysis for Valsartan.

For Other CAD Drugs
To predict properties for other coronary artery disease drugs:

Calculate the topological indices for your drug

Call the prediction function with your values:

maple
predict_CAD_drug_properties(M_fe, M_ge, GA_e, ABC_e, chi_e, IST_e, ABL_e)
Common CAD drugs include:

Valsartan

Losartan

Atorvastatin

Simvastatin

Requirements
Maple 2018 or newer

No additional packages required beyond standard Maple library

Output
The code generates a comprehensive table (Table 33 format) showing:

Experimental values

Predicted values

Absolute errors

Relative errors (%)

Verification
Results should match Table 33 in the original manuscript within rounding differences.

File Structure
Molecular_Property_Prediction.mpl - Main Maple code file

README.md - This documentation file

References
This code implements the methodology described in the research paper:
"A QSPR Study of Coronary Artery Disease Drugs Using Eccentricity-based Indices"



To begin, we label the molecular graph of Valsartan with vertices v1, v2, ..., v32. Subsequently, the edge set will be defined as follows:
Edge set := {"v10v11", "v11v12", "v11v21", "v12v13", "v13v14", "v14v15", "v15v16", "v17v12", "v1v18", "v1v2", "v20v19", "v20v9",
 "v21v22", "v21v25", "v22v23", "v22v24", "v25v26", "v25v27", "v28v29", "v28v32", "v29v30", "v2v3", "v30v31", "v31v32", "v3v4", 
"v4v28", "v4v5", "v5v18", "v5v6", "v6v19", "v6v7", "v7v8", "v8v9", "v9v10"}.
Next, we determine the eccentricity of each vertex and create edge partitions based on the eccentricities of the edge's endpoints. Using this edge partitioning, we proceed as follows.

Define edge partitions based on eccentricities of end vertices of edges and their corresponding frequencies of Valsartan
edge_table := table([ [15,14] = [3], [15,15] = [1], [14,13] = [5], [13,12] = [8], [12,11] = [6], [11,10] = [3], [10,9] = [3], [9,8] = [3], [8,8] = [2] ]);



The step-by-step code is as follows;
printf("STEP 1: CALCULATING TOPOLOGICAL INDICES FOR VALSARTAN\n");
printf("=====================================================\n");
edge_table := table([[15, 14] = [3], [15, 15] = [1], [14, 13] = [5], [13, 12] = [8], [12, 11] = [6], [11, 10] = [3], [10, 9] = [3], [9, 8] = [3], [8, 8] = [2]]);
printf("Edge Partition Table for Valsartan:\n");
printf("Edge Type\tFrequency\n");
printf("---------\t---------\n");
edges := [indices(edge_table)];
for i to nops(edges) do
edge := edges[i];
if type(edge, list) and nops(edge) = 1 then
edge := edge[1];
end if;
printf("[%d,%d]\t\t%d\n", edge[1], edge[2], edge_table[edge][1]);
end do;
printf("\n");
index_values := table(["M1^e" = 0., "M2^e" = 0., "GA^e" = 0., "ABC^e" = 0., "?^e" = 0., "IST^e" = 0., "ABL^e" = 0.]);
edges := [indices(edge_table)];
edges := map(edge -> if type(edge, list) and nops(edge) = 1 then edge[1]; else edge; end if, edges);
printf("Calculating topological indices step by step:\n");
printf("Edge\t\tM1^e\tM2^e\tGA^e\tABC^e\t?^e\tIST^e\tABL^e\tFreq\n");
printf("----\t\t---\t---\t---\t----\t---\t----\t----\t----\n");
for i to nops(edges) do
current_edge := edges[i];
r := current_edge[1];
t := current_edge[2];
freqs := edge_table[current_edge];
freq := freqs[1];
m1_weight := r + t;
m2_weight := rt;
if r + t <> 0 then
ga_weight := 2sqrt(rt)/(r + t);
else
ga_weight := 0;
end if;
if rt <> 0 then
abc_weight := sqrt((r + t - 2)/(rt));
else
abc_weight := 0;
end if;
if r + t <> 0 then
chi_weight := 1/sqrt(r + t);
else
chi_weight := 0;
end if;
if r + t <> 0 then
ist_weight := rt/(r + t);
else
ist_weight := 0;
end if;
abl_weight := abs(r - t);
printf("[%d,%d]\t%.1f\t%.1f\t%.4f\t%.4f\t%.4f\t%.4f\t%.1f\t%d\n", r, t, m1_weight, m2_weight, ga_weight, abc_weight, chi_weight, ist_weight, abl_weight, freq);
index_values["M1^e"] := freqm1_weight + index_values["M1^e"];
index_values["M2^e"] := freqm2_weight + index_values["M2^e"];
index_values["GA^e"] := freqga_weight + index_values["GA^e"];
index_values["ABC^e"] := freqabc_weight + index_values["ABC^e"];
index_values["?^e"] := freqchi_weight + index_values["?^e"];
index_values["IST^e"] := freqist_weight + index_values["IST^e"];
index_values["ABL^e"] := freq*abl_weight + index_values["ABL^e"];
end do;
printf("\nFinal Topological Indices for Valsartan:\n");
printf("=======================================\n");
index_names := ["M1^e", "M2^e", "GA^e", "ABC^e", "?^e", "IST^e", "ABL^e"];
for j to nops(index_names) do
idx_name := index_names[j];
printf("%s: %.6f\n", idx_name, index_values[idx_name]);
end do;
printf("\n\n");

printf("STEP 2: REGRESSION MODELS DEFINITION\n");
printf("====================================\n");
printf("MW = 5.071009 + 14.940646GA^e - 0.051512GA^e^2 (R² = 0.987)\n");
printf("HAC = 1.633191 + 0.898649GA^e (R² = 0.992)\n");
printf("CO = 1464.609602 - 411.521528ABC^e + 41.322346ABC^e^2 - 1.136243ABC^e^3 (R² = 0.985)\n");
printf("BP = -656.715103 + 366.999694log(GA^e) (R² = 0.978)\n");
printf("ENV = 77.561582 - 4.542787ABL^e + 0.286690ABL^e^2 - 0.003783ABL^e^3 (R² = 0.981)\n");
printf("MR = -81.059629 + 13.377086ABL^e - 0.333008ABL^e^2 + 0.003469ABL^e^3 (R² = 0.976)\n");
printf("PO = -32.110883 + 5.297975ABL^e - 0.131718ABL^e^2 + 0.001370ABL^e^3 (R² = 0.973)\n");
printf("MV = -476.702302 + 243.262764*log(ABL^e) (R² = 0.979)\n");
printf("\n\n");

printf("STEP 3: MAKING PREDICTIONS FOR VALSARTAN\n");
printf("=======================================\n");

MW_exp := 435.5;
HAC_exp := 32;
CO_exp := 608;
BP_exp := 684.9;
ENV_exp := 105.5;
MR_exp := 120.6;
PO_exp := 47.8;
MV_exp := 359.1;
M_fe := 793;
M_ge := 4749;
GA_e := 33.969953;
ABC_e := 13.601934;
chi_e := 7.123505;
IST_e := 197.913319;
ABL_e := 31;
predict_MW := proc(GAe) local result; result := 5.071009 + 14.940646GAe + (-1)0.051512GAe^2; printf("MW = 5.071009 + 14.940646%.6f - 0.051512*%.6f^2 = %.6f\n", GAe, GAe, result); return result; end proc;
predict_HAC := proc(GAe) local result; result := 1.633191 + 0.898649GAe; printf("HAC = 1.633191 + 0.898649%.6f = %.6f\n", GAe, result); return result; end proc;
predict_CO := proc(ABCe) local result; result := 1464.609602 + (-1)411.521528ABCe + 41.322346ABCe^2 + (-1)1.136243ABCe^3; printf("CO = 1464.609602 - 411.521528%.6f + 41.322346*%.6f^2 - 1.136243*%.6f^3 = %.6f\n", ABCe, ABCe, ABCe, result); return result; end proc;
predict_BP := proc(GAe) local result; result := -656.715103 + 366.999694log(GAe); printf("BP = -656.715103 + 366.999694log(%.6f) = %.6f\n", GAe, result); return result; end proc;
predict_ENV := proc(ABLe) local result; result := 77.561582 + (-1)4.542787ABLe + 0.286690ABLe^2 + (-1)0.003783ABLe^3; printf("ENV = 77.561582 - 4.542787%.1f + 0.286690*%.1f^2 - 0.003783*%.1f^3 = %.6f\n", ABLe, ABLe, ABLe, result); return result; end proc;
predict_MR := proc(ABLe) local result; result := -81.059629 + 13.377086ABLe + (-1)0.333008ABLe^2 + 0.003469ABLe^3; printf("MR = -81.059629 + 13.377086*%.1f - 0.333008*%.1f^2 + 0.003469*%.1f^3 = %.6f\n", ABLe, ABLe, ABLe, result); return result; end proc;
predict_PO := proc(ABLe) local result; result := -32.110883 + 5.297975ABLe + (-1)0.131718ABLe^2 + 0.001370ABLe^3; printf("PO = -32.110883 + 5.297975*%.1f - 0.131718*%.1f^2 + 0.001370*%.1f^3 = %.6f\n", ABLe, ABLe, ABLe, result); return result; end proc;
predict_MV := proc(ABLe) local result; result := -476.702302 + 243.262764log(ABLe); printf("MV = -476.702302 + 243.262764log(%.1f) = %.6f\n", ABLe, result); return result; end proc;
printf("Making predictions using the regression models:\n");
printf("----------------------------------------------\n");
printf("\n1. Molecular Weight (MW) prediction:\n");
MW_pred := predict_MW(GA_e);
printf("\n2. Heavy Atom Count (HAC) prediction:\n");
HAC_pred := predict_HAC(GA_e);
printf("\n3. Complexity (CO) prediction:\n");
CO_pred := predict_CO(ABC_e);
printf("\n4. Boiling Point (BP) prediction:\n");
BP_pred := predict_BP(GA_e);
printf("\n5. Enthalpy of Vaporization (ENV) prediction:\n");
ENV_pred := predict_ENV(ABL_e);
printf("\n6. Molar Refractivity (MR) prediction:\n");
MR_pred := predict_MR(ABL_e);
printf("\n7. Polarizability (PO) prediction:\n");
PO_pred := predict_PO(ABL_e);
printf("\n8. Molar Volume (MV) prediction:\n");
MV_pred := predict_MV(ABL_e);
printf("\n\n");

printf("STEP 4: ERROR CALCULATION AND FINAL RESULTS\n");
printf("===========================================\n");
printf("Example prediction for Valsartan:\n");
valsartan_predictions := predict_CAD_drug_properties(M_1e, M_2e, GA_e, ABC_e, chi_e, IST_e, ABL_e);
printf("\nModel Validation for Valsartan:\n");
printf("============================================================\n");
MW_pred := predict_MW(GA_e);
HAC_pred := predict_HAC(GA_e);
CO_pred := predict_CO(ABC_e);
BP_pred := predict_BP(GA_e);
ENV_pred := predict_ENV(ABL_e);
MR_pred := predict_MR(ABL_e);
PO_pred := predict_PO(ABL_e);
MV_pred := predict_MV(ABL_e);
MW_MAE := abs(MW_pred - MW_exp);
HAC_MAE := abs(HAC_pred - HAC_exp);
CO_MAE := abs(CO_pred - CO_exp);
BP_MAE := abs(BP_pred - BP_exp);
ENV_MAE := abs(ENV_pred - ENV_exp);
MR_MAE := abs(MR_pred - MR_exp);
PO_MAE := abs(PO_pred - PO_exp);
MV_MAE := abs(MV_pred - MV_exp);
MW_RMSE := sqrt((MW_pred - MW_exp)^2);
HAC_RMSE := sqrt((HAC_pred - HAC_exp)^2);
CO_RMSE := sqrt((CO_pred - CO_exp)^2);
BP_RMSE := sqrt((BP_pred - BP_exp)^2);
ENV_RMSE := sqrt((ENV_pred - ENV_exp)^2);
MR_RMSE := sqrt((MR_pred - MR_exp)^2);
PO_RMSE := sqrt((PO_pred - PO_exp)^2);
MV_RMSE := sqrt((MV_pred - MV_exp)^2);
printf("Error Metrics for Individual Properties:\n");
printf("Property MAE RMSE\n");
printf("----------------------------------------\n");
printf("MW: %-12.6f %-12.6f\n", MW_MAE, MW_RMSE);
printf("HAC: %-12.6f %-12.6f\n", HAC_MAE, HAC_RMSE);
printf("CO: %-12.6f %-12.6f\n", CO_MAE, CO_RMSE);
printf("BP: %-12.6f %-12.6f\n", BP_MAE, BP_RMSE);
printf("ENV: %-12.6f %-12.6f\n", ENV_MAE, ENV_RMSE);
printf("MR: %-12.6f %-12.6f\n", MR_MAE, MR_RMSE);
printf("PO: %-12.6f %-12.6f\n", PO_MAE, PO_RMSE);
printf("MV: %-12.6f %-12.6f\n", MV_MAE, MV_RMSE);
printf("============================================================\n\n");
printf("Table 33: Experimental and Predicted Values for Valsartan\n");
printf("==================================================================\n");
printf("%-10s %-15s %-15s\n", "Property", "Experimental", "Predicted");
printf("------------------------------------------------------------------\n");
printf("%-10s %-15.1f %-15.6f\n", "MW", MW_exp, MW_pred);
printf("%-10s %-15d %-15.6f\n", "HAC", HAC_exp, HAC_pred);
printf("%-10s %-15d %-15.6f\n", "CO", CO_exp, CO_pred);
printf("%-10s %-15.1f %-15.6f\n", "BP", BP_exp, BP_pred);
printf("%-10s %-15.1f %-15.6f\n", "ENV", ENV_exp, ENV_pred);
printf("%-10s %-15.1f %-15.6f\n", "MR", MR_exp, MR_pred);
printf("%-10s %-15.1f %-15.6f\n", "PO", PO_exp, PO_pred);
printf("%-10s %-15.1f %-15.6f\n", "MV", MV_exp, MV_pred);
printf("==================================================================\n\n");

printf("STEP 5: USAGE INSTRUCTIONS\n");
printf("==========================\n");
printf("VERIFICATION INSTRUCTIONS:\n");
printf("1. This code can be run in any Maple 2018+ environment\n");
printf("2. No additional packages are required beyond the standard Maple library\n");
printf("3. Copy and paste this code into a Maple worksheet and execute it\n");
printf("4. Results should match Table 33 in the manuscript within rounding differences\n\n");
printf("USAGE INSTRUCTIONS FOR CAD DRUG PREDICTION:\n");
printf("1. Calculate topological indices for your CAD drug using graph theory methods\n");
printf("2. Call predict_CAD_drug_properties(M_fe, M_ge, GA_e, ABC_e, chi_e, IST_e, ABL_e)\n");
printf("3. Replace the parameters with your drug's calculated topological indices\n");
printf("4. The function returns predicted values for all 8 molecular properties\n");
printf("5. Common CAD drugs include: Valsartan, Losartan, Atorvastatin, Simvastatin, etc.\n");
