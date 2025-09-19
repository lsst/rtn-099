# Photometric Transformation Relations for the LSST Data Preview 1

```{abstract}
This technical note provides photometric transformation relations between the Vera C. Rubin Observatory's LSST and ComCam systems and other photometric systems. These transformations are derived using both synthetic and empirical data and are intended to support calibration and comparison across survey systems. We present both polynomial equations and lookup-table-based methods, depending on the available data and desired accuracy. The transformations are generally valid for stars with typical spectral energy distributions (SEDs), and caution should be used when applying them to objects with strong emission lines or atypical colors.
```

## Add content here

See the [Documenteer documentation](https://documenteer.lsst.io/technotes/index.html) for tips on how to write and configure your new technote.

## 0. General Overview

These transformations were derived by matching data from other surveys with the Rubin LSST DP1 {cite}`RTN-095`.  This technical note provides a general overview of the results from the calculation of transformations to and from the Rubin LSST DP1 photometric system.  Details of the process will be presented in on or more future journal articles (Porter et al., _in preparation_, etc.).  

This technical note should be considered a living document:  as additional photometric systems are analyzed and/or current transformations are updated, this document will be updated to include the new material.

## 1. Polynomial Fit Transformations

### 1.1. Overview

_Under Construction_



### 1.2. Synthetic LSSTCam Transformations

Synthetic magnitudes were derived by integrating spectrophotometric spectra from the Pickles Stellar Spectra Library {cite}`1998PASP..110..863P` with filter passband transmission curves for DES and LSST. These magnitudes were calculated using broad-band absolute magnitude definitions and processed using a Python-based fitting code to generate transformation equations. Due to the limited number of stars in the Pickles library (~100), the resulting plots are sparse but provide a consistent reference.


:::{table} DES to LSST Transformation Equations (Version `v_2025_08_22`).
:widths: auto

| Transformation Equation                                                     | RMS      | Applicable Color Range       | QA Plot  |
| :-------------------------------------------------------------------------- | -------: | ---------------------------: | -------: |
| $g_{LSST} = g_{DES} + 0.016 (g-i)_{DES} - 0.003 (g-i)^2_{DES} + 0.006$      |  0.002   | $-1.0 < (g-i)_{DES} < 3.9$   | [link](_static/plots/qaPlot.des_to_lsst.fit.dmag_g.gi_des.norder2.qa1.png) |
| $r_{LSST} = r_{DES} + 0.185 (r-i)_{DES} - 0.015 (r-i)^2_{DES} + 0.010$      |  0.008   | $-0.4 < (r-i)_{DES} < 2.3$   | [link](_static/plots/qaPlot.des_to_lsst.fit.dmag_r.ri_des.norder2.qa1.png) |
| $i_{LSST} = i_{DES} + 0.150 (r-i)_{DES} - 0.003 (r-i)^2_{DES} - 0.009$      |  0.005   | $-0.4 < (r-i)_{DES} < 2.2$   | [link](_static/plots/qaPlot.des_to_lsst.fit.dmag_i.ri_des.norder2.qa1.png) |
| $z_{LSST} = z_{DES} + 0.270 (i-z)_{DES} + 0.036 (i-z)^2_{DES} - 0.003$      |  0.010   | $-0.3 < (i-z)_{DES} < 1.84   | [link](_static/plots/qaPlot.des_to_lsst.fit.dmag_z.iz_des.norder2.qa1.png) |
:::


   

### 1.3. ComCam Transformations

ComCam data were used to derive empirical transformations between DES and LSST ComCam filters. All $S/N >$ 5 point sources in the DES footprint were selected, including quasars and non-standard stars.

:::{table} ComCam to DES Transformation Equations (Version `v_2025_08_22`).
:widths: auto

| Transformation Equation                                                             | RMS      | Applicable Color Range        | QA Plot  |
| :---------------------------------------------------------------------------------- | -------: | ----------------------------: | -------: |
| $g_{DES} = g_{ComCam} + 0.005 (g-i)_{ComCam} - 0.001$                               |  0.013   | $-0.6 < (g-i)_{ComCam} < 3.7$ | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_g.gi_ComCam.norder1.qa1.png) |
| $r_{DES} = r_{ComCam} - 0.292 (i-z)_{ComCam} - 0.005 (i-z)^2_{ComCam} + 0.013$      |  0.010   | $-0.2 < (i-z)_{ComCam} < 1.0$ | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_r.iz_ComCam.norder2.qa1.png) |
| $i_{DES} = i_{ComCam} + 0.262 (i-z)_{ComCam} + 0.048 (i-z)^2_{ComCam} - 0.006$      |  0.008   | $-0.2 < (i-z)_{ComCam} < 1.0$ | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_i.iz_ComCam.norder2.qa1.png) |
| $z_{DES} = z_{ComCam} + 0.262 (i-z)_{ComCam} - 0.046 (i-z)^2_{ComCam} + 0.001$      |  0.010   | $-0.2 < (i-z)_{ComCam} < 0.8$ | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_z.iz_ComCam.norder2.qa1.png) |
:::



   

## 2. Lookup Table (Interpolation) Transformations

### 2.1. Overview

Interpolation methods were used to model complex or non-linear relationships between survey measurements. 
These methods rely on binning color indices and computing median magnitude differences.

The lookup tables included in the tables below can be used to convert data from one photometric system to the other via interpolation methods. 
The files contain the delta_mag vs color locus in bins of (typically) 0.1-mag binsize along the color axis. 
Here is a python code that takes the lookkup table CSV file for the transformation from ComCam $g$-band and $(g-i)$ color to DES $g$-band. The code makes use of the `scipy` `interpolate` routine.

```
import pandas as pd
from scipy import interpolate

# Read in lookup table CSV file...
lut_name = 'transInterp.ComCam_to_des.g_gi_ComCam.csv'
df_interp = pd.read_csv(lut_name)

# Create linear interpolation of the median dmag vs. color
#  bin calculated above...
response = interpolate.interp1d(\
                  df_interp.bin_label.values.astype(float), \
                  df_interp.bin_median.values, \
                  bounds_error=False, fill_value=0., \
                  kind='linear')

# Read in file with data to be transformed...
df = pd.read_csv(inputFile)
# The following assumes a column with ComCam g and
# a column with ComCam (g-i) in this file...
df['offset'] = response(df['gi_ComCam'].values)
df['g_des'] = df['g_ComCam'] - df['offset']
```

### 2.2 ComCam Transformations

:::{table} ComCam DP1 to DES DR2 (Version `v_2025_08_22`).
:widths: auto

| Transformation Relation    | RMS      | Applicable Color Range      | QA Plot  | Lookup Table  |
| :--------------------------| -------: | --------------------------: | -------: | ------------: |
| ComCam g,g-i to DES g      |  0.010   | $0.2 < (g-i)_{DES} < 3.0$   | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.g_gi_ComCam.png)     | [link](_static/data/transInterp.ComCam_to_des.g_gi_ComCam.csv) |
| ComCam r,r-i to DES r      |  0.007   | $0.0 < (r-i)_{DES} < 1.7$   | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.r_ri_ComCam.png)     | [link](_static/data/transInterp.ComCam_to_des.r_ri_ComCam.csv) |
| ComCam i,i-z to DES i      |  0.006   | $-0.1 < (i-z)_{DES} < 0.8$  | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.i_iz_ComCam.png)     | [link](_static/data/transInterp.ComCam_to_des.i_iz_ComCam.csv) |
| ComCam z,i-z to DES z      |  0.007   | $-0.1 < (i-z)_{DES} < 0.8$  | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.z_iz_ComCam.png)     | [link](_static/data/transInterp.ComCam_to_des.z_iz_ComCam.csv) |
:::




| Filter | Equation | RMS | Range | Link |
|----------|-------------------------------------------------------------------------------------|---|---|---|
| g_ComCam | $g_ComCam - VIS_{EUCLID} = 0.051 +1.465(VIS-Y)_{Euclid} +0.390(VIS-Y)_{Euclid}^2$ | 0.069 | $-0.1 < ((VIS-Y)_{Euclid}) <= 0.9$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_g_ComCam-VIS_EUCLID.VISY_EUCLID.norder2.qa1.png) |
| g_ComCam | $g_ComCam - VIS_EUCLID = 0.986 +0.488((VIS-Y)_{Euclid}) +0.287((VIS-Y)_{Euclid})^2$ | 0.073 | $0.9 < ((VIS-Y)_{Euclid}) <= 2.3$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_g_ComCam-VIS_EUCLID.VISY_EUCLID.norder2.qa1.png) |
| i_ComCam | $i_ComCam - VIS_EUCLID = -0.054 -0.345((VIS-Y)_{Euclid}) +0.091((VIS-Y)_{Euclid})^2$ | 0.019 | $-0.1 < ((VIS-Y)_{Euclid}) <= 2.3$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_i_ComCam-VIS_EUCLID.VISY_EUCLID.norder2.qa1.png) |
| r_ComCam | $r_ComCam - VIS_EUCLID = -0.059 +0.163((VIS-Y)_{Euclid}) +0.381((VIS-Y)_{Euclid})^2$ | 0.042 | $-0.1 < ((VIS-Y)_{Euclid}) <= 2.3$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_r_ComCam-VIS_EUCLID.VISY_EUCLID.norder2.qa1.png) |
| y_ComCam | $y_ComCam - Y_EUCLID = 0.094 +0.290((Y-H)_{Euclid}) +0.238((Y-H)_{Euclid})^2$ | 0.042 | $-0.6 < ((Y-H)_{Euclid}) <= 0.0$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_y_ComCam-Y_EUCLID.YH_EUCLID.norder2.qa1.png) |
| y_ComCam | $y_ComCam - Y_EUCLID = 0.173 +0.031((Y-H)_{Euclid}) +0.095((Y-H)_{Euclid})^2$ | 0.049 | $0.0 < ((Y-H)_{Euclid}) <= 0.6$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_y_ComCam-Y_EUCLID.YH_EUCLID.norder2.qa1.png) |
| z_ComCam | $z_ComCam - Y_EUCLID = -0.026 +0.366((VIS-Y)_{Euclid}) -0.006((VIS-Y)_{Euclid})^2$ | 0.023 | $-0.1 < ((VIS-Y)_{Euclid}) <= 2.3$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_z_ComCam-Y_EUCLID.VISY_EUCLID.norder2.qa1.png) |


| Filter | Equation | RMS | Range | Link |
|---|---|---|---|---|
| BP_gaia | $BP_gaia - g_ComCam = 0.162 -0.449((g-i)_{ComCam}) +0.081((g-i)_{ComCam})^2$ | 0.027 | $-0.7 < ((g-i)_{ComCam}) <= 3.0$ | [link](_static/plots/qaPlot.ComCam_to_GaiaDR3.fit.dmag_BP_gaia-g_ComCam.gi_ComCam.norder2.qa1.png) |
| G_gaia | $G_gaia - g_ComCam = -0.054 -0.610((g-i)_{ComCam}) -0.033((g-i)_{ComCam})^2$ | 0.008 | $-0.7 < ((g-i)_{ComCam}) <= 3.0$ | [link](_static/plots/qaPlot.ComCam_to_GaiaDR3.fit.dmag_G_gaia-g_ComCam.gi_ComCam.norder2.qa1.png) |
| RP_gaia | $RP_gaia - r_ComCam = -0.500 +0.146((g-i)_{ComCam}) -0.233((g-i)_{ComCam})^2$ | 0.036 | $-0.7 < ((g-i)_{ComCam}) <= 3.0$ | [link](_static/plots/qaPlot.ComCam_to_GaiaDR3.fit.dmag_RP_gaia-r_ComCam.gi_ComCam.norder2.qa1.png) |


| Filter | Equation | RMS | Range | Link |
|---|---|---|---|---|
| g_ps1 | $g_{ps1} - g_ComCam = -0.019 -0.035((g-i)_{ComCam})$ | 0.014 | $-2.2 < ((g-i)_{ComCam}) <= 3.0$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_g_ps1-g_ComCam.gi_ComCam.norder1.qa1.png) |
| i_ps1 | $i_{ps1} - i_ComCam = -0.016 +0.014((g-i)_{ComCam})$ | 0.010 | $-2.2 < ((g-i)_{ComCam}) <= 3.0$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_i_ps1-i_ComCam.gi_ComCam.norder1.qa1.png) |
| r_ps1 | $r_{ps1} - r_ComCam = -0.001 +0.004((g-i)_{ComCam})$ | 0.011 | $-2.2 < ((g-i)_{ComCam}) <= 3.0$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_r_ps1-r_ComCam.gi_ComCam.norder1.qa1.png) |
| y_ps1 | $y_{ps1} - y_ComCam = -0.012 +0.057((z-y)_{ComCam})$ | 0.013 | $-1.7 < ((z-y)_{ComCam}) <= 3.0$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_y_ps1-y_ComCam.zy_ComCam.norder1.qa1.png) |
| z_ps1 | $z_{ps1} - z_ComCam = 0.003 +0.020((i-z)_{ComCam})$ | 0.009 | $-1.8 < ((i-z)_{ComCam}) <= 1.5$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_z_ps1-z_ComCam.iz_ComCam.norder1.qa1.png) |


| Filter | Equation | RMS | Range | Link |
|---|---|---|---|---|
| g_ComCam | $g_ComCam - g_sdss = 0.007 -0.062((g-i)_{sdss})$ | 0.015 | $0.2 < ((g-i)_{sdss}) <= 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_g_ComCam-g_sdss.gi_sdss.norder1.qa1.png) |
| gi_ComCam | $gi_ComCam - gi_sdss = -0.002 -0.064((g-i)_{sdss})$ | 0.021 | $0.2 < ((g-i)_{sdss}) <= 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_gi_ComCam-gi_sdss.gi_sdss.norder1.qa1.png) |
| i_ComCam | $i_ComCam - i_sdss = 0.019 -0.010((g-i)_{sdss})$ | 0.012 | $0.2 < ((g-i)_{sdss}) <= 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_i_ComCam-i_sdss.gi_sdss.norder1.qa1.png) |
| r_ComCam | $r_ComCam - r_sdss = 0.005 -0.007((g-i)_{sdss})$ | 0.011 | $0.2 < ((g-i)_{sdss}) <= 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_r_ComCam-r_sdss.gi_sdss.norder1.qa1.png) |
| z_ComCam | $z_ComCam - z_sdss = 0.032 +0.020((g-i)_{sdss})$ | 0.017 | $0.2 < ((g-i)_{sdss}) <= 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_z_ComCam-z_sdss.gi_sdss.norder1.qa1.png) |


| Filter | Equation | RMS | Range | Link |
|---|---|---|---|---|
| B | $B - g_{ComCam} = 0.215 +0.284((g-i)_{ComCam})$ | 0.024 | $0.2 < ((g-i)_{ComCam}) <= 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_B-g_ComCam.gi_ComCam.norder1.qa1.png) |
| I | $I - i_{ComCam} = -0.359 -0.074((g-i)_{ComCam})$ | 0.016 | $0.2 < ((g-i)_{ComCam}) <= 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_I-i_ComCam.gi_ComCam.norder1.qa1.png) |
| R | $R - r_{ComCam} = -0.142 -0.095((g-i)_{ComCam})$ | 0.015 | $0.2 < ((g-i)_{ComCam}) <= 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_R-r_ComCam.gi_ComCam.norder1.qa1.png) |
| U | $U - u_{ComCam} = -0.687 +0.057((g-i)_{ComCam})$ | 0.088 | $0.2 < ((g-i)_{ComCam}) <= 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_U-u_ComCam.gi_ComCam.norder1.qa1.png) |
| V | $V - g_{ComCam} = -0.041 -0.302((g-i)_{ComCam})$ | 0.022 | $0.2 < ((g-i)_{ComCam}) <= 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_V-g_ComCam.gi_ComCam.norder1.qa1.png) |

## References

```{bibliography}
```
