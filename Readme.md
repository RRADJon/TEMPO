## Toxicology Exposure-based Mechanistic Pathway mOdel (TEMPO): a Boltz-PBPK-AOP Toxicology Predictor

This is still a **Work in Progress (WiP)** demonstrating the capabilities of post-AlphaFold-3 models for toxicology prediction.  

By integrating **NVIDIA’s Boltz-2 NIM-API** developed by co-folding  (Passaro et al., 2025), a **Neural Network Ensemble**, and a **Physiologically Based Pharmacokinetic (PBPK) engine**, this model predicts how a molecule interacts with critical Molecular Initiating Event (MIE) pathways in virtual patients.

## Key Features
* **Input:** Raw SMILES  
* **Output:** Tissue-specific hazard ratings  


## The Pipeline

### 1. Plasma Protein Binding & Fraction Unbound
Molecular binding affinity is calculated via Boltz-2 co-folding for the chemical of interest against **Albumin** and **AGP**. These results are fed to an ensemble NN to produce the fraction unbound ($f_u$).  
* **Performance:** Current $R^2$ for the NN is **64%**.

### 2. MIE Co-folding & Classification
The molecule is co-folded against predicted MIEs tagged to specific **Adverse Outcome Pathways (AOPs)**. Because Boltz-2 often produces docked results even for non-binders, we use a classifier NN to filter true vs. false binders.  
* **Performance:** Current classifier accuracy on the test set is **65%**.

### 3. PBPK Modeling (ODE Implementation)
The molecule and predicted $f_u$ are processed by a 16-compartment Python-based ODE engine. This predicts distribution, elimination, and tissue concentration ($C_{tissue}$).  
* **Benchmarking:** Compared to PK-Sim, the model achieved an $R^2$ for half-life of **51.8%** and a fold-error of **1.44** across 11 drugs.

### 4. Hazard Integration
$C_{tissue}$ and MIE affinity are fed to an **AOP/Hazard Integration Module**. This combines protein-specific expression data and SIDER side-effect associations to produce hazard ratings for various toxicities.

---

##  Getting Started
To use the tool:
1.  Open the `Integrated Boltz_PBPK_AOP_Simulation.ipynb` notebook.
2.  Follow the link to launch in **Google Colab**.
3.  You will be prompted to obtain an **NVIDIA API Key** (available for free) to run cloud-based Boltz-2 predictions.
