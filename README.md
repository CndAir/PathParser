# PathParser
PathParser is a python-based computational tool for the thermodynamics and robustness analysis of native and designed metabolic pathways. The following functionalities are provided:   
   
__1 Max-min driving force (MDF) optimization.__ The Gibbs free energy change of the least favorable reaction will be maximized to evaluate thermodynamic feasibility of the entire pathway.  
   
__2 Protein cost estimation.__ The smallest enzyme investment will be assessed by calculating the minimal total enzyme cost supporting a given pathway flux.   
   
__3 Robustness analysis.__ Specifically, an ensemble of models is generated to simulate the system response to enzyme perturbation based on bifurcation theory and a continuation method. Probability of system failure and flux fold change against enzyme perturbation are estimated as well as a flux control index.   
## Dependencies 
PathParser was developed and tested using Python 3.6+ with the following packages:   
   
numpy 1.16.1, pandas 0.23.4, scipy 1.2.1, sympy 1.1.1, matplotlib 3.0.3, seaborn 0.8.1, openopt 0.5625, pillow 6.0.0, xlrd 1.2.0, cvxopt and openpyxl
## Usage
__main1.py__ performs MDF optimization and protein cost estimation with the following arguments:
   
>-o, --outDir: output directory   
-r, --reactionFile: reaction file, required fields: Enzyme ID, Substrates, Products, Reversibility, [Δ<sub>r</sub>G'<sup>m</sup>](http://equilibrator.weizmann.ac.il/static/classic_rxns/faq.html#what-does-the-m-in-rg-m-fg-m-and-e-m-mean) and Enzyme MW. See below as an example   
   
|#Enzyme ID|Reversibility|Δ<sub>r</sub>G'<sup>m</sup> (kJ/mol)|Substrates|Products|Substrate Km (mM)|Product Km (mM)|kcat (1/s)|Enzyme MW (kDa)|
|---|---|---|---|---|---|---|---|---|
|RuBisCO|0|-34.7|RuBP;CO2|2G3P|0.08(0.019,0.105);0.67(0.529,0.85)||11.6(3.5,14.28)|70|
|PGK|1|18.7|G3P;ATP|BGP;ADP|0.18;0.19|;||41.7|
|GAPDH|1|-19.3|BGP;NADPH|GAP;Pi;NADP|;|;;||36.5|
|TPI|1|-5.5|GAP|DHAP||||26.1|
|FBA|1|-4.6|GAP;DHAP|FBP|;|0.008(0.007,0.16)||40|
|FBPase|0|-26.3|FBP|F6P;Pi|0.052(0.025,0.057)|;|10.5|40|
|TKT1|1|10.1|F6P;GAP|E4P;X5P|;|;||75.1|
|SBA|1|3|E4P;DHAP|SBP|;|0.047(0.008,10)||40|
|SBPase|0|-33.2|SBP|S7P;Pi|0.24|;|4.2|40|
|TKT2|1|3.9|GAP;S7P|R5P;X5P|;|;||75.1|
|RPE|1|3.4|X5P|Ru5P||||25|
|RPI|1|2|R5P|Ru5P||||25
|PRK|0|-22.7|Ru5P;ATP|RuBP;ADP|0.28(0.27,0.29);0.36(0.09,1.42)|;||43.5|
   
Orders in Substrate Km and Product Km fields are the same with those in Substrates and Products; values in Substrate Km, Product Km and kcat fields are presented as geomean(lower bound, upper bound); defaults will be used for missing values
>-b, --concBnds: concentration lower and upper bound (mM) for all metabolites, sep by ","   
-w, --runWhich: which analysis to run, '1' for maximizing the minimal driving force, '2' for minimizing the totol enzyme protein cost, '12' for both   
-i, --iniMetabs: optional, metabolites as initial substrates, sep by ",". By default, they will be detected automatically, sometimes they should be set explicitly, e.g. for cylic pathways   
-f, --finMetabs: optional, metabolites as end products, sep by ",". By default, they will be detected automatically, sometimes they should be set explicitly, e.g. for cylic pathways  
-eb, --exBalMetabs: optional, metabolites excluded from mass balance, sep by ","  
-eo, --exOptMetabs: optional, metabolites excluded from optimization, sep by ","  
-a, --assignFlux: optional, assign flux to some enzyme in the format "enzyme ID:value", then flux distribution will be calculated. By default, influx to pathway will be set to 1. NOTE the calculated flux distribution is equivalent to occurance not the real flux  
-h, --help: show help message and exit  
