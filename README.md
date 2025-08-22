[![Website](https://img.shields.io/badge/sitcomtn--162-lsst.io-brightgreen.svg)](https://sitcomtn-162.lsst.io)
[![CI](https://github.com/lsst-sitcom/sitcomtn-162/actions/workflows/ci.yaml/badge.svg)](https://github.com/lsst-sitcom/sitcomtn-162/actions/workflows/ci.yaml)

# Testing the implementation of Metadetection and Cell-Based Coadds on Abell 360 ComCam data

## SITCOMTN-162

The purpose of this technote is to test the technical quality of ComCam commissioning data, specifically the Rubin_SV_38_7 field, by utilizing cell-based coadds and Metadetection by measuring the tangential shear profile, and the cross shear profile, of the massive cluster Abell 360 (called A360 throughout the technote). The process entails generating the cell-based coadds for Metadetection to run on, identifying and removing cluster member galaxies, applying quality cuts, and calibrating the shear measurements. Once a shear profile is generated, validation is the bulk of the remaining analysis.

Cell-based coadds and Metadetection are both currently in the process of being implemented within the LSST Science Pipelines at the time of this technote. There is quite a bit of technical value in attempting a difficult measurement prior to full implementation. Measuring the tangential shear around A360 will showcase the current abilities of these algorithms, as well as highlight where work is still needed.

**Links:**

- Publication URL: https://sitcomtn-162.lsst.io
- Alternative editions: https://sitcomtn-162.lsst.io/v
- GitHub repository: https://github.com/lsst-sitcom/sitcomtn-162
- Build system: https://github.com/lsst-sitcom/sitcomtn-162/actions/


## Build this technical note

You can clone this repository and build the technote locally if your system has Python 3.11 or later:

```sh
git clone https://github.com/lsst-sitcom/sitcomtn-162
cd sitcomtn-162
make init
make html
```

Repeat the `make html` command to rebuild the technote after making changes.
If you need to delete any intermediate files for a clean build, run `make clean`.

The built technote is located at `_build/html/index.html`.

## Publishing changes to the web

This technote is published to https://sitcomtn-162.lsst.io whenever you push changes to the `main` branch on GitHub.
When you push changes to a another branch, a preview of the technote is published to https://sitcomtn-162.lsst.io/v.

## Editing this technical note

The main content of this technote is in `index.md` (a Markdown file parsed as [CommonMark/MyST](https://myst-parser.readthedocs.io/en/latest/index.html)).
Metadata and configuration is in the `technote.toml` file.
For guidance on creating content and information about specifying metadata and configuration, see the Documenteer documentation: https://documenteer.lsst.io/technotes.
