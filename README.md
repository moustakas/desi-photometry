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
(EDR)](https://data.desi.lbl.gov/public/edr). The delivered files include merged
[DESI targeting
catalogs](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
(with a handful of supplemental columns) and [Tractor catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS-DR9](https://www.legacysurvey.org/dr9/description) for every observed
and *potential* non-sky DESI target in the EDR.

Content & Organization
----------------------

This value-added catalog (VAC) can be accessed at NERSC in the following
top-level directory:

```bash
/global/cscratch1/sd/ioannis/lsdr9-photometry/v1.0
```



Data Model
----------

The targetphot catalogs nclude all the possible targeting bits.


Reproducibility
---------------

To generate 


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


