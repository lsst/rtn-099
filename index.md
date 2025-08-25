# Photometric transformation relations for the Vera C. Rubin Observatory

```{abstract}
This technical note provides photometric transformation relations between the Vera C. Rubin Observatory's LSST and ComCam systems and other photometric systems. These transformations are derived using both synthetic and empirical data and are intended to support calibration and comparison across survey systems. We present both polynomial equations and lookup-table-based methods, depending on the available data and desired accuracy. The transformations are generally valid for stars with typical spectral energy distributions (SEDs), and caution should be used when applying them to objects with strong emission lines or atypical colors.
```
DOI: ***TBD***

Useful technical notes and webpages to consult:
* https://sitcomtn-162.lsst.io/ (for markdown formatting advice)
* https://sitcomtn-050.lsst.io/ (for markdown formatting advice)
* https://des.ncsa.illinois.edu/releases/dr2/dr2-docs/dr2-transformations (we wish to roughly following the format of these DES DR2 transformation relations webpages for this technical note)

## Transformation Methods

### Synthetic Transformations

Synthetic magnitudes were derived by integrating spectrophotometric spectra from the Pickles Stellar Spectra Library [Pickles:1998] with filter passband transmission curves for DES and LSST. These magnitudes were calculated using broad-band absolute magnitude definitions and processed using a Python-based fitting code to generate transformation equations. Due to the limited number of stars in the Pickles library (~100), the resulting plots are sparse but provide a consistent reference.


:::{table} DES to LSST Transformation Equations (Version `v_2025_08_22`).
:widths: auto

| Transformation Equation                                                     | RMS      | Applicable Color Range       | QA Plot  |
| :-------------------------------------------------------------------------- | -------: | ---------------------------: | -------: |
| `g_{LSST} = g_{DES} + 0.016 (g-i)_{DES} - 0.003 (g-i)^2_{DES} + 0.006`      | `0.002`  | `-1.0 < (g-i)_{DES} < 3.9`   | [link](_static/plots/qaPlot.des_to_lsst.fit.dmag_g.gi_des.norder2.qa1.png) |
| `r_{LSST} = r_{DES} + 0.185 (r-i)_{DES} - 0.015 (r-i)^2_{DES} + 0.010`      | `0.008`  | `-0.4 < (r-i)_{DES} < 2.3`   | [link](_static/plots/qaPlot.des_to_lsst.fit.dmag_r.ri_des.norder2.qa1.png) |
| `i_{LSST} = i_{DES} + 0.150 (r-i)_{DES} - 0.003 (r-i)^2_{DES} - 0.009`      | `0.005`  | `-0.4 < (r-i)_{DES} < 2.2`   | [link](_static/plots/qaPlot.des_to_lsst.fit.dmag_i.ri_des.norder2.qa1.png) |
| `z_{LSST} = z_{DES} + 0.270 (i-z)_{DES} + 0.036 (i-z)^2_{DES} - 0.003`      | `0.010`  | `-0.3 < (i-z)_{DES} < 1.8`   | [link](_static/plots/qaPlot.des_to_lsst.fit.dmag_z.iz_des.norder2.qa1.png) |
:::


   

### ComCam Transformations

ComCam data were used to derive empirical transformations between DES and LSST ComCam filters. All $S/N >$ 5 point sources in the DES footprint were selected, including quasars and non-standard stars.

:::{table} ComCam to DES Transformation Equations (Version `v_2025_08_22`).
:widths: auto

| Transformation Equation                                                             | RMS      | Applicable Color Range        | QA Plot  |
| :---------------------------------------------------------------------------------- | -------: | ----------------------------: | -------: |
| `g_{DES} = g_{ComCam} + 0.005 (g-i)_{ComCam} - 0.001`                               | `0.013   | `-0.6 < (g-i)_{ComCam} < 3.7` | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_g.gi_ComCam.norder1.qa1.png) |
| `r_{DES} = r_{ComCam} - 0.292 (i-z)_{ComCam} - 0.005 (i-z)^2_{ComCam} + 0.013`      | `0.010`  | `-0.2 < (i-z)_{ComCam} < 1.0` | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_r.iz_ComCam.norder2.qa1.png) |
| `i_{DES} = i_{ComCam} + 0.262 (i-z)_{ComCam} + 0.048 (i-z)^2_{ComCam} - 0.006`      | `0.008`  | `-0.2 < (i-z)_{ComCam} < 1.0` | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_i.iz_ComCam.norder2.qa1.png) |
| `z_{DES} = z_{ComCam} + 0.262 (i-z)_{ComCam} - 0.046 (i-z)^2_{ComCam} + 0.001`      | `0.010`  | `-0.2 < (i-z)_{ComCam} < 0.8` | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_z.iz_ComCam.norder2.qa1.png) |
:::



   

## Lookup Table (Interpolation) Transformations

Interpolation methods were used to model complex or non-linear relationships between survey measurements. These methods rely on binning color indices and computing median magnitude differences.

You can use the interpolation CSV files below to convert data from one photometric system to the other via interpolation methods. The files contain the delta_mag vs color locus in bins of (typically) 0.1-mag binsize along the color axis. Here is some python code that takes the interpolation CSV file for the transformation from ATLAS-REFCAT2 g-band to DES g-band to using the ATLAS-REFCAT2 g mag and ATLAS-REFCAT2 g-i color. The code makes use of the scipy "interpolate" routine.

```
import pandas as pd
from scipy import interpolate

# Read in interpolation file...
df_interp = pd.read_csv('transInterp.ref2_to_des.g_gi_ref2.csv')  # interpolation file for g-band

# Create linear interpolation of the median dmag vs. color bin calculated above...
response = interpolate.interp1d(df_interp.bin_label.values.astype(float), df_interp.bin_median.values, \
                                    bounds_error=False, fill_value=0., kind='linear')

# Read in file with data to be transformed...
df = pd.read_csv(inputFile)   # The following assumes a column with ATLAS-REFCAT2 in (g-i) in this file.
df['offset'] = response(df['gi_ref2'].values)
df['g_des'] = df['g_ref2'] - df['offset']
```

:::{table} ComCam DP1 to DES DR2 (Version `v_2025_08_22`).
:widths: auto

| Transformation Relation    | RMS      | Applicable Color Range      | QA Plot  | Lookup Table  |
| :--------------------------| -------: | --------------------------: | -------: | ------------: |
| ComCam g,g-i to DES g      | `0.010`  | `0.2 < (g-i)_{DES} < 3.0`   | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.g_gi_ComCam.png)     | link          |
| ComCam r,r-i to DES r      | `0.007`  | `0.0 < (r-i)_{DES} < 1.7`   | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.r_ri_ComCam.png)     | link          |
| ComCam i,i-z to DES i      | `0.006`  | `-0.1 < (i-z)_{DES} < 0.8`  | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.i_iz_ComCam.png)     | link          |
| ComCam z,i-z to DES z      | `0.007`  | `-0.1 < (i-z)_{DES} < 0.8`  | [link](_static/plots/qaPlot_transInterp.ComCam_to_des.z_iz_ComCam.png)     | link          |
:::





# References

Rubin Observatory overview paper: \citet{2019ApJ...873..111I}.




