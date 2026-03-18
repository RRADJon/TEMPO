This is still a WiP to show the capabilities post-AlphaFold-3 models for toxicology prediction.

By integrating NVIDIA’s Boltz-2 NIM-API co-folding a Neural Network Ensemble and a Physiologically Based Pharmacokinetic (PBPK) engine, the model predicts how a molecule interacts with predicted critical molecular initiating event (MIE) pathways in virtual patients.

Key Features:
Input: raw SMILES 
Output: Tissue specific hazard
The bits in between: 
1) Molecular binding affinity through boltz-2 cofolding is calculated for the chemical of interest against Albumin and AGP and fed to an ensemble NN to produce the fraction unbound. Current r^2 for the NN is 64%
2) Next the molecule of interest is co-folded against predicted MIEs tagged to specific adverse outcome pathways (AOPs), these are fed to a classifer NN to account for the propensity for Boltz-2 to always give a docked result, with sometimes overlapping affinities and probabilities for true and false binders. Current classifer NN accuracy on test set is 65%
3) The molecule and the predicted fraction unbound are fed to a python based ODE implementation of PBPK modelling. with 16 compartments. The distribution, elimination, and tissue concentration of the drug are predicted. The PBPK model was benchmarked on same inputs given to PKsim with a r^2 for half-life of 51.8% and Fold-error of 1.44 across 11 drugs.
4) The tissue concentration and affinity to MIE associated proteins is fed to an AOP/Hazard integration module, where the protein specific expression, and SIDER Side effect associated results are combined to produce a hazard rating for different toxicities.

To use click the Integrated Boltz_PBPK_AOP_Simulation.ipynb notebook, and follow to google colab, there you'll be directed to obtain an NVIDIA API key, these are free, and allow you to run very fast Boltz-2 predictions in the cloud.
