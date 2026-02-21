# BD-WD-Spectral-Analysis

Finalized, polished code for the spectral analysis of eclipsing brown dwarf–white dwarf (BD–WD) binary systems.

## Project Description

This repository presents a spectral analysis of the binary systems **SDSS J1411** and **WD1202**. Using optical and infrared spectra obtained from the Multi-Object Double CCD Spectrographs of the **Large Binocular Telescope (LBT)**, we measure the radial velocities (RVs), effective temperatures ($T_{\text{eff}}$), and surface gravity ($\log g$) of the white dwarf.

The analysis was performed using two primary approaches: individual absorption line fitting and $\chi^2$ minimization with WD templates.

### 1. Individual Absorption Line Fitting
To analyze the orbital motion, we modeled the wavelength shifts of individual Balmer absorption lines ($H\alpha, H\beta, H\gamma,$ and $H\delta$). 
* **Model:** Each line was tracked using a composite model consisting of a **Voigt profile** and a low-order polynomial to model the local continuum. 
* **Optimization:** The optimal polynomial order was selected by a custom **Bayesian Information Criterion (BIC)** algorithm to prevent overfitting. 
* **Implementation:** Fitting was performed using `scipy.optimize.curve_fit` to determine best-fit parameters and derive RV semi-amplitudes.

### 2. $\chi^2$ Minimization & Parameter Estimation
We performed a $\chi^2$ minimization fit by comparing the observed spectrum to a synthetic model grid.
* **Surface Interpolation:** We constructed a 2D spectral surface using `RegularGridInterpolator` from the SciPy library, allowing for continuous sampling of the $T_{\text{eff}}$ and $\log g$ parameter space. 
* **Optimization:** Best-fit parameters were identified using the **L-BFGS-B algorithm**. 
* **Uncertainty Analysis:** Formal uncertainties were derived from 1D $\Delta\chi^2 = 1$ intervals and a 2D covariance analysis. By computing the **Hessian matrix** of the $\chi^2$ surface, we accounted for parameter degeneracies and defined $1\sigma$ and $2\sigma$ joint confidence contours.

### 3. Radial Velocity Constraints
Finally, we used the best-fit $\log g$ and $T_{\text{eff}}$ parameters to extract a specific model template. This template was used to constrain the RVs through two methods:
1. **$\chi^2$ Grid Search:** Manually shifting the model template across the data using a grid of RV shifts.
2. **Cross-Correlation (CC):** Finding the shift with the highest correlation at each time step.

*Note: By fixing the temperature and surface gravity first, we allowed the CC and grid search to focus on line locations rather than differences in line depth and width.*

## Requirements

The following Python packages are required to run the analysis:

```bash
pip install numpy scipy xarray h5py specutils astropy matplotlib pandas
```

### Acknowledgements
I sincerely thank Dr. Arthur Adams for his mentorship, guidance, and invaluable feedback throughout this project. I would also like to thank Prof. Yifan Zhou at the University of Virginia for mentorship, financial support, and for providing the data used in this project.
This research was conducted as part of the VICO–CASSUM Summer Program and supported by the University of Chicago Metcalf Summer Experience Grant.

### Contact Information
For further information or if you have any questions, please reach out:
  Emma Pavlicek
  University of Chicago
  Email: ecpavlicek@uchicago.edu

### Note on Scope
This research was conducted over a 7-month period. While the methods used are robust, they represent a specific stage of analysis and may be subject to further refinement or more in-depth exploration in future work.
