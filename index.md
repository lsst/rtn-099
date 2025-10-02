# Photometric Transformation Relations for the LSST Data Preview 1

```{abstract}
This technical note provides photometric transformation relations between the Vera C. Rubin Observatory's LSST and ComCam systems and other photometric systems. These transformations are derived using both synthetic and empirical data and are intended to support calibration and comparison across survey systems. We present both polynomial equations and lookup-table-based methods, depending on the available data and desired accuracy. The transformations are generally valid for stars with typical spectral energy distributions (SEDs), and caution should be used when applying them to objects with strong emission lines or atypical colors.
```

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

#### 1.3.1 ComCam <--> DES

#### 1.3.1.1 Original

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

#### 1.3.1.2 Updated

| Conversion               | Transformation Equation                            |   RMS | Applicable Color Range        | QA Plot                                                                                            |
|:-------------------------|:---------------------------------------------------|------:|:------------------------------|:---------------------------------------------------------------------------------------------------|
|      &nbsp;              |                                                    |       |                               |       |
| $g_{des} \to g_{ComCam}$ | $g_{ComCam} - g_{des} = -0.001 (g-i)_{des} +0.005$ | 0.008 | $-0.7 < (g-i)_{des} \leq 2.3$ | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_g_ComCam-g_des.gi_des.norder1.qa1.png) |
| $g_{des} \to g_{ComCam}$ | $g_{ComCam} - g_{des} = +0.016 (g-i)_{des} -0.026$ | 0.016 | $2.3 < (g-i)_{des} \leq 4.0$  | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_g_ComCam-g_des.gi_des.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $r_{des} \to r_{ComCam}$ | $r_{ComCam} - r_{des} = +0.051 (g-i)_{des} -0.003$ | 0.006 | $-0.7 < (g-i)_{des} \leq 2.0$ | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_r_ComCam-r_des.gi_des.norder1.qa1.png) |
| $r_{des} \to r_{ComCam}$ | $r_{ComCam} - r_{des} = +0.104 (g-i)_{des} -0.109$ | 0.013 | $2.0 < (g-i)_{des} \leq 4.0$  | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_r_ComCam-r_des.gi_des.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $i_{des} \to i_{ComCam}$ | $i_{ComCam} - i_{des} = +0.041 (g-i)_{des} -0.016$ | 0.006 | $-0.7 < (g-i)_{des} \leq 1.8$ | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_i_ComCam-i_des.gi_des.norder1.qa1.png) |
| $i_{des} \to i_{ComCam}$ | $i_{ComCam} - i_{des} = +0.109 (g-i)_{des} -0.141$ | 0.009 | $1.8 < (g-i)_{des} \leq 4.0$  | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_i_ComCam-i_des.gi_des.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $z_{des} \to z_{ComCam}$ | $z_{ComCam} - z_{des} = +0.304 (i-z)_{des} -0.004$ | 0.006 | $-0.3 < (i-z)_{des} \leq 0.2$ | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_z_ComCam-z_des.iz_des.norder1.qa1.png) |
| $z_{des} \to z_{ComCam}$ | $z_{ComCam} - z_{des} = +0.237 (i-z)_{des} +0.005$ | 0.007 | $0.2 < (i-z)_{des} \leq 1.1$  | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_z_ComCam-z_des.iz_des.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $Y_{des} \to y_{ComCam}$ | $y_{ComCam} - Y_{des} = +0.103 (i-z)_{des} -0.034$ | 0.01  | $-0.3 < (i-z)_{des} \leq 0.2$ | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_y_ComCam-Y_des.iz_des.norder1.qa1.png) |
| $Y_{des} \to y_{ComCam}$ | $y_{ComCam} - Y_{des} = +0.041 (i-z)_{des} -0.027$ | 0.011 | $0.2 < (i-z)_{des} \leq 1.1$  | [link](_static/plots/qaPlot.DESDR2_to_ComCam_ECDFS.fit.dmag_y_ComCam-Y_des.iz_des.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |

| Conversion               | Transformation Equation                               |   RMS | Applicable Color Range           | QA Plot                                                                                               |
|:-------------------------|:------------------------------------------------------|------:|:---------------------------------|:------------------------------------------------------------------------------------------------------|
|      &nbsp;              |                                                    |       |                               |       |
| $g_{ComCam} \to g_{des}$ | $g_{des} - g_{ComCam} = -0.000 (g-i)_{ComCam} -0.004$ | 0.009 | $-0.6 < (g-i)_{ComCam} \leq 2.3$ | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_g_des-g_ComCam.gi_ComCam.norder1.qa1.png) |
| $g_{ComCam} \to g_{des}$ | $g_{des} - g_{ComCam} = -0.022 (g-i)_{ComCam} +0.039$ | 0.017 | $2.3 < (g-i)_{ComCam} \leq 3.7$  | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_g_des-g_ComCam.gi_ComCam.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $r_{ComCam} \to r_{des}$ | $r_{des} - r_{ComCam} = -0.054 (g-i)_{ComCam} +0.005$ | 0.007 | $-0.6 < (g-i)_{ComCam} \leq 2.0$ | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_r_des-r_ComCam.gi_ComCam.norder1.qa1.png) |
| $r_{ComCam} \to r_{des}$ | $r_{des} - r_{ComCam} = -0.116 (g-i)_{ComCam} +0.127$ | 0.013 | $2.0 < (g-i)_{ComCam} \leq 3.7$  | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_r_des-r_ComCam.gi_ComCam.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $i_{ComCam} \to i_{des}$ | $i_{des} - i_{ComCam} = -0.043 (g-i)_{ComCam} +0.017$ | 0.006 | $-0.6 < (g-i)_{ComCam} \leq 1.8$ | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_i_des-i_ComCam.gi_ComCam.norder1.qa1.png) |
| $i_{ComCam} \to i_{des}$ | $i_{des} - i_{ComCam} = -0.122 (g-i)_{ComCam} +0.157$ | 0.009 | $1.8 < (g-i)_{ComCam} \leq 3.7$  | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_i_des-i_ComCam.gi_ComCam.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $z_{ComCam} \to z_{des}$ | $z_{des} - z_{ComCam} = -0.318 (i-z)_{ComCam} +0.004$ | 0.007 | $-0.3 < (i-z)_{ComCam} \leq 0.2$ | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_z_des-z_ComCam.iz_ComCam.norder1.qa1.png) |
| $z_{ComCam} \to z_{des}$ | $z_{des} - z_{ComCam} = -0.217 (i-z)_{ComCam} -0.010$ | 0.007 | $0.2 < (i-z)_{ComCam} \leq 1.1$  | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_z_des-z_ComCam.iz_ComCam.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $y_{ComCam} \to Y_{des}$ | $Y_{des} - y_{ComCam} = -0.105 (i-z)_{ComCam} +0.034$ | 0.01  | $-0.3 < (i-z)_{ComCam} \leq 0.2$ | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_Y_des-y_ComCam.iz_ComCam.norder1.qa1.png) |
| $y_{ComCam} \to Y_{des}$ | $Y_{des} - y_{ComCam} = -0.038 (i-z)_{ComCam} +0.026$ | 0.011 | $0.2 < (i-z)_{ComCam} \leq 1.1$  | [link](_static/plots/qaPlot.ComCam_to_DESDR2_ECDFS.fit.dmag_Y_des-y_ComCam.iz_ComCam.norder1.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |


#### 1.3.2 ComCam <--> Euclid 

| Conversion                    | Transformation Equation                                                                |   RMS | Applicable Color Range             | QA Plot                                                                                                |
|:------------------------------|:---------------------------------------------------------------------------------------|------:|:-----------------------------------|:-------------------------------------------------------------------------------------------------------|
|      &nbsp;              |                                                    |       |                               |       |
| $VIS_{EUCLID} \to g_{ComCam}$ | $g_{ComCam} - VIS_{EUCLID} = +0.390 (VIS-Y)_{Euclid}^2 +1.465 (VIS-Y)_{Euclid} +0.051$ | 0.069 | $-0.1 < (VIS-Y)_{Euclid} \leq 0.9$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_g_ComCam-VIS_EUCLID.VISY_EUCLID.norder2.qa1.png) |
| $VIS_{EUCLID} \to g_{ComCam}$ | $g_{ComCam} - VIS_{EUCLID} = +0.287 (VIS-Y)_{Euclid}^2 +0.488 (VIS-Y)_{Euclid} +0.986$ | 0.073 | $0.9 < (VIS-Y)_{Euclid} \leq 2.3$  | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_g_ComCam-VIS_EUCLID.VISY_EUCLID.norder2.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $VIS_{EUCLID} \to r_{ComCam}$ | $r_{ComCam} - VIS_{EUCLID} = +0.381 (VIS-Y)_{Euclid}^2 +0.163 (VIS-Y)_{Euclid} -0.059$ | 0.042 | $-0.1 < (VIS-Y)_{Euclid} \leq 2.3$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_r_ComCam-VIS_EUCLID.VISY_EUCLID.norder2.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $VIS_{EUCLID} \to i_{ComCam}$ | $i_{ComCam} - VIS_{EUCLID} = +0.091 (VIS-Y)_{Euclid}^2 -0.345 (VIS-Y)_{Euclid} -0.054$ | 0.019 | $-0.1 < (VIS-Y)_{Euclid} \leq 2.3$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_i_ComCam-VIS_EUCLID.VISY_EUCLID.norder2.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $Y_{EUCLID} \to z_{ComCam}$   | $z_{ComCam} - Y_{EUCLID} = -0.006 (VIS-Y)_{Euclid}^2 +0.366 (VIS-Y)_{Euclid} -0.026$   | 0.023 | $-0.1 < (VIS-Y)_{Euclid} \leq 2.3$ | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_z_ComCam-Y_EUCLID.VISY_EUCLID.norder2.qa1.png)   |
|      &nbsp;              |                                                    |       |                               |       |
| $Y_{EUCLID} \to y_{ComCam}$   | $y_{ComCam} - Y_{EUCLID} = +0.238 (Y-H)_{Euclid}^2 +0.290 (Y-H)_{Euclid} +0.094$       | 0.042 | $-0.6 < (Y-H)_{Euclid} \leq 0.0$   | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_y_ComCam-Y_EUCLID.YH_EUCLID.norder2.qa1.png)     |
| $Y_{EUCLID} \to y_{ComCam}$   | $y_{ComCam} - Y_{EUCLID} = +0.095 (Y-H)_{Euclid}^2 +0.031 (Y-H)_{Euclid} +0.173$       | 0.049 | $0.0 < (Y-H)_{Euclid} \leq 0.6$    | [link](_static/plots/qaPlot.Euclid_to_ComCam.fit.dmag_y_ComCam-Y_EUCLID.YH_EUCLID.norder2.qa1.png)     |
|      &nbsp;              |                                                    |       |                               |       |


| Conversion                    | Transformation Equation                                                            |   RMS | Applicable Color Range           | QA Plot                                                                                              |
|:------------------------------|:-----------------------------------------------------------------------------------|------:|:---------------------------------|:-----------------------------------------------------------------------------------------------------|
|      &nbsp;              |                                                    |       |                               |       |
| $r_{ComCam} \to VIS_{EUCLID}$ | $VIS_{EUCLID} - r_{ComCam} = -0.047 (g-i)_{ComCam}^2 -0.127 (g-i)_{ComCam} +0.074$ | 0.021 | $0.0 < (g-i)_{ComCam} \leq 1.8$  | [link](_static/plots/qaPlot.ComCam_to_Euclid.fit.dmag_VIS_EUCLID-r_ComCam.gi_ComCam.norder2.qa1.png) |
| $r_{ComCam} \to VIS_{EUCLID}$ | $VIS_{EUCLID} - r_{ComCam} = -0.065 (g-i)_{ComCam}^2 -0.516 (g-i)_{ComCam} +0.852$ | 0.055 | $1.8 < (g-i)_{ComCam} \leq 4.0$  | [link](_static/plots/qaPlot.ComCam_to_Euclid.fit.dmag_VIS_EUCLID-r_ComCam.gi_ComCam.norder2.qa1.png) |
|      &nbsp;              |                                                    |       |                               |       |
| $y_{ComCam} \to Y_{EUCLID}$   | $Y_{EUCLID} - y_{ComCam} = -18.061 (z-y)_{ComCam}^2 -0.379 (z-y)_{ComCam} -0.037$  | 0.045 | $-0.2 < (z-y)_{ComCam} \leq 0.1$ | [link](_static/plots/qaPlot.ComCam_to_Euclid.fit.dmag_Y_EUCLID-y_ComCam.zy_ComCam.norder2.qa1.png)   |
| $y_{ComCam} \to Y_{EUCLID}$   | $Y_{EUCLID} - y_{ComCam} = +0.256 (z-y)_{ComCam}^2 -0.588 (z-y)_{ComCam} -0.071$   | 0.03  | $0.1 < (z-y)_{ComCam} \leq 0.7$  | [link](_static/plots/qaPlot.ComCam_to_Euclid.fit.dmag_Y_EUCLID-y_ComCam.zy_ComCam.norder2.qa1.png)   |
|      &nbsp;              |                                                    |       |                               |       |
| $y_{ComCam} \to H_{EUCLID}$   | $H_{EUCLID} - y_{ComCam} = -41.687 (z-y)_{ComCam}^2 -3.080 (z-y)_{ComCam} +0.178$  | 0.106 | $-0.2 < (z-y)_{ComCam} \leq 0.1$ | [link](_static/plots/qaPlot.ComCam_to_Euclid.fit.dmag_H_EUCLID-y_ComCam.zy_ComCam.norder2.qa1.png)   |
| $y_{ComCam} \to H_{EUCLID}$   | $H_{EUCLID} - y_{ComCam} = -0.024 (z-y)_{ComCam}^2 -0.569 (z-y)_{ComCam} -0.235$   | 0.06  | $0.1 < (z-y)_{ComCam} \leq 0.7$  | [link](_static/plots/qaPlot.ComCam_to_Euclid.fit.dmag_H_EUCLID-y_ComCam.zy_ComCam.norder2.qa1.png)   |
|      &nbsp;              |                                                    |       |                               |       |
| $y_{ComCam} \to J_{EUCLID}$   | $J_{EUCLID} - y_{ComCam} = -27.204 (z-y)_{ComCam}^2 -1.564 (z-y)_{ComCam} +0.020$  | 0.071 | $-0.2 < (z-y)_{ComCam} \leq 0.1$ | [link](_static/plots/qaPlot.ComCam_to_Euclid.fit.dmag_J_EUCLID-y_ComCam.zy_ComCam.norder2.qa1.png)   |
| $y_{ComCam} \to J_{EUCLID}$   | $J_{EUCLID} - y_{ComCam} = +0.490 (z-y)_{ComCam}^2 -0.889 (z-y)_{ComCam} -0.165$   | 0.04  | $0.1 < (z-y)_{ComCam} \leq 0.7$  | [link](_static/plots/qaPlot.ComCam_to_Euclid.fit.dmag_J_EUCLID-y_ComCam.zy_ComCam.norder2.qa1.png)   |
|      &nbsp;              |                                                    |       |                               |       |


#### 1.3.3 ComCam <--> Johnson-Cousins UBVRcIc

| Conversion         | Transformation Equation                         |   RMS | Applicable Color Range          | QA Plot                                                                                      |
|:-------------------|:------------------------------------------------|------:|:--------------------------------|:---------------------------------------------------------------------------------------------|
| $u_{ComCam} \to U$ | $U - u_{ComCam} = +0.057 (g-i)_{ComCam} -0.687$ | 0.088 | $0.2 < (g-i)_{ComCam} \leq 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_U-u_ComCam.gi_ComCam.norder1.qa1.png) |
| $g_{ComCam} \to B$ | $B - g_{ComCam} = +0.284 (g-i)_{ComCam} +0.215$ | 0.024 | $0.2 < (g-i)_{ComCam} \leq 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_B-g_ComCam.gi_ComCam.norder1.qa1.png) |
| $g_{ComCam} \to V$ | $V - g_{ComCam} = -0.302 (g-i)_{ComCam} -0.041$ | 0.022 | $0.2 < (g-i)_{ComCam} \leq 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_V-g_ComCam.gi_ComCam.norder1.qa1.png) |
| $r_{ComCam} \to R$ | $R - r_{ComCam} = -0.095 (g-i)_{ComCam} -0.142$ | 0.015 | $0.2 < (g-i)_{ComCam} \leq 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_R-r_ComCam.gi_ComCam.norder1.qa1.png) |
| $i_{ComCam} \to I$ | $I - i_{ComCam} = -0.074 (g-i)_{ComCam} -0.359$ | 0.016 | $0.2 < (g-i)_{ComCam} \leq 2.5$ | [link](_static/plots/qaPlot.ComCam_to_Stetson.fit.dmag_I-i_ComCam.gi_ComCam.norder1.qa1.png) |


| Conversion         | Transformation Equation                |   RMS | Applicable Color Range   | QA Plot                                                                               |
|:-------------------|:---------------------------------------|------:|:-------------------------|:--------------------------------------------------------------------------------------|
| $V \to g_{ComCam}$ | $g_{ComCam} - V = +0.503 (B-V) -0.082$ | 0.017 | $0.3 < (B-V) \leq 1.7$   | [link](_static/plots/qaPlot.Stetson_to_ComCam.fit.dmag_g_ComCam-V.BV.norder1.qa1.png) |
| $R \to r_{ComCam}$ | $r_{ComCam} - R = +0.223 (R-I) +0.122$ | 0.014 | $0.2 < (R-I) \leq 1.4$   | [link](_static/plots/qaPlot.Stetson_to_ComCam.fit.dmag_r_ComCam-R.RI.norder1.qa1.png) |
| $I \to i_{ComCam}$ | $i_{ComCam} - I = +0.178 (R-I) +0.341$ | 0.01  | $0.2 < (R-I) \leq 1.4$   | [link](_static/plots/qaPlot.Stetson_to_ComCam.fit.dmag_i_ComCam-I.RI.norder1.qa1.png) |
| $I \to z_{ComCam}$ | $z_{ComCam} - I = -0.300 (R-I) +0.456$ | 0.013 | $0.2 < (R-I) \leq 1.4$   | [link](_static/plots/qaPlot.Stetson_to_ComCam.fit.dmag_z_ComCam-I.RI.norder1.qa1.png) |
| $I \to y_{ComCam}$ | $y_{ComCam} - I = -0.559 (R-I) +0.522$ | 0.026 | $0.2 < (R-I) \leq 1.4$   | [link](_static/plots/qaPlot.Stetson_to_ComCam.fit.dmag_y_ComCam-I.RI.norder1.qa1.png) |   


#### 1.3.4 ComCam <--> GAIA DR3

| Conversion                 | Transformation Equation                                                         |   RMS | Applicable Color Range           | QA Plot                                                                                            |
|:---------------------------|:--------------------------------------------------------------------------------|------:|:---------------------------------|:---------------------------------------------------------------------------------------------------|
| $g_{ComCam} \to G_{gaia}$  | $G_{gaia} - g_{ComCam} = -0.033 (g-i)_{ComCam}^2 -0.610 (g-i)_{ComCam} -0.054$  | 0.008 | $-0.7 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_GaiaDR3.fit.dmag_G_gaia-g_ComCam.gi_ComCam.norder2.qa1.png)  |
| $g_{ComCam} \to BP_{gaia}$ | $BP_{gaia} - g_{ComCam} = +0.081 (g-i)_{ComCam}^2 -0.449 (g-i)_{ComCam} +0.162$ | 0.027 | $-0.7 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_GaiaDR3.fit.dmag_BP_gaia-g_ComCam.gi_ComCam.norder2.qa1.png) |
| $r_{ComCam} \to RP_{gaia}$ | $RP_{gaia} - r_{ComCam} = -0.233 (g-i)_{ComCam}^2 +0.146 (g-i)_{ComCam} -0.500$ | 0.036 | $-0.7 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_GaiaDR3.fit.dmag_RP_gaia-r_ComCam.gi_ComCam.norder2.qa1.png) |


| Conversion                | Transformation Equation                                                        |   RMS | Applicable Color Range           | QA Plot                                                                                            |
|:--------------------------|:-------------------------------------------------------------------------------|------:|:---------------------------------|:---------------------------------------------------------------------------------------------------|
| $G_{gaia} \to g_{ComCam}$ | $g_{ComCam} - G_{gaia} = -0.079 (BP-RP)_{gaia}^2 +1.113 (BP-RP)_{gaia} -0.480$ | 0.029 | $-0.2 < (BP-RP)_{gaia} \leq 3.1$ | [link](_static/plots/qaPlot.GaiaDR3_to_ComCam.fit.dmag_g_ComCam-G_gaia.BP_RP_gaia.norder2.qa1.png) |
| $G_{gaia} \to r_{ComCam}$ | $r_{ComCam} - G_{gaia} = +0.305 (BP-RP)_{gaia}^2 -0.711 (BP-RP)_{gaia} +0.363$ | 0.014 | $-0.2 < (BP-RP)_{gaia} \leq 3.1$ | [link](_static/plots/qaPlot.GaiaDR3_to_ComCam.fit.dmag_r_ComCam-G_gaia.BP_RP_gaia.norder2.qa1.png) |
| $G_{gaia} \to i_{ComCam}$ | $i_{ComCam} - G_{gaia} = +0.099 (BP-RP)_{gaia}^2 -0.672 (BP-RP)_{gaia} +0.343$ | 0.009 | $-0.2 < (BP-RP)_{gaia} \leq 3.1$ | [link](_static/plots/qaPlot.GaiaDR3_to_ComCam.fit.dmag_i_ComCam-G_gaia.BP_RP_gaia.norder2.qa1.png) |
| $G_{gaia} \to z_{ComCam}$ | $z_{ComCam} - G_{gaia} = +0.034 (BP-RP)_{gaia}^2 -0.747 (BP-RP)_{gaia} +0.416$ | 0.014 | $-0.2 < (BP-RP)_{gaia} \leq 3.1$ | [link](_static/plots/qaPlot.GaiaDR3_to_ComCam.fit.dmag_z_ComCam-G_gaia.BP_RP_gaia.norder2.qa1.png) |
| $G_{gaia} \to y_{ComCam}$ | $y_{ComCam} - G_{gaia} = +0.041 (BP-RP)_{gaia}^2 -0.906 (BP-RP)_{gaia} +0.527$ | 0.014 | $-0.2 < (BP-RP)_{gaia} \leq 3.1$ | [link](_static/plots/qaPlot.GaiaDR3_to_ComCam.fit.dmag_y_ComCam-G_gaia.BP_RP_gaia.norder2.qa1.png) |


#### 1.3.5 ComCam <--> SDSS DR18

| Conversion                  | Transformation Equation                                |   RMS | Applicable Color Range        | QA Plot                                                                                        |
|:----------------------------|:-------------------------------------------------------|------:|:------------------------------|:-----------------------------------------------------------------------------------------------|
| $g_{sdss} \to g_{ComCam}$   | $g_{ComCam} - g_{sdss} = -0.062 (g-i)_{sdss} +0.007$   | 0.015 | $0.2 < (g-i)_{sdss} \leq 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_g_ComCam-g_sdss.gi_sdss.norder1.qa1.png)   |
| $r_{sdss} \to r_{ComCam}$   | $r_{ComCam} - r_{sdss} = -0.007 (g-i)_{sdss} +0.005$   | 0.011 | $0.2 < (g-i)_{sdss} \leq 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_r_ComCam-r_sdss.gi_sdss.norder1.qa1.png)   |
| $i_{sdss} \to i_{ComCam}$   | $i_{ComCam} - i_{sdss} = -0.010 (g-i)_{sdss} +0.019$   | 0.012 | $0.2 < (g-i)_{sdss} \leq 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_i_ComCam-i_sdss.gi_sdss.norder1.qa1.png)   |
| $z_{sdss} \to z_{ComCam}$   | $z_{ComCam} - z_{sdss} = +0.020 (g-i)_{sdss} +0.032$   | 0.017 | $0.2 < (g-i)_{sdss} \leq 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_z_ComCam-z_sdss.gi_sdss.norder1.qa1.png)   |
| $(g-i)_{sdss} \to (g-i)_{ComCam}$ | $(g-i)_{ComCam} = +0.936 (g-i)_{sdss} -0.002$ | 0.021 | $0.2 < (g-i)_{sdss} \leq 3.1$ | [link](_static/plots/qaPlot.SDSS_to_ComCam.fit.dmag_gi_ComCam-gi_sdss.gi_sdss.norder1.qa1.png) |


| Conversion                  | Transformation Equation                                  |   RMS | Applicable Color Range          | QA Plot                                                                                          |
|:----------------------------|:---------------------------------------------------------|------:|:--------------------------------|:-------------------------------------------------------------------------------------------------|
| $g_{ComCam} \to g_{sdss}$   | $g_{sdss} - g_{ComCam} = +0.066 (g-i)_{ComCam} -0.006$   | 0.015 | $0.2 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_SDSS.fit.dmag_g_sdss-g_ComCam.gi_ComCam.norder1.qa1.png)   |
| $r_{ComCam} \to r_{sdss}$   | $r_{sdss} - r_{ComCam} = +0.007 (g-i)_{ComCam} -0.005$   | 0.011 | $0.2 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_SDSS.fit.dmag_r_sdss-r_ComCam.gi_ComCam.norder1.qa1.png)   |
| $i_{ComCam} \to i_{sdss}$   | $i_{sdss} - i_{ComCam} = +0.012 (g-i)_{ComCam} -0.020$   | 0.012 | $0.2 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_SDSS.fit.dmag_i_sdss-i_ComCam.gi_ComCam.norder1.qa1.png)   |
| $z_{ComCam} \to z_{sdss}$   | $z_{sdss} - z_{ComCam} = -0.022 (g-i)_{ComCam} -0.031$   | 0.017 | $0.2 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_SDSS.fit.dmag_z_sdss-z_ComCam.gi_ComCam.norder1.qa1.png)   |
| $(g-i)_{ComCam} \to (g-i)_{sdss}$ | $(g-i)_{sdss} = +1.065 (g-i)_{ComCam} +0.005$ | 0.023 | $0.2 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_SDSS.fit.dmag_gi_sdss-gi_ComCam.gi_ComCam.norder1.qa1.png) |

#### 1.3.6 ComCam <--> PanStarrs1 DR2

| Conversion               | Transformation Equation                               |   RMS | Applicable Color Range           | QA Plot                                                                                         |
|:-------------------------|:------------------------------------------------------|------:|:---------------------------------|:------------------------------------------------------------------------------------------------|
| $g_{ComCam} \to g_{ps1}$ | $g_{ps1} - g_{ComCam} = -0.035 (g-i)_{ComCam} -0.019$ | 0.014 | $-2.2 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_g_ps1-g_ComCam.gi_ComCam.norder1.qa1.png) |
| $r_{ComCam} \to r_{ps1}$ | $r_{ps1} - r_{ComCam} = +0.004 (g-i)_{ComCam} -0.001$ | 0.011 | $-2.2 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_r_ps1-r_ComCam.gi_ComCam.norder1.qa1.png) |
| $i_{ComCam} \to i_{ps1}$ | $i_{ps1} - i_{ComCam} = +0.014 (g-i)_{ComCam} -0.016$ | 0.01  | $-2.2 < (g-i)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_i_ps1-i_ComCam.gi_ComCam.norder1.qa1.png) |
| $z_{ComCam} \to z_{ps1}$ | $z_{ps1} - z_{ComCam} = +0.020 (i-z)_{ComCam} +0.003$ | 0.009 | $-1.8 < (i-z)_{ComCam} \leq 1.5$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_z_ps1-z_ComCam.iz_ComCam.norder1.qa1.png) |
| $y_{ComCam} \to y_{ps1}$ | $y_{ps1} - y_{ComCam} = +0.057 (z-y)_{ComCam} -0.012$ | 0.013 | $-1.7 < (z-y)_{ComCam} \leq 3.0$ | [link](_static/plots/qaPlot.ComCam_to_PS1DR2.fit.dmag_y_ps1-y_ComCam.zy_ComCam.norder1.qa1.png) |


| Conversion               | Transformation Equation                            |   RMS | Applicable Color Range        | QA Plot                                                                                      |
|:-------------------------|:---------------------------------------------------|------:|:------------------------------|:---------------------------------------------------------------------------------------------|
| $g_{ps1} \to g_{ComCam}$ | $g_{ComCam} - g_{ps1} = +0.036 (g-i)_{ps1} +0.019$ | 0.015 | $-0.7 < (g-i)_{ps1} \leq 2.9$ | [link](_static/plots/qaPlot.PS1DR2_to_ComCam.fit.dmag_g_ComCam-g_ps1.gi_ps1.norder1.qa1.png) |
| $r_{ps1} \to r_{ComCam}$ | $r_{ComCam} - r_{ps1} = -0.004 (g-i)_{ps1} +0.001$ | 0.011 | $-0.7 < (g-i)_{ps1} \leq 2.9$ | [link](_static/plots/qaPlot.PS1DR2_to_ComCam.fit.dmag_r_ComCam-r_ps1.gi_ps1.norder1.qa1.png) |
| $i_{ps1} \to i_{ComCam}$ | $i_{ComCam} - i_{ps1} = -0.014 (g-i)_{ps1} +0.016$ | 0.01  | $-0.7 < (g-i)_{ps1} \leq 2.9$ | [link](_static/plots/qaPlot.PS1DR2_to_ComCam.fit.dmag_i_ComCam-i_ps1.gi_ps1.norder1.qa1.png) |
| $z_{ps1} \to z_{ComCam}$ | $z_{ComCam} - z_{ps1} = -0.017 (i-z)_{ps1} -0.004$ | 0.01  | $-0.3 < (i-z)_{ps1} \leq 0.8$ | [link](_static/plots/qaPlot.PS1DR2_to_ComCam.fit.dmag_z_ComCam-z_ps1.iz_ps1.norder1.qa1.png) |
| $y_{ps1} \to y_{ComCam}$ | $y_{ComCam} - y_{ps1} = -0.040 (z-y)_{ps1} +0.010$ | 0.014 | $-0.3 < (z-y)_{ps1} \leq 0.5$ | [link](_static/plots/qaPlot.PS1DR2_to_ComCam.fit.dmag_y_ComCam-y_ps1.zy_ps1.norder1.qa1.png) |


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

#### 2.2.1 ComCam <--> DES DR2

##### 2.2.1.1 Original

:::{table} ComCam DP1 to DES DR2 (Version `v_2025_08_22`).
:widths: auto

| Transformation Relation    | RMS      | Applicable Color Range      | QA Plot  | Lookup Table  |
| :--------------------------| -------: | --------------------------: | -------: | ------------: |
| ComCam g,g-i to DES g      |  0.010   | $0.2 < (g-i)_{DES} < 3.0$   | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.g_gi_ComCam.png)     | [link](_static/data/transInterp.ComCam_to_des.g_gi_ComCam.csv) |
| ComCam r,r-i to DES r      |  0.007   | $0.0 < (r-i)_{DES} < 1.7$   | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.r_ri_ComCam.png)     | [link](_static/data/transInterp.ComCam_to_des.r_ri_ComCam.csv) |
| ComCam i,i-z to DES i      |  0.006   | $-0.1 < (i-z)_{DES} < 0.8$  | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.i_iz_ComCam.png)     | [link](_static/data/transInterp.ComCam_to_des.i_iz_ComCam.csv) |
| ComCam z,i-z to DES z      |  0.007   | $-0.1 < (i-z)_{DES} < 0.8$  | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.z_iz_ComCam.png)     | [link](_static/data/transInterp.ComCam_to_des.z_iz_ComCam.csv) |
:::

##### 2.2.1.2 Updated

| Conversion               |   RMS | Applicable Color Range    | QA Plot                                                                             | Lookup Table                                                                |
|:-------------------------|------:|:--------------------------|:------------------------------------------------------------------------------------|:----------------------------------------------------------------------------|
| $g_{des} \to g_{ComCam}$ | 0.01  | $0.2 < (g-i)_{des} < 3.3$ | [link](_static/plots/qaPlot_transInterp.DESDR2_to_ComCam_ECDFS.g_ComCam_gi_des.png) | [link](_static/data/transInterp.DESDR2_to_ComCam_ECDFS.g_ComCam_gi_des.csv) |
| $r_{des} \to r_{ComCam}$ | 0.008 | $0.2 < (g-i)_{des} < 3.3$ | [link](_static/plots/qaPlot_transInterp.DESDR2_to_ComCam_ECDFS.r_ComCam_gi_des.png) | [link](_static/data/transInterp.DESDR2_to_ComCam_ECDFS.r_ComCam_gi_des.csv) |
| $i_{des} \to i_{ComCam}$ | 0.008 | $0.2 < (g-i)_{des} < 3.3$ | [link](_static/plots/qaPlot_transInterp.DESDR2_to_ComCam_ECDFS.i_ComCam_gi_des.png) | [link](_static/data/transInterp.DESDR2_to_ComCam_ECDFS.i_ComCam_gi_des.csv) |
| $z_{des} \to z_{ComCam}$ | 0.007 | $0.2 < (g-i)_{des} < 3.3$ | [link](_static/plots/qaPlot_transInterp.DESDR2_to_ComCam_ECDFS.z_ComCam_gi_des.png) | [link](_static/data/transInterp.DESDR2_to_ComCam_ECDFS.z_ComCam_gi_des.csv) |
| $Y_{des} \to y_{ComCam}$ | 0.009 | $0.2 < (g-i)_{des} < 3.3$ | [link](_static/plots/qaPlot_transInterp.DESDR2_to_ComCam_ECDFS.y_ComCam_gi_des.png) | [link](_static/data/transInterp.DESDR2_to_ComCam_ECDFS.y_ComCam_gi_des.csv) |


| Conversion               |   RMS | Applicable Color Range       | QA Plot                                                                             | Lookup Table                                                                |
|:-------------------------|------:|:-----------------------------|:------------------------------------------------------------------------------------|:----------------------------------------------------------------------------|
| $g_{ComCam} \to g_{des}$ | 0.01  | $0.2 < (g-i)_{ComCam} < 3.1$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_DESDR2_ECDFS.g_des_gi_ComCam.png) | [link](_static/data/transInterp.ComCam_to_DESDR2_ECDFS.g_des_gi_ComCam.csv) |
| $r_{ComCam} \to r_{des}$ | 0.009 | $0.2 < (g-i)_{ComCam} < 3.1$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_DESDR2_ECDFS.r_des_gi_ComCam.png) | [link](_static/data/transInterp.ComCam_to_DESDR2_ECDFS.r_des_gi_ComCam.csv) |
| $i_{ComCam} \to i_{des}$ | 0.008 | $0.2 < (g-i)_{ComCam} < 3.1$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_DESDR2_ECDFS.i_des_gi_ComCam.png) | [link](_static/data/transInterp.ComCam_to_DESDR2_ECDFS.i_des_gi_ComCam.csv) |
| $z_{ComCam} \to z_{des}$ | 0.007 | $0.2 < (g-i)_{ComCam} < 3.1$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_DESDR2_ECDFS.z_des_gi_ComCam.png) | [link](_static/data/transInterp.ComCam_to_DESDR2_ECDFS.z_des_gi_ComCam.csv) |
| $y_{ComCam} \to Y_{des}$ | 0.009 | $0.2 < (g-i)_{ComCam} < 3.1$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_DESDR2_ECDFS.Y_des_gi_ComCam.png) | [link](_static/data/transInterp.ComCam_to_DESDR2_ECDFS.Y_des_gi_ComCam.csv) |



#### 2.2.2 ComCam <--> Euclid

| Conversion                    |   RMS | Applicable Color Range        | QA Plot                                                                            | Lookup Table                                                               |
|:------------------------------|------:|:------------------------------|:-----------------------------------------------------------------------------------|:---------------------------------------------------------------------------|
| $r_{ComCam} \to VIS_{EUCLID}$ | 0.038 | $0.3 < (g-i)_{ComCam} < 3.3$  | [link](_static/plots/qaPlot_transInterp.ComCam_to_Euclid.VIS_EUCLID_gi_ComCam.png) | [link](_static/data/transInterp.ComCam_to_Euclid.VIS_EUCLID_gi_ComCam.csv) |
| $y_{ComCam} \to Y_{EUCLID}$   | 0.031 | $-0.1 < (z-y)_{ComCam} < 0.5$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_Euclid.Y_EUCLID_zy_ComCam.png)   | [link](_static/data/transInterp.ComCam_to_Euclid.Y_EUCLID_zy_ComCam.csv)   |
| $y_{ComCam} \to H_{EUCLID}$   | 0.068 | $-0.1 < (z-y)_{ComCam} < 0.5$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_Euclid.H_EUCLID_zy_ComCam.png)   | [link](_static/data/transInterp.ComCam_to_Euclid.H_EUCLID_zy_ComCam.csv)   |
| $y_{ComCam} \to J_{EUCLID}$   | 0.046 | $-0.1 < (z-y)_{ComCam} < 0.5$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_Euclid.J_EUCLID_zy_ComCam.png)   | [link](_static/data/transInterp.ComCam_to_Euclid.J_EUCLID_zy_ComCam.csv)   |


| Conversion                    |   RMS | Applicable Color Range         | QA Plot                                                                            | Lookup Table                                                               |
|:------------------------------|------:|:-------------------------------|:-----------------------------------------------------------------------------------|:---------------------------------------------------------------------------|
| $VIS_{EUCLID} \to g_{ComCam}$ | 0.075 | $0.1 < (VIS-Y)_{Euclid} < 1.9$ | [link](_static/plots/qaPlot_transInterp.Euclid_to_ComCam.g_ComCam_VISY_EUCLID.png) | [link](_static/data/transInterp.Euclid_to_ComCam.g_ComCam_VISY_EUCLID.csv) |
| $VIS_{EUCLID} \to r_{ComCam}$ | 0.034 | $0.1 < (VIS-Y)_{Euclid} < 1.9$ | [link](_static/plots/qaPlot_transInterp.Euclid_to_ComCam.r_ComCam_VISY_EUCLID.png) | [link](_static/data/transInterp.Euclid_to_ComCam.r_ComCam_VISY_EUCLID.csv) |
| $VIS_{EUCLID} \to i_{ComCam}$ | 0.017 | $0.1 < (VIS-Y)_{Euclid} < 1.9$ | [link](_static/plots/qaPlot_transInterp.Euclid_to_ComCam.i_ComCam_VISY_EUCLID.png) | [link](_static/data/transInterp.Euclid_to_ComCam.i_ComCam_VISY_EUCLID.csv) |
| $Y_{EUCLID} \to z_{ComCam}$   | 0.018 | $0.1 < (VIS-Y)_{Euclid} < 1.9$ | [link](_static/plots/qaPlot_transInterp.Euclid_to_ComCam.z_ComCam_VISY_EUCLID.png) | [link](_static/data/transInterp.Euclid_to_ComCam.z_ComCam_VISY_EUCLID.csv) |
| $Y_{EUCLID} \to y_{ComCam}$   | 0.045 | $-0.4 < (Y-H)_{Euclid} < 0.4$  | [link](_static/plots/qaPlot_transInterp.Euclid_to_ComCam.y_ComCam_YH_EUCLID.png)   | [link](_static/data/transInterp.Euclid_to_ComCam.y_ComCam_YH_EUCLID.csv)   |

#### 2.2.3 ComCam <--> GAIA DR3

| Conversion                 |   RMS | Applicable Color Range       | QA Plot                                                                                | Lookup Table                                                                   |
|:---------------------------|------:|:-----------------------------|:---------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------|
| $r_{ComCam} \to G_{gaia}$  | 0.014 | $0.3 < (g-i)_{ComCam} < 2.8$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_GaiaDR3_ECDFS.G_gaia_gi_ComCam.png)  | [link](_static/data/transInterp.ComCam_to_GaiaDR3_ECDFS.G_gaia_gi_ComCam.csv)  |
| $g_{ComCam} \to BP_{gaia}$ | 0.013 | $0.3 < (g-i)_{ComCam} < 2.8$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_GaiaDR3_ECDFS.BP_gaia_gi_ComCam.png) | [link](_static/data/transInterp.ComCam_to_GaiaDR3_ECDFS.BP_gaia_gi_ComCam.csv) |
| $r_{ComCam} \to RP_{gaia}$ | 0.022 | $0.3 < (g-i)_{ComCam} < 2.8$ | [link](_static/plots/qaPlot_transInterp.ComCam_to_GaiaDR3_ECDFS.RP_gaia_gi_ComCam.png) | [link](_static/data/transInterp.ComCam_to_GaiaDR3_ECDFS.RP_gaia_gi_ComCam.csv) |


| Conversion                |   RMS | Applicable Color Range       | QA Plot                                                                                  | Lookup Table                                                                     |
|:--------------------------|------:|:-----------------------------|:-----------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------|
| $G_{gaia} \to g_{ComCam}$ | 0.016 | $0.6 < (BP-RP)_{gaia} < 2.9$ | [link](_static/plots/qaPlot_transInterp.GaiaDR3_to_ComCam_ECDFS.g_ComCam_BP_RP_gaia.png) | [link](_static/data/transInterp.GaiaDR3_to_ComCam_ECDFS.g_ComCam_BP_RP_gaia.csv) |
| $G_{gaia} \to r_{ComCam}$ | 0.01  | $0.6 < (BP-RP)_{gaia} < 2.9$ | [link](_static/plots/qaPlot_transInterp.GaiaDR3_to_ComCam_ECDFS.r_ComCam_BP_RP_gaia.png) | [link](_static/data/transInterp.GaiaDR3_to_ComCam_ECDFS.r_ComCam_BP_RP_gaia.csv) |
| $G_{gaia} \to i_{ComCam}$ | 0.008 | $0.6 < (BP-RP)_{gaia} < 2.9$ | [link](_static/plots/qaPlot_transInterp.GaiaDR3_to_ComCam_ECDFS.i_ComCam_BP_RP_gaia.png) | [link](_static/data/transInterp.GaiaDR3_to_ComCam_ECDFS.i_ComCam_BP_RP_gaia.csv) |
| $G_{gaia} \to y_{ComCam}$ | 0.016 | $0.6 < (BP-RP)_{gaia} < 2.9$ | [link](_static/plots/qaPlot_transInterp.GaiaDR3_to_ComCam_ECDFS.y_ComCam_BP_RP_gaia.png) | [link](_static/data/transInterp.GaiaDR3_to_ComCam_ECDFS.y_ComCam_BP_RP_gaia.csv) |


## References

```{bibliography}
```
