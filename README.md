# MBL Bed Topography using SGS and Co-Simulation 
This repository contains the implementation of Sequential Gaussian Simulation (SGS) and Co-Simulation techniques to create realistic subglacial topography simulations of the volcanic region of Marie Byrd Land (MBL). Part of the code in this repository is based on Gator Glaciology’s GStatSim GitHub repository (link in References).
## Overview
### Background
Previously the bed topography of MBL has been mapped by Bedmap, BedMachine, and DEMOGORGAN. Bedmap and BedMachine rely mainly on kriging, while DEMOGORGAN uses SGS. Kriging will often make a smoothed surface with irregularities, which might cause issues when visualizing volcanoes. DEMOGORGAN uses SGS to simulate the bed topography, but it does not represent the volcanism in MBL well. The method used in this project is both Sequential Gaussian Simulation (SGS) and Co-Simulation. Co-Simulation is used at higher elevations where there is linear correlation between ice surface elevation and bed elevation, and SGS is used everywhere else. 
### Study Area
The study area for this project is the volcanic region of Marie Byrd Land (MBL) in West Antarctica. In this region there are many subglacial volcanoes which makes for many peaks and varied topography. 
<img width="478" height="396" alt="Study Area" align="center" src="https://github.com/user-attachments/assets/8ae4200b-c6ae-47fd-acfc-e7c6d13d94f0" />
## Methods
We use both Co-Simulation and SGS to create realizations of our study area. This mixed approach allows us to use Co-Simulation, which requires a linear correlation between parameters to function. This means for this project that Co-Simulation can only be used when there is a linear correlation between ice surface elevation and bed elevation. In this case correlation only occurs at higher elevations where the ice is generally thinner.
- Import and set data boundary from Bed Machine
- Normalize data for bed elevation and ice surface elevation
- Find correlation coefficient and location for bed elevation and ice surface
<img width="282" height="264" alt="MBL Correlation with caption" src="https://github.com/user-attachments/assets/4a6ff1d8-26bf-42e7-9184-83a2f390f8c2" />

- Run CoKriging and Co-Simulation on all data, using both bed elevation and ice surface elevation to make the realization.
- Run Kriging and SGS on all data, which only uses bed elevation data to create the realization.
- Use correlation form above to split the data. Where there is linear correlation use values from the Co-Simulation realization, and everywhere else use values from the SGSrealization.
- Merge the simulations into one realization. Use data from Co-Simulation above the cutoff, and SGS for everything else.
- Use HiPerGator to create a 100 member ensemble of realizations to upload to DEMOGORGN
<img width="2022" height="649" alt="methods with caption" src="https://github.com/user-attachments/assets/3c6baf4f-faf4-48fd-8816-e02ef358f9a5" />

## Results
The results from this project are 100 realizations of the study are in MBL, which can be used to update DEMOGORGN. The final realizations show both the realistic topography from the geostatistical methods (SGS), as well as clear peaks of the higher elevation volcanoes from the Co-Simulation methods.
<img width="2096" height="732" alt="Comparison" src="https://github.com/user-attachments/assets/85071fdb-382e-45e4-b40f-ac7c97541c3c" />

**Placeholder: Picture of example MBL final visual**
## How to Use
### Environment/Dependancies  
We recommend creating a conda environment using the geostats2026.yml file. 
You can do that with the following commands:
conda env create -f geostats2026.yml
conda activate geostats2026

**We use the following dependencies:** 
- import numpy as np
- import pandas as pd
- import skgstat as skg
- import xarray as xr
- from sklearn.preprocessing import QuantileTransformer
- from skgstat import models
- import matplotlib.pyplot as plt
- from mpl_toolkits.axes_grid1
- import make_axes_locatable from matplotlib.colors
- import LightSource
- import gstatsim as gs
- import random
### Data:
We use data from Bed Machine v3 (placeholder data access). Download this data. The file path will need to be adjusted to where you have saved the data.
### Files:
- **DEMOGORGN_MBL_Visual.ipynb** : downloading and visualizing DEMOGORGAN’s bed elevation for the MBL study area
- **DEMOGORGN_Test_Visual.ipynb** : downloading and visualizing DEMOGORGAN’s bed elevation for the test area
- **Simulation_MBL** : running all 100 simulations of whole MBL area on HiPerGator
- **Simulation_Test.ipynb** : Running 1 combined SGS and Co-Simulation simulation on test area
- **geostats2026.yml** : the enviroment we used, described above

In order to reproduce the results we created, the files should be run as is. If you would like to look at a different smaller test area of MBL you can adjust the initial coordinates in Simulation_Test.ipynb
## Acknowledgements & Data Sources
Initial data is from Bed Machine v3
Geostatistical methods (SGS and Co-Simulation) methods are from Emma Mackie’s GStatSim: https://github.com/GatorGlaciology/GStatSim/tree/main   
For downloading and visualizing data from DEMOGORGN this project follows steps from: https://developmentseed.org/astera-demogorgn/ 

<img width="150" height="150" alt="30" src="https://github.com/user-attachments/assets/da8793cc-b0ba-4d63-972e-d41e3e6f8e5a" />

## Poster
**Placeholder: Picture of poster**
## References
- Behrendt, J. C. (1999). Crustal and lithospheric structure of the west Antarctic rift system from geophysical investigations - a review. Global and Planetary Change, 23(1–4), 25–44. https://doi.org/10.1016/s0921-8181(99)00049-1
- Lucas, E. M., Nyblade, A. A., Wiens, D. A., Aster, R. C., & Wilson, T. J. (2025). Seismic evidence for widespread active magmatism in eastern marie byrd land, Antarctica. Geophysical Research Letters, 52(19). https://doi.org/10.1029/2025gl116647
- MacKie, E. J., Field, M., Wang, L., Yin, Z., Schoedl, N., Hibbs, M., & Zhang, A. (2023). GStatSim V1.0: A python package for geostatistical interpolation and conditional simulation. Geoscientific Model Development, 16(13), 3765–3783. https://doi.org/10.5194/gmd-16-3765-2023. 
- MacKie, E. J., Schroeder, D. M., Zuo, C., Yin, Z., & Caers, J. (2020). Stochastic modeling of subglacial topography exposes uncertainty in water routing at Jakobshavn Glacier. Journal of Glaciology, 67(261), 1–9. https://doi.org/10.1017/jog.2020.84
- Van Wyk de Vries, M., Bingham, R. G., & Hein, A. S. (2017). A new volcanic province: An inventory of subglacial volcanoes in West Antarctica. Geological Society, London, Special Publications, 461(1), 231–248. https://doi.org/10.1144/sp461.7
