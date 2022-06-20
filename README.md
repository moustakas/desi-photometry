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
(EDR)](https://data.desi.lbl.gov/public/edr). The delivered files contain
[Tractor catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS-DR9](https://www.legacysurvey.org/dr9/description) for every observed
and *potential* non-sky DESI target in the EDR.

Content & Organization
----------------------


Known Issues
------------

For secondary targets in SV1, the targeting catalog filenames recorded in the
fiberassign header are inconsistent with the contents of the corresponding
fibermap catalog for a given TILEID.

For example, consider the following object:

```
(tileid,survey,program,targetid)=(80705,sv1,dark,39627751564509817)
```

The fiberassign header for this target indicates that
`${DESI_TARGET}/catalogs/dr9/0.50.0/targets/sv1/secondary/dark` was used to
gather the targeting information for this object. However, this catalog does not
contain any Legacy Surveys *grz* photometry for this target, whereas in the
final DESI redshift catalog,
`${DESI_ROOT}/spectro/redux/fuji/zcatalog/ztile-sv1-dark-cumulative.fits`, there
are *grz* fluxes for this target. Say something about patching. We solve this by
using the latest possible secondary catalog,
`${DESI_TARGET}/catalogs/dr9/0.52.0/targets/sv1/secondary/dark`, which was used
in the patching process.




(Pdb) jj = Table(fitsio.read('/global/cfs/cdirs/desi/target/catalogs/dr9/0.52.0/targets/sv1/secondary/dark/sv1targets-dark-secondary.fits'))
(Pdb) jj = Table(fitsio.read('/global/cfs/cdirs/desi/target/catalogs/dr9/0.51.0/targets/sv1/resolve/dark/sv1targets-dark-hp-158.fits'))



Contact
-------

For questions (or problems) regarding these catalogs or its construction, please
contact [John Moustakas](jmoustakas@siena.edu) ([Siena
College](https://siena.edu)).


