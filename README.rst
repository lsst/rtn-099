.. image:: https://img.shields.io/badge/rtn--099-lsst.io-brightgreen.svg
   :target: https://rtn-099.lsst.io
.. image:: https://github.com/lsst/rtn-099/workflows/CI/badge.svg
   :target: https://github.com/lsst/rtn-099/actions/

################################################################
Photometric Transformation Relations for the LSST Data Preview 1
################################################################

RTN-099
=======

Here we provide photometric transformation relations between the LSST Commissioning Camera data in the Data Preview 1 data set and other well-known optical/NIR photometric systems (Dark Energy Survey, PanSTARRS-1, Euclid, etc.)

Links
=====

- Live drafts: https://rtn-099.lsst.io
- GitHub: https://github.com/lsst/rtn-099

Build
=====

This repository includes lsst-texmf_ as a Git submodule.
Clone this repository::

    git clone --recurse-submodules https://github.com/lsst/rtn-099

Compile the PDF::

    make

Clean built files::

    make clean

Updating acronyms
-----------------

A table of the technote's acronyms and their definitions are maintained in the `acronyms.tex` file, which is committed as part of this repository.
To update the acronyms table in ``acronyms.tex``::

    make acronyms.tex

*Note: this command requires that this repository was cloned as a submodule.*

The acronyms discovery code scans the LaTeX source for probable acronyms.
You can ensure that certain strings aren't treated as acronyms by adding them to the `skipacronyms.txt <./skipacronyms.txt>`_ file.

The lsst-texmf_ repository centrally maintains definitions for LSST acronyms.
You can also add new acronym definitions, or override the definitions of acronyms, by editing the `myacronyms.txt <./myacronyms.txt>`_ file.

Updating lsst-texmf
-------------------

`lsst-texmf`_ includes BibTeX files, the ``lsstdoc`` class file, and acronym definitions, among other essential tooling for LSST's LaTeX documentation projects.
To update to a newer version of `lsst-texmf`_, you can update the submodule in this repository::

   git submodule update --init --recursive

Commit, then push, the updated submodule.

.. _lsst-texmf: https://github.com/lsst/lsst-texmf
