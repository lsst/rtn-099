# Photometric transformation relations for the Vera C. Rubin Observatory

```{abstract}
This technical note provides photometric transformation relations between the Vera C. Rubin Observatory's LSST and ComCam systems and other photometric systems. These transformations are derived using both synthetic and empirical data and are intended to support calibration and comparison across survey systems. We present both polynomial equations and lookup-table-based methods, depending on the available data and desired accuracy. The transformations are generally valid for stars with typical spectral energy distributions (SEDs), and caution should be used when applying them to objects with strong emission lines or atypical colors.
```
DOI: ***TBD***

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

| Transformation Equation                                                             | RMS      | Applicable Color Range       | QA Plot  |
| :---------------------------------------------------------------------------------- | -------: | ---------------------------: | -------: |
| `g_{ComCam} = g_{DES} + 0.005 (g - i)_{ComCam} - 0.001`                             | `0.013   | `-1.0 < (g-i)_{DES} < 3.9`   | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_g.gi_des.norder1.qa1.png) |
| `r_{ComCam} = r_{DES} - 0.292 (i - z)_{ComCam} - 0.005 (i - z)^2_{ComCam} + 0.013`  | `0.010`  | `-0.4 < (i-z)_{DES} < 2.3`   | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_r.iz_des.norder2.qa1.png) |
| `i_{ComCam} = i_{DES} + 0.262 (i - z)_{ComCam} + 0.048 (i - z)^2_{ComCam} - 0.006`  | `0.008`  | `-0.4 < (i-z)_{DES} < 2.2`   | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_i.iz_des.norder2.qa1.png) |
| `z_{ComCam} = z_{DES} + 0.262 (i - z)_{ComCam} - 0.046 (i - z)^2_{ComCam} + 0.001`  | `0.010`  | `-0.3 < (i-z)_{DES} < 1.8`   | [link](_static/plots/qaPlot.ComCam_to_des.fit.dmag_z.iz_des.norder2.qa1.png) |
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

| Transformation Relation    | RMS      | Applicable Color Range  | QA Plot  | Lookup Table  |
| :--------------------------| -------: | ----------------------: | -------: | ------------: |
| ComCam g,g-i to DES g      | `0.00X`  | `X < (g-i)_{DES} < Y`   | link     | link          |
| ComCam r,r-i to DES r      | `0.00X`  | `X < (g-i)_{DES} < Y`   | link     | link          |
| ComCam i,i-z to DES i      | `0.00X`  | `X < (g-i)_{DES} < Y`   | link     | link          |
| ComCam z,i-z to DES z      | `0.01X`  | `X < (g-i)_{DES} < Y`   | link     | link          |
:::



\begin{figure}[ht]
    \centering
    \includegraphics[width=1.0\textwidth]{./Figures/qaPlot_transInterp.ComCam_to_des.g_gi_ComCam.png}
    \caption{Transformation Interpolations from ComCam-$g$ to DES-$g$ in the ($g - i$) filter color}
    \label{fig:interp-g}
\end{figure}

\begin{figure}[ht]
    \centering
    \includegraphics[width=1.0\textwidth]{./Figures/qaPlot_transInterp.ComCam_to_des.i_iz_ComCam.png}
    \caption{Transformation Interpolations from ComCam-$i$ to DES-$i$ in the ($i - z$) filter color}
    \label{fig:interp-i}
\end{figure}

\begin{figure}[ht]
    \centering
    \includegraphics[width=1.0\textwidth]{./Figures/qaPlot_transInterp.ComCam_to_des.r_ri_ComCam.png}
    \caption{Transformation Interpolations from ComCam-$r$ to DES-$r$ in the ($r - i$) filter color}
    \label{fig:interp-r}
\end{figure}

\begin{figure}[ht]
    \centering
    \includegraphics[width=1.0\textwidth]{./Figures/qaPlot_transInterp.ComCam_to_des.z_iz_ComCam.png}
    \caption{Transformation Interpolations from ComCam-$z$ to DES-$z$ in the ($i - z$) filter color}
    \label{fig:interp-z}
\end{figure}

References

This technical note follows the style and structure of the DES DR2 photometric transformation documentation: https://des.ncsa.illinois.edu/releases/dr2/dr2-docs/dr2-transformations

Rubin Observatory overview paper: \citet{2019ApJ...873..111I}.























## Cell-Based Coadds Input

At the time of this analysis, cell-based coadds are not a part of the default LSST Science Pipeline and must be generated independently. The equivalent of the pipetask command below was run on the `w_2025_17` weekly stack version of the Pipeline, along with customized branches in `drp_tasks` and `cell_coadds` using the branch `u/mirarenee/no_ap_corr`. The patches and tracts are those that fully or partially fall within 0.5 degrees of the Brightest Cluster Galaxy of A360 at RA, DEC of 37.865017, 6.982205.

The input images and catalogs used to generated the cell-based coadds and other analyses in this technote are from the LSST DRP1 ({cite:p}`RTN-095_temp`), focusing on images taken with ComCam.

```
pipetask run -j 4 --register-dataset-types  \
-b /repo/main \
-i LSSTComCam/runs/DRP/DP1/w_2025_17/DM-50530 \
-o u/$USER/ComCam_Cells/a360 \
-p /sdf/group/rubin/user/mgorsuch/ComCam/pipeline.yaml \
-d "((tract=10463 AND patch IN (30..34,40..44,50..54,60..64,70..74,80..84,90..94)) \
OR (tract=10464 AND patch IN (37..39,47..49,57..59,67..69,77..79,87..89,97..99)) \
OR (tract=10704 AND patch IN (0..5)) \
OR (tract=10705 AND patch IN (8, 9))) \
AND (band='g' OR band='r' OR band='i') AND skymap='lsst_cells_v1'"
```

The `pipeline.yaml` file used is shown below:

```yaml
description: A simple pipeline to test development of cell-based coadds in ComCam

instrument: lsst.obs.lsst.LsstComCam

tasks:
    makeDirectWarp:
        class: lsst.drp.tasks.make_direct_warp.MakeDirectWarpTask
        config:
            connections.calexp_list: preliminary_visit_image
            connections.visit_summary: visit_summary
            connections.warp: direct_warp
            connections.masked_fraction_warp: direct_warp_masked_fraction
            doWarpMaskedFraction : true
            doPreWarpInterpolation : true
    makePsfMatchedWarp:
        class: lsst.drp.tasks.make_psf_matched_warp.MakePsfMatchedWarpTask
        config:
            connections.direct_warp: direct_warp
            connections.psf_matched_warp: psf_matched_warp
    assembleDeepCoadd:
        class: lsst.drp.tasks.assemble_coadd.CompareWarpAssembleCoaddTask
        config:
            connections.inputWarps: direct_warp
            connections.psfMatchedWarps: psf_matched_warp
            doWriteArtifactMasks : true
    assembleCellCoadd:
        class: lsst.drp.tasks.assemble_cell_coadd.AssembleCellCoaddTask
        config:
            connections.inputWarps: direct_warp
            connections.visitSummaryList: visit_summary
```

There are a few reasons why there are additional tasks on top of the cell-based coaddition task, `assembleCellCoadd`. The primary input images for the cell-based coadds are warped images using the `makeDirectWarp` task. For cell-based coadds, the `doPreWarpInterpolation` configuration needs to be set to `True` manually, as the default pipeline setting is False. This `doPreWarpInterpolation` config is used to properly propagate the mask plane to the from the warps to the cell-based coadds. The `makePsfMatchedWarp` and `assembleDeepCoadd` tasks are needed to generate artifact masks, which are required inputs for the cell-based coaddition task to run.

The collections for the cell-based coadds are stored in `u/mgorsuch/a360_cell_coadd` for *r*- and *i*-bands, and `u/mgorsuch/a360_cell_coadd_g` for *g*-band (separate collections due to not running *g*-band initially).

Cell-based coadds are stored as patch-sized coadds, divided into 484 cell regions (22 by 22 cells). Individual cells have both inner and outer boundary boxes, which are 150 by 150 and 250 by 250 pixels, respectively.

Metadetection utilizes both inner and outer boundaries of cells, and does not duplicate objects from this overlap. However, there is also overlap between patches and tracts that Metadetection does not address, and this leads to duplicate objects within the object tables. Overlap between patches within the same tract is 2 cells wide, and duplicates are removed by removing objects found in the outer ring of cells of each patch. There is also significant overlap between tracts. Patches in tract 10463 that fully overlap with tract 10464 are ignored. After removing the fully overlapping patches, there’s still a 4 cell wide overlap on one side between tract 10463 and 10464. Overlapping cells are again removed. Currently, the WCS information is not enough to remove exact duplicates based on RA and DEC. Why this is the case requires further investigation.

```{figure} _static/3_band_image_distribution.png
:name: image_dist

These three figures show the input image distribution for the patches, each composed of 484 cells, around A360 in the g, r, and i-bands. The red squares outline the inner patch boundaries, where the 2 cell overlap is visible. The three missing patches are due to processing errors when running Metadetection, though do not significantly overlap with the 0.5 degree radius around the BCG (cyan circle).
```

```{figure} _static/3_band_psf_e_distribution.png
:name: ellip_dist

PSF ellipticity distribution with one PSF realization per cell for the patches around A360 in the g, r, and i-bands. The red squares outline the inner patch boundaries. The general pattern is consistent with [cite PSF technote], especially in the r- and i-band. The mean ellipticities are 0.0563, 0.0689, and 0.0987 for the g, r, and i-bands, respectively.
```

Note that for cell-based coadds, there is a single PSF model for each cell, realized at the center of the cell. The distribution of PSF ellipticities are seen in {numref}`ellip_dist`.

## Running Metadetection

Metadetection ({cite:p}`meta_hm`, {cite:p}`Sheldon_2017`, {cite:p}`Sheldon_2020`) is a shear calibration software focused on an empirical approach of artificially shearing images of galaxies to measure the response calibration matrix R, which is then applied to the unsheared images to calibrate their shear measurements. Metadetection is the sequel software to the original Metacalibration. The primary difference between the two is that while Metacalibration measures the shear response of individual objects for calibration, Metadetection is designed to detect and measure after the applied shear, resulting in 5 catalogs of shear types (non-sheared, in the plus/minus g1 direction, and in the plus/minus g2 direction). The main consequence of this is that since detection is shear-dependent, as seen in {cite:p}`Sheldon_2020`, the 5 Metadetection catalogs do not have necessarily the same objects, and cannot be matched to each other; shear is instead calibrated using the mean shape values.

The default setting for measuring object shapes is `wmom` (weighted moments), used throughout this technote. Each shape measurement is a weighted average of the second moments using the three bands, g, r, and i. The weights for averaging across bands come from the inverse variance of the image. In a similar vein, the flux measurements are the zero moment of each object. Both the zero and second moments are weighted by a Gaussian with a FWHM of 1.2 arcseconds.

Metadetection is currently being integrated into the LSST Science Pipelines as a pipeline task to fully utilize the cell-based coaddition based tasks in the pipeline structure.

The Metadetection shear catalog for this technote was run on the `w_2025_17` weekly stack version of the Pipeline, along with customized branches in `drp_tasks` and metadetect using the branch `u/mirarenee/meta_test`, since a few minor changes were needed to run Metadetection on more recent pipeline stacks.

The pipetask command used to generate the Metadetection catalog is found below:

```
pipetask run -j 4 --register-dataset-types \
-b /repo/main \
-i refcats,u/mgorsuch/a360_cell_coadd,u/mgorsuch/a360_cell_coadd_g \
-o u/$USER/metadetect/a360_3_band \
-p /sdf/group/rubin/user/mgorsuch/notebooks/metadetect/comcam_pipeline.yaml \
-d "skymap='lsst_cells_v1'"
```

The associated `comcam_pipeline.yaml` file used for defining the tasks is outline below:

```yaml
description: Pipeline for running metadetection on ComCam
instrument: lsst.obs.lsst.LsstComCam

tasks:
    metadetectionShear:
        class: lsst.drp.tasks.metadetection_shear.MetadetectionShearTask
        config:
            connections.ref_cat: the_monster_20250219
            required_bands : ["g", "r", "i"]
            python: |
               from metadetect.lsst.configs import get_config as get_mdet_config
               mdet_config = get_mdet_config()
               mdet_config['metacal']['types']=['noshear', '1p', '1m', '2p', '2m']

               config.ref_loader.filterMap = {'lsst_'+band: 'monster_ComCam_%s' % (band) for band in 'ugrizy'}
```

Currently, with the custom branches, the only python line needed in the pipeline file is the filter map. The custom branches have a workaround in place for changing Metadetection specific configs for the time being, though the additional python lines should be sufficient for changing those configs through the pipeline file in the future.

An important note is that, for this analysis, a single noise image is generated for each cell within Metadetection. The noise image is generated from a Gaussian distribution with a standard deviation equal to the median variance of the image. It’s possible to produce multiple noise images prior to the warping process in order to better account for noise correlation across pixels, though this extra warping is computationally expensive and skipped for now.

Prior to Metadetection flags (i.e. objects cut due to measurement failures), there are 435599 rows after removing duplicate objects due to overlap between patches and tracts. With duplicates removed, there are 81099 objects with Metadetection flags, about 19% of objects. Once all flagged objects are removed (which removes all nan values), there are 354500 remaining objects. Note that this total number is still the combination of all 5 shear type catalogs, with 20% (70902) of the objects being in the non-sheared catalog. The number of flagged objects is quite high, and requires further investigation.

## Red Sequence Galaxy Identification

Cluster member galaxies of the lensing cluster structure will not have a lensing signal (at least not from the cluster itself). Due to this, these galaxies need to be identified and removed from the lensing sample to avoid diluting the shear signal [citation?]. These lensing galaxies are primarily identified through visual inspection using color-magnitude plots across three different bands.

```{figure} _static/object-magnitudes.png
:name: obj-mags

The distribution of object magnitudes, from Metadetection flux measurements, in each of the bands used. This distribution is after Metadetection flagged objects are cut, though prior to red sequence galaxy identification and selection cuts.The red vertical lines are the current magnitude cuts at the limiting magnitude, determined by eye.
```

A series of color-magnitude plots with progressive cuts is used to identify the red sequence (RS) galaxies. Each cut is applied to the entire catalog, though only the non-sheared catalog is shown in the color-magnitude plots. Previous visual inspection showed that while there is some variation in  between shear type catalogs, the variation is minimal and random enough that applying the same cuts should be sufficient for this analysis.

The catalog is first cut to galaxies less than 0.1 degree away from the BCG to focus on galaxies that are more likely to be cluster members. The red sequence cluster members are identified in a line of objects with relatively consistent color across a range of magnitudes, with the line being more apparent in the smaller sample of galaxies. This line of galaxies is highlighted with orange points, with the upper and lower limits shown in red. The same visual inspection is done again for the larger sample of galaxies, those within 0.5 degrees of the BCG. Within the larger sample, the objects identified within the limits of either the g-r or r-i RS galaxy limits are cut, leaving a sample of galaxies for measuring weak lensing. This sample still includes bright foreground galaxies, though these should not bias the signal as their shape distribution should be random without the lensing from the cluster. Still, bright galaxies are cut, as seen in the selection cut section.

```{figure} _static/color-magnitude-0-1.png
:name: color-magnitude-0-1

Color-magnitude diagram cut to 0.1 degrees within the BCG.
```

```{figure} _static/color-magnitude-0-1-orange.png
:name: color-magnitude-0-1-orange

Color-magnitude diagram cut to 0.1 degrees within the BCG. Objects that are included within the cut are highlighted in orange.
```

```{figure} _static/color-magnitude-0-5-no-line.png
:name: color-magnitude-0-5-no-line

Color-magnitude diagram cut to 0.5 degrees within the BCG.
```

```{figure} _static/color-magnitude-0-5-orange.png
:name: color-magnitude-0-5-orange

Color-magnitude diagram cut to 0.5 degrees within the BCG. Objects that are included within the cut are highlighted in orange.
```

```{figure} _static/color-magnitude-0-5-removed.png
:name: color-magnitude-0-5-removed

Color-magnitude diagram cut to 0.5 degrees within the BCG. Objects that are included within the cut are removed.
```

After applying the 0.5 degree cut, but prior to removing RS objects, there are 217527 object rows. After applying the RS cuts, there are 152838, with 30532 (20%) of which are the non-sheared catalog.

## Selection Cuts

After Metadetection flags are applied and red sequence galaxies are identified and removed, additional selection cuts are applied. These cuts are primarily based on {cite:p}`yamamoto`, though are made slightly less stringent in order to increase the number of objects used in this relatively small region around the cluster. In numerical terms, the number of objects remaining after selection cuts is 46447, with 9353 (20%) in the non-sheared catalog. Of particular note is the signal-to-noise cut, which doesn’t remove any rows, despite that being  a large cut of the total rows in {cite:p}`yamamoto`. The missing low S/N objects are another quality that requires more investigation.

The Yamamoto cuts describe a size ratio cut, defined as the size of the object squared divided by size of the PSF squared, or $T^{gauss}/T^{gauss}_{PSF}$. This is used as a star-galaxy cut. For the Yamamoto measurements, these sizes are measured for the pre-PSF objects, so stars will hover around 0 for this ratio. Meanwhile, the weighted moments measurements with Metadetection for T and T_PSF are both measured after the reconvolution step, and will be slightly larger; stars will hover closer to 1. The Metadetection paper {cite:p}`Sheldon_2023` uses a cut of 1.2 for this size ratio, and indicates that while the inclusion of stars might introduce a bias, it’s quite small.

Based on {numref}`obj_T_vs_s2n`, the stars identified around `wmom_T_ratio` = 1 appear to fall consistently below `wmom_T_ratio` = 1.1. This value is chosen instead for the `wmom_T_ratio` cut, since the inclusion of extra galaxies in the weak lensing sample outweighs the potential inclusion of some low S/N stars.

The magnitude cuts also deviate slightly from {cite:p}`yamamoto`. These cuts are based on the estimated  limiting magnitude of each band, as seen from {numref}`obj-mags`.

:::{table} Each cut applied to the Metadetection catalog after the RS cuts. Note that the number of rows removed is for each individual cut, so cuts may overlap with other cuts.
:widths: auto

| Selection Cut                         | Rows Removed | Fraction Removed |
| :------------------------------------ | -----------: | ---------------: |
| `wmom_T_ratio` > 1.1                    | 59426        | 38.9%            |
| `wmom_s2n` > 10                         | 0            | 0.0%             |
| `wmom_T` < 20                           | 0            | 0.0%             |
| `mfrac` < 0.1                           | 0            | 0.0%             |
| `wmom_band_mag_g` < 24.7                | 52355        | 34.3%            |
| `wmom_band_mag_r` < 24.2                | 26595        | 17.4%            |
| `wmom_band_mag_i` < 23.6                | 37795        | 24.7%            |
| `wmom_band_mag_i` > 20 (brightness cut) | 9662         | 6.3%             |
| `wmom_color_mag_g-r` (abs. value) < 5   | 857          | 0.6%             |
| `wmom_color_mag_r-i` (abs. value) < 5   | 209          | 0.1%             |
| `wmom_T` < 0.425 - 2.0*`wmom_T_err`       | 5352         | 3.5%             |
| `wmom_T` * `wmom_T_err` < 0.006           | 181          | 0.1%             |
:::

For reference against another catalog, it's useful to at the number of objects found in the HSM catalog ({cite:p}`HSM1`, {cite:p}`HSM2`) after different cuts. The HSM catalog first reads in 183791 objects prior to any cuts. After RS cuts, there are 104257 objects. Finally, after selection cuts, the final HSM source galaxy sample is 24362 objects. With the final `Metedetection` source galaxy sample catalog at 9353 for non-sheared objects, HSM is producing over two times as many objects. This comparison is another sign that the low number of usable objects in the Metadetection catalog needs investigation.

## Shear Calibration

Understanding the relationship between the shear applied to an object and the effect of that shear on measuring the object’s shape is a critical step to calibrating the catalog’s shear measurements. The main purpose of Metadetection’s 5 sub-catalogs is to calculate the linear response matrix, R.

The main assumption is that the weak lensing shear signal is small enough that we can Taylor expand the measured galaxy ellipticity about a zero shear signal. The first term goes to 0 in the limit of a large enough sample of galaxies where the shape noise averages out. The relation between the measured galaxy ellipticity and the resulting shear signal is controlled by R, the linear response of the ellipticity to an applied shear. Within the Metadetection framework, the components of R can be calculated by taking the mean measured galaxy ellipticities of the 4 artificially shear object catalogs. The magnitude of the applied shear in each catalog is 0.01, to make $\Delta\gamma_j$ a total of 0.02 for each component of R. Once R is calculated, it’s applied to the mean non-sheared object ellipticities to produce the calibrated shear.

$$
\begin{align}
\mathbf{e}&\approx\mathbf{e}|_{\gamma=0}+ \frac{\partial\mathbf{e}}{\partial\mathbf{\gamma}}\bigg\rvert_{\gamma=0} \\
\mathbf{e}&\approx\cancel{\langle\mathbf{e}\rangle|_{\gamma=0}}+\langle\mathbf{R}\mathbf{\gamma}\rangle \\
\langle\mathbf{\gamma}\rangle&\approx\langle\mathbf{R}^{-1}\rangle\langle\mathbf{e}\rangle \\
\langle R_{ij}\rangle&=\frac{\langle e_i^+\rangle-\langle e_i^-\rangle}{\Delta\gamma_j}
\end{align}
$$ (eqn1)

In the specific case of cluster lensing, we are more interested in the tangential and cross shears around the cluster, split into radial bins. The response matrix R is first calculated on all galaxy ellipticities measurements from the four sheared catalogs, after all applied cuts, but prior to binning in order to improve the uncertainty of R. Then for each bin, the mean tangential and cross ellipticities are calculated from the uncalibrated galaxy ellipticity measurements in the non-sheared catalog. The response matrix R is then applied to the mean tangential and cross ellipticities to produce the calibrated tangential and cross shears for the bin.

### Shear Results

The resulting shear profile is shown below in {numref}`shear-final`. Each bin is calculated from the calibrated shapes of galaxies in six bins that range from 0.32 Mpc to 6.39 Mpc, split evenly in $\log_{10}$ space. The x-axis shear profile points are calculated from the mean Mpc distance of each object for each bin. The error bars for all shear profiles are bootstrapped samples of each radial bin with 95% confidence levels, which is then calibrated with R. From smallest radial separation to largest, the number of galaxies in each bin are 63, 169, 377, 828, 2028, and 5818 galaxies. The three innermost bins are likely struggling due to the intense blending near the cluster field.

The Mpc distances are assuming a cluster redshift of z=0.22 {cite:p}`a360_z`.

```{figure} _static/shear-final.png
:name: shear-final

The reduced shear profile around A360 for both tangential and cross shear measurements, using the cuts described throughout the technote. Both measured profiles have 95% confidence intervals.
```

The theoretical shear profile is produced using Cluster Lensing Mass Modeling (CLMM) code {cite:p}`clmm`. This profile is purely for a rough reference, and is not fit to the calibrated shear data. The profile is using an NFW halo with an estimated cluster mass of 4e14 solar masses ({cite:p}`a360_mass`) and a concentration of 4. The source redshift distribution is based off of the DESC Science Requirements Document (SRD, {cite:p}`desc-srd`).

To see if more galaxies were needed in the sample to better characterize R, Metadetection was additionally run on the entire Rubin SV 38 7 field. The errors on the components of R mildly improve, though little difference is seen in the shear profile.

```{figure} _static/shear-all-cal.png
:name: shear-all-cal

Shear profile using the same data as {numref}`shear-final`, except R is calculated from all galaxies in the Rubin SV 38 7 field produced by Metadetection.
```

:::{table} Values of the R components used to calibrate the shape measurements.
:widths: auto

|                   | Galaxies < 0.5 degrees | Galaxies in SV 38 7 |
| :---------------- | ---------------------: | ------------------: |
| R_11              | 0.2391                 | 0.2070              |
| R_22              | 0.1796                 | 0.2204              |
| R_11_err          | 0.000935               | 0.0007604           |
| R_22_err          | 0.000963               | 0.0007670           |
| \| R_11 - R_22 \| | 0.00595                | 0.01336             |
:::

## Validation & Testing

This section contains a non-exhaustive list of notes and figures on characterizing the Metadetection output catalog and the effects from various cuts.

### Object Size and S/N Cuts

The measured object size compared to the signal-to-noise ratio is a simple cut to differentiate stars and galaxies.

```{figure} _static/obj_T_vs_s2n.png
:name: obj_T_vs_s2n

Left: relationship between the object size ratio and the S/N of each object prior to selection cuts, though after red sequence galaxy removal. The red line is a visual reference to see what objects are removed by the 1.1 object size ratio cut. Stars are expected to fall near an object ratio of 1, which is seen clearly for high S/N objects. Right. Distribution of objects after the object size ratio cut (and other additional cuts). The line of stars is removed, though some low S/N stars may survive the cut, as those tend to have higher size uncertainties as seen in {cite:p}`yamamoto`.
```

### Angular Correlations of PSF Ellipticities

The version of cell-based coadds used here is not tested on downstream tasks, which is beyond the scope of this technote. Instead, the PSF information from reserved stars (n=212) is taken from the ComCam DRP `patch_table` using the `w_2025_17` weekly pipeline stack version; the specific collection used is `LSSTComCam/runs/DRP/DP1/w_2025_17/DM-50530`. With this in mind, this section covers some angular correlations of PSF qualities between the reserved stars and the model PSF at the center of each cell. For a more thorough treatment of the PSFs in the `Rubin_SV_38_7` field, see [PSF Technote].

The PSF quantities used here have the same definitions for both the reserve star PSFs and the cell-based coadd model PSFs, and both only use the i-band. These are defined with:

$$
\begin{align}
T &= I_{xx}  + I_{yy} \\
e_1 &= (I_{xx}-I_{yy}) / T \\
e_2 &= 2I_{xy} / T \\
e &= \sqrt{e_1^2 + e_2^2}
\end{align}
$$ (eqn2)

The moments used for the reserve stars come from the `i_ixx`, `i_iyy`, and `i_ixy` columns from the object table. As for the model PSF, this is measured directly from the PSF model image for each cell using methods from `lsst.afw.math` and `lsst.meas.algorithms`. Angular correlations are calculated using the `TreeCorr` package ({cite:p}`treecorr`), using the default “shot” variance.

```{figure} _static/psf_correlations.png
:name: psf_correlations

Top row: Residuals of the measured star PSF properties and the cell model PSFs. Note that not all cells will necessarily have a reserved star within them. Bottom row: Angular correlation between residual PSF values. The errors generated from `TreeCorr` are extremely small and blown up to show relative errors between bins.
```

### False Detections

{cite:p}`yamamoto` introduces a few cuts for spurious detections. The first is an upper limit of `wmom_T` as a function of `wmom_T_err`, while the second cut is the product of `wmom_T` x `wmom_T_err`. These focus on areas around bright stars and spurious detections within cluster fields, respectively. Plots in {numref}`junk1` and {numref}`junk2` were used to get an initial idea of where the data lied. Visual inspection of these objects showed that the majority are around bright stars, spurious detections, and small galaxies with close, undetected neighbors. Still, these cuts remove potentially viable galaxies from the sample, and don’t find every spurious detection. The values used in these cuts come from maximizing the number of spurious detections removed and minimizing the number of viable galaxies kept using visual inspection.

```{figure} _static/junk1.png
:name: junk1

Distribution of data points before and after cuts. The red lines indicate where the relevant junk cut removes data. The large cluster of points prior to applying all cuts in the left plot is mostly stars removed from the star-galaxy cut, and are not present in the plot on the right.
```

```{figure} _static/junk2.png
:name: junk2

Distribution of data points before and after cuts.
```

### Object Distributions

Plotting the distribution of objects on the sky is a simple but effective way to discover inconsistencies within the data. For example, the leftmost plot in {numref}`object-distribution` shows an overdensity of objects detected that align with where patches overlap, unexpected after removing exact duplicates based on RA and DEC coordinates. This duplication is addressed, as seen in the following plots in {numref}`object-distribution`.

```{figure} _static/object-distribution-before-after.png
:name: object-distribution

Object distributions of the non-sheared catalog at various points during cuts. Left: Galaxy distributions prior to any cuts. There is a clear overdensity that overlaps between patches. Middle: the distribution after the red sequence galaxies have been removed. Right: the object distribution after all cuts have been applied.
```

## References

```{bibliography}

```




