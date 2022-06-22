Legacy Surveys DR9 Photometric Catalogs
=======================================

DESI Value-Added Catalog  
Early Data Release (Fuji)  
2022 June XX  

**Version: 1.0**

Description
-----------

This document describes the content and construction of the Legacy Surveys DR9
(LS-DR9) value-added photometric catalogs for the [DESI Early Data Release
(EDR)](https://data.desi.lbl.gov/public/edr). In short, the delivered files
include merged [DESI targeting
catalogs](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
and [Tractor catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS-DR9](https://www.legacysurvey.org/dr9/description) for observed and
*potential* DESI targets (excluding sky fibers) in the EDR.

Content, Organization, & Data Model
-----------------------------------

The LS-DR9 value-added catalog (VAC) can be accessed at NERSC in the following
top-level directory (**TBD**):

```bash
/global/cscratch1/sd/ioannis/lsdr9-photometry/v1.0
```

The VAC contains two basic kinds of files: targeting (*targetphot*) catalogs,
and photometric or Tractor (*tractorphot*) catalogs, which we now describe in
more detail.

#### Targeting (*targetphot*) Catalogs

In the [DESI/EDR](https://data.desi.lbl.gov/public/edr), the targeting catalogs
used for DESI target selection are organized in a variety of files and locations
and with a different data model depending on the kind of target observed (e.g.,
*primary* versus *secondary* targets; see [Meyers et
al. 2022](https://desi.lbl.gov/DocDB/cgi-bin/private/ShowDocument?docid=6693)). However,
for some applications, it is convenient to have a merged targeting catalog for
all targets and with a common data model, which is precisely what this VAC
provides.

There are five relevant catalogs, one for each *survey* in the EDR: [**(1)
Release cmx? (2) Use "edr" rather than fuji suffix? (3) Merge these into one
catalog? Need to add a documentation link for "survey".**):

```
targetphot-cmx-fuji.fits
targetphot-special-fuji.fits
targetphot-sv1-fuji.fits
targetphot-sv2-fuji.fits
targetphot-sv3-fuji.fits
```



The data model of the *targetphot* catalogs is described
[here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
(see also [Meyers et
al. 2022](https://desi.lbl.gov/DocDB/cgi-bin/private/ShowDocument?docid=6693))
with the following difference:

In addition: zeros and blank strings.


#### Tractor (*tractorphot*) Catalogs

Write me. Matching radius; no matches for XXX objects.

[Tractor catalog photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)

#### Potential Targets

Write me.

Reproducibility
---------------

Assuming all the relevant DESI environment variables have been set (write me),
one can regenerate the catalogs by cloning this repository and calling:

```bash
lsdr9-photometry --reduxdir $DESI_ROOT/spectro/redux/fuji -o /global/cscratch1/sd/ioannis/photocatalog/fuji --outprefix fuji --mp 32 --targetphot

```

Known Issues
------------

* For secondary targets in SV1, the targeting catalog filenames recorded in the
fiberassign header are inconsistent with the contents of the corresponding
fibermap catalog for a given TILEID.

* 

Contact
-------

For questions (or problems) regarding these catalogs or its construction, please
contact [John Moustakas](jmoustakas@siena.edu) ([Siena
College](https://siena.edu)).


