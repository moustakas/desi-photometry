Legacy Surveys DR9 Photometric Catalogs
=======================================

DESI Value-Added Catalog  
Early Data Release (EDR/Fuji)
2022 June XX  

**Version: 1.0**

Description
-----------

This document describes the content and construction of the Legacy Surveys DR9
(LS-DR9) value-added photometric catalogs for the [DESI Early Data Release
(DESI/EDR)](https://data.desi.lbl.gov/public/edr). In short, the delivered files
include merged [DESI targeting
catalogs](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
and [Tractor catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS-DR9](https://www.legacysurvey.org/dr9/description) for observed and
*potential* DESI targets (excluding sky fibers) in the EDR.

Content, Organization, & Data Model
-----------------------------------

The LS-DR9 value-added catalog (VAC) can be accessed at NERSC at the following
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

There are five relevant catalogs, one for each *survey* (**need a documentation
link**) in the EDR, as well as a simple stack of all five catalogs.

```
targetphot-cmx-edr.fits [4.4MB, N=4,146]
targetphot-special-edr.fits [39MB, N=37,296]
targetphot-sv1-edr.fits [712MB, N=685,884]
targetphot-sv2-edr.fits [119MB, N=113,914]
targetphot-sv3-edr.fits [1.2GB, N=1,164,263]
targetphot-edr.fits [2.1GB, N=2,005,503]
```

The data model for each *targetphot* catalog is documented
[here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
(see also [Meyers et
al. 2022](https://desi.lbl.gov/DocDB/cgi-bin/private/ShowDocument?docid=6693))
with the following differences:

* Each catalog contains the targeting columns for *all* the possible surveys in
the EDR (making it easier for for the catalogs to be stacked or combined),
specifically: `CMX_TARGET` `DESI_TARGET`, `BGS_TARGET`, `MWS_TARGET`,
`SV1_DESI_TARGET`, `SV1_BGS_TARGET`, `SV1_MWS_TARGET`, `SV2_DESI_TARGET`,
`SV2_BGS_TARGET`, `SV2_MWS_TARGET`, `SV3_DESI_TARGET`, `SV3_BGS_TARGET`,
`SV3_MWS_TARGET`, `SCND_TARGET`, `SV1_SCND_TARGET`, `SV2_SCND_TARGET`, and
`SV3_SCND_TARGET` (all with a `numpy.int64` data type). In addition, the merged
catalog (`targetphot-edr.fits`) contains a `SURVEY` column.

* Some targets have partial or minimal targeting information (e.g., *secondary*
  targets). For these objects, we populate "missing" *targetphot* columns with
  zeros or blank strings (depending on the data type of the column). We
  emphasize that the absence of this information doesn't mean the information
  doesn't exist *somewhere*, just that it wasn't used for targeting.

> **Note:** In the DESI/EDR, the same object can appear in two different surveys
but *with different targeting information*. For example, an object may be a
*primary* target in one survey but a *secondary* target in another
survey. Consequently, we recommend considering both `TARGETID` and `SURVEY` when
retrieving the targeting information for specific targets (depending on how that
information will be used, of course).

#### Tractor (*tractorphot*) Catalogs

For each unique target in the `targetphot-edr.fits` file, we retrieve [Tractor
catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS-DR9](https://www.legacysurvey.org/dr9/description). These catalogs are
"value-added" compared to the information in the [official DESI/EDR targeting
catalogs](https://data.desi.lbl.gov/public/edr/target/catalogs) in a couple
ways:

* First, the *tractorphot* catalogs included in this VAC contain *all* the
  measurements from the Tractor, not just the measurements included in the
  light-weight [sweep
  catalogs](https://www.legacysurvey.org/dr9/files/#sweep-catalogs-region-sweep)
  which are used by DESI targeting.

* And second, the *tractorphot* catalogs include
  [LS-DR9](https://www.legacysurvey.org/dr9/description) photometry for targets
  which were not necessarily targeted from LS-DR9 (such as *secondary* targets),
  using positional matching.

No matches for 21,361 objects.

#### Potential Targets

Write me.

Reproducibility
---------------

Assuming all the relevant DESI environment variables have been set (write me),
one can regenerate the catalogs by cloning this repository and calling:

```bash
lsdr9-photometry --reduxdir /path/to/edr -o /path/to/output --outsuffix edr --mp 1 --targetphot

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
College](https://siena.edu)). **Need to acknowledge other contributors / members
of the DESI Data Team.**


