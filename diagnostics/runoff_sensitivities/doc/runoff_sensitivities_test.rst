Runoff Sensitivities Diagnostic Documentation
=============================================

Last update: 12/02/2024

Introduction to runoff sensitivity
----------------------------------

Runoff projections are essential for future water security. The Earth System Models (ESMs) are now being used to inform water resource assessments. However, the substantial model uncertainty has undermined the reliability of ESM projection.The primary source of the runoff projection uncertainty is the model's climate forcing of the land surface, namely inter-model differences in precipitation (P) and surface air temperature (T) trends.

However, runoff projections are generally more uncertain than either P and T projections, implying that additional uncertainties arise at the terrestrial interface (:ref:`Lehner et al., 2019 <ref-Lehner>`; :ref:`Wang et al., 2022 <ref-Wang>`). These additional uncertainties stem from differences in the sensitivity of runoff generation - how runoff responds to a given set of climate forcings. Previous studies have measured this using runoff sensitivity, a metric that qunatifies runoff (Q) changes in response to changes in preciptiation (P sensitivity; δQ/δP, %/%) and temeprature (T sensitivity; δQ/δT, %/°C) (:ref:`Tang and Lettenmaier, 2012 <ref-Tang>`; :ref:`Hoerling et al., 2019 <ref-Hoerling>`; :ref:`Lehner et al., 2019 <ref-Lehner>`; :ref:`Milly and Dunne, 2020 <ref-Milly>`). 

Uncertainty in this runoff sensitivity, inherent to each ESM, can contribute to the projection uncertainty as much as the climate forcings (Kim et al. 2025, *in preparation*). In addition, the runoff sensitivity is often biased in ESMs, indicating that ESMs underestimate the future runoff declines globally (:ref:`Zhang et al., 2023 <ref-Zhang>`; :ref:`Douville et al., 2024 <ref-Douville>`; Kim et al. 2025, *in preparation*). Therefore, reducing the runoff sensitivity bias is important for producing a more reliable projection of terrestrial climate change.

Calculation of runoff sensitivity
---------------------------------

In this diagnostics, we quantify the runoff sensitivity of target model using the multiple linear regression. The historical timeseries (1945-2014) is averaged for each water year (Oct.-Sep.) and 131 global river basins.
Then, the annual timeseries is smoothed with 5-year moving windows to minimize the storage effect of runoff from previous years. Eventually, the runoff sensitivity is calculated with multiple linear regression as below:

.. math::

   \delta{Q} &= {\alpha}\delta{P}
   \frac{D \mathbf{u}_g}{Dt} + f_0 \hat{\mathbf{k}} \times \mathbf{u}_a &= 0; \\

δQ=αδP+βδT+γδPδT

where α and β are estimates of P and T sensitivity respectively, δ represents 5-year averaged anomalies relative to long-term mean, δQ and δP are percent anomalies in runoff and precipitation, δT is temperature anomalies, δPδT is an interaction term. Note that the interaction term is usually negligible and does not affect the values of α and β in CMIP5/6 models.

The calculated runoff sensitivity for target model is then compared to the pre-calculated observational estimations and CMIP5/6 models. For the observational estimates, we used the machine learning-based global runoff reanalysis (GRUN-ENSEMBLE; Ghiggi et al. 2021). We used 100 realizations of GRUN-ENSEMBLES to sample the observational uncertainty. For CMIP models, we used historical simulation from 1945 to 2014. For CMIP5, we extended the timeseries to 2014 using RCP4.5 scenario. The ranges from observational uncertainty, inter-model spread, and uncertainty of regression coefficients are shown foe specific river basins.

Version & Contact info
----------------------

- Version/revision information: version 2 (12/03/2024)
- PI: Flavio Lehner, Cornell University, flavio.lehner@cornell.edu
- Developer/point of contact: Hanjun Kim, Cornell University, hk764@cornell.edu
- Other contributors: Andy wood, David Lawrence, Katie Dagon, Sean Swenson, Nathan Collier

Open source copyright agreement
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The MDTF framework is distributed under the LGPLv3 license (see LICENSE.txt). 

Functionality
-------------

The main driver code (runoff_sensitivities_diag.py) include all functions, codes for calculations and figures.

0) Functions are pre-defined for further analysis.
1) The codes will load data and do average for each water year and 131 global river basins (Basin Mask source: GRDC Major River Basins of the World).
2) Water budget clousre (precipitaton - evapotranspiration == runoff) in long term average is checked.
3) Using the pre-defined function "runoff_sens_reg", runoff sensitivities are calculated.
4) Calculated runoff sensitivities for target models are saved as .nc file.
5) The diagnostic figures will be plotted with the pre-calculated OBS and CMIP data.


Required programming language and libraries
-------------------------------------------

Python 3 is used to calculate and draw the figures.

- All libraries used in this diagnostic are available in MDTF conda environment "_MDTF_python3_base".
- Used libraries: "scipy", "numpy", "matplotlib", "netCDF4", "cartopy", "sklearn"    
- To deal with the shapefile, "cartopy.io.shapereader" and "matplotlib.path" is utilized.
- For multi-linear regression, "sklearn.linear_model" is utilized.    

**Caution**: In Oct. 2023, diagnostic does not work after update in "pydantic" library.
Below commands for downgrading "pydantic" solved the problem for us.

.. code-block:: restructuredtext
   
   conda activate _MDTF_base
   conda install -c conda-forge pydantic==1.10.9


Required model output variables
-------------------------------

The monthly historical simulations including period 1945-2014 are needed.
(Model outputs are assumed to be same with CMIP output.)

Target variables:
   - tas (surface air temperature, K), [time, lat, lon]
   - pr (precipitaiton, kg m-2 s-1), [time, lat, lon] 
   - hfls (latent heat flux, W m-2), [time, lat, lon]
   - mrro (runoff, kg m-2 s-1), [time, lat, lon]

Lon-lat grids for all variables have to be same. In CMIP, there are some models in which grids are slightly different between land and atmospheric variables. Checking and interpolation are recommended.


References
----------   
.. _ref-Lehner:

1. Lehner et al. (2019): The potential to reduce uncertainty in regional runoff projections from climate models. *Nature Climate Change*, **9** (12), 926-933, `doi:10.1038/s41558-019-0639-x <https://doi.org/10.1038/s41558-019-0639-x>`__.

.. _ref-Wang:

2. Wang et al. (2022): Future Changes in Global Runoff and Runoff Coefficient From CMIP6 Multi‐Model Simulation Under SSP1‐2.6 and SSP5‐8.5 Scenarios. *Earth’s Future*, **10** (12), e2022EF002910, `doi:10.1029/2022EF002910 <https://doi.org/10.1029/2022EF002910>`__.

.. _ref-Tang:

3. Tang, Q., & Lettenmaier, D. P. (2012): 21st century runoff sensitivities of major global river basins. *Geophysical Research Letters*, **39** (6), 2011GL050834, `doi:10.1029/2011GL050834 <https://doi.org/10.1029/2011GL050834>`__.

.. _ref-Hoerling:

4. Hoerling et al. (2019): Causes for the Century-Long Decline in Colorado River Flow. *Journal of Climate*, **32** (23), 8181–8203, `doi:10.1175/JCLI-D-19-0207.1 <https://doi.org/10.1175/JCLI-D-19-0207.1>`__.

.. _ref-Zhang:

5. Zhang et al. (2023): Future global streamflow declines are probably more severe than previously estimated. *Nature Water*, **1** (3), 261–271, `doi:10.1038/s44221-023-00030-7 <https://doi.org/10.1038/s44221-023-00030-7>`__.

.. _ref-Milly:

6. Milly, P. C. D., & Dunne, K. A. (2020): Colorado River flow dwindles as warming-driven loss of reflective snow energizes evaporation. *Science*, **367** (6483), 1252–1255, `doi:10.1126/science.aay9187 <https://doi.org/10.1126/science.aay9187>`__.

.. _ref-Douville:

7. Douville, H. (2024): Observational Constraints on Basin-Scale Runoff: A Request for Both Improved ESMs and Streamflow Reconstructions. * Geophysical Research Letters*, **51** (13), e2024GL108824, `doi:10.1029/2024GL108824 <https://doi.org/10.1029/2024GL108824>`__.

.. _ref-Ghiggi:

8. Ghiggi et al. (2021): G‐RUN ENSEMBLE: A multi‐forcing observation‐based global runoff reanalysis. *Water Resources Research*, **57** (5), e2020WR028787, `doi:10.1029/2020WR028787 <https://doi.org/10.1029/2020WR028787>`__.

:ref:`Lehner et al., 2019 <ref-Lehner>`
:ref:`Wang et al., 2022 <ref-Wang>`
:ref:`Tang and Lettenmaier, 2012 <ref-Tang>`
:ref:`Hoerling et al., 2019 <ref-Hoerling>`
:ref:`Zhang et al., 2023 <ref-Zhang>`
:ref:`Milly and Dunne, 2020 <ref-Milly>`
:ref:`Douville et al., 2024 <ref-Douville>`
:ref:`Ghiggi et al., 2019 <ref-Ghiggi>`


More about this diagnostic
--------------------------

TBD


The runoff sensitivity in climate model is often biased. In general, the negative T sensitivity is often too weak in climate models, indicating that ESMs often underestimate the future runoff decline. 
However, while the P sensitivity is generally correlated with the mean state biases, the T sensitivity exhibits no systematic relationship with mean state biases. Hence, the traditional modeling approaches, which focus on improving mean state biases, may not reseolve the T sensitivity biases. Therefore, we need the new diagnostics of runoff sensitivity to facilitate the future model development.
