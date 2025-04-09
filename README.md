# 🧪 ComBat Harmonization Demo (MATLAB)

This repository provides a simple demonstration of how to use ComBat to harmonize multi-site neuroimaging data in MATLAB. ComBat helps reduce inter-scanner variability while preserving meaningful biological differences — an essential step when combining datasets from multiple imaging sites.

---

## 📁 What This Demo Covers

- How to prepare your data for ComBat (feature matrix, batch labels, covariates)  
- A step-by-step MATLAB example for applying ComBat  
- Visualization of the results before and after harmonization

---

## 🔗 Core Method Reference

This demo uses the Matlab implementation from the official ComBat Harmonization repository:  
🔗 [Jfortin1/ComBatHarmonization](https://github.com/Jfortin1/ComBatHarmonization)

---

## 🚀 Getting Started

### 1. Download Required ComBat Scripts

Make sure you have the following `.m` files saved in the **same folder**:

- `aprior.m`  
- `bprior.m`  
- `itSol.m`  
- `postmean.m`  
- `postvar.m`  
- `inteprior.m`  
- `combat.m`

You can find these files in the [ComBatHarmonization repository](https://github.com/Jfortin1/ComBatHarmonization).

Place all the `.m` files into a single directory on your computer (e.g., `~/Documents/MATLAB/ComBat/`).

---

### 2. Add the Folder to Your MATLAB Path

In your MATLAB command window or in a script, run:

```matlab
addpath('path/to/the/scripts/folder'); 

Replace 'path/to/the/scripts/folder' with the actual path to the folder containing the .m files.

### 3. (Optional) Save the Path for Future Sessions

If you want MATLAB to remember this path across sessions, use:

```matlab
savepath;
