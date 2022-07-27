Legacy Surveys DR9 Photometric Catalogs for DESI Targets
========================================================

DESI Value-Added Catalogs
Fuji / Early Data Release (EDR)  
Guadalupe supplement to Data Release 1 (DR1)  

**Version: 1.0**  
2022 July XX

Description
-----------

This document describes the content and construction of [Legacy Surveys DR9
(LS/DR9)](https://www.legacysurvey.org/dr9/description) value-added photometric
catalogs for the following [DESI](https://desi.lbl.gov/) data releases:

1. The [DESI Early Data Release
(DESI/EDR)](https://data.desi.lbl.gov/public/edr), also known as the **fuji
release**; and
2. The **guadalupe release**, a supplemental dataset to [DESI Data Release 1
(DESI/DR1)](https://data.desi.lbl.gov/public/dr1).

In short, the delivered files include merged [DESI targeting
catalogs](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
and [Tractor catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS/DR9](https://www.legacysurvey.org/dr9/description) for both *observed*
and *potential* DESI targets (excluding sky fibers), for both the fuji and
guadalupe releases.

> **Note:** The VAC associated with fuji will be publicly released with the
  DESI/EDR in early 2023 (exact date TBD), and the VAC associated with guadalupe
  will be publicly released with DESI/DR1 (release date TBD).

Getting Started Quickly
-----------------------

This [example
notebook](https://github.com/moustakas/desi-photometry/blob/v1.0/example.ipynb)
shows how to quickly grab targeting and Tractor photometry from the fuji
value-added catalog for a hypothetical set of observed targets. However, be sure
to read the documentation below for all the details!

Content, Organization, & Data Model
-----------------------------------

The LS/DR9 value-added catalogs (VACs) can be accessed at the following urls:
| Data Release | url |
|------------|-----|
| fuji (EDR) | [https://data.desi.lbl.gov/doc/releases/edr/vac/lsdr9-photometry/v1.0](https://data.desi.lbl.gov/doc/releases/edr/vac/lsdr9-photometry/v1.0) |
| guadalupe (DR1 supplement) | [https://data.desi.lbl.gov/doc/releases/dr1/vac/lsdr9-photometry/guadalupe/v1.0](https://data.desi.lbl.gov/doc/releases/dr1/vac/lsdr9-photometry/guadalupe/v1.0) |

> **For DESI Collaborators:** At NERSC, the catalogs can also be accessed at the
    following top-level directories:
  ```
  /global/cfs/cdirs/desi/public/edr/vac/lsdr9-photometry/v1.0
  /global/cfs/cdirs/desi/public/dr1/vac/lsdr9-photometry/guadalupe/v1.0
  ```

The VAC contains two basic kinds of files: targeting (*targetphot*) catalogs,
and photometric or Tractor (*tractorphot*) catalogs, which we now describe in
more detail.

#### Targeting (*targetphot*) Catalogs

In each DESI data release, the targeting catalogs used for DESI target selection
are organized in a variety of files and locations and with a different data
model depending on the kind of target observed (e.g., *primary* versus
*secondary* targets; see [Meyers et
al. 2022](https://desi.lbl.gov/DocDB/cgi-bin/private/ShowDocument?docid=6693)). However,
for some applications, it is convenient to have a merged targeting catalog for
all targets and with a common data model, which is precisely what our VACs
provide.

The data model for each *targetphot* catalog is documented
[here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
(see also [Meyers et
al. 2022](https://desi.lbl.gov/DocDB/cgi-bin/private/ShowDocument?docid=6693)),
but with some minor additional columns documented below.

##### fuji

In fuji, there are five *targetphot* catalogs, one for each
[survey](https://data.desi.lbl.gov/doc/organization) in the EDR, as well as a
simple stack of all five catalogs.

| File Name    | File Size | Number of Objects | Notes |
|--------------|:-----:|:-----------:|-----------|
| observed-targets/targetphot-cmx-fuji.fits     | 4.4MB | 4,146      | CMX Survey      |
| observed-targets/targetphot-special-fuji.fits | 39MB  | 37,296     | Special targets |
| observed-targets/targetphot-sv1-fuji.fits     | 712MB | 685,884    | SV1             |
| observed-targets/targetphot-sv2-fuji.fits     | 119MB | 113,914    | SV2             |
| observed-targets/targetphot-sv3-fuji.fits     | 1.2GB | 1,164,263  | SV3             |
| observed-targets/targetphot-fuji.fits         | 2.1GB | 2,005,5030 | Stack of the preceding five catalogs. |

**Note:**

* In addition to the columns defined
  [here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1),
  these catalogs contain the targeting columns for *all* the possible surveys in
  the EDR (making it easier for for the catalogs to be stacked or combined),
  specifically: `CMX_TARGET` `DESI_TARGET`, `BGS_TARGET`, `MWS_TARGET`,
  `SV1_DESI_TARGET`, `SV1_BGS_TARGET`, `SV1_MWS_TARGET`, `SV2_DESI_TARGET`,
  `SV2_BGS_TARGET`, `SV2_MWS_TARGET`, `SV3_DESI_TARGET`, `SV3_BGS_TARGET`,
  `SV3_MWS_TARGET`, `SCND_TARGET`, `SV1_SCND_TARGET`, `SV2_SCND_TARGET`, and
  `SV3_SCND_TARGET` (all with a `numpy.int64` data type). In addition, the
  merged catalog (`targetphot-fuji.fits`) contains a `SURVEY` (`<U7`) column.

* Some targets have partial or minimal targeting information (e.g., *secondary*
  targets). For these objects, we populate "missing" *targetphot* columns with
  zeros or blank strings (depending on the data type of the column). We
  emphasize that the absence of this information doesn't mean the information
  doesn't exist *somewhere*, just that it wasn't used for targeting.

* In fuji, the same object can appear in two different surveys but *with
  different targeting information*. For example, an object may be a *primary*
  target in one survey but a *secondary* target in another survey. Consequently,
  we recommend considering both `TARGETID` and `SURVEY` when retrieving the
  targeting information for specific targets (depending on how that information
  will be used, of course).

##### guadalupe

In guadalupe, there are just two *targetphot* catalogs as well as a simple
stack of these two catalogs.

| File Name    | File Size | Number of Objects | Notes |
|--------------|:-----:|:-----------|-----------|
| observed-targets/targetphot-special-guadalupe.fits | 17MB  | 16,088    | Special targets |
| observed-targets/targetphot-main-guadalupe.fits    | 2.7GB | 2,596,279 | Main Survey |
| observed-targets/targetphot-guadalupe.fits         | 2.7GB | 2,612,367 | Stack of the preceding two catalogs. |

**Note:**

* The data model for these catalogs is defined
  [here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1). In
  addition, the merged catalog (`targetphot-guadalupe.fits`) contains a `SURVEY`
  (`<U7`) column.

* As for fuji, some targets have partial or minimal targeting information (e.g.,
  *secondary* targets), in which case we populate "missing" columns with zeros
  or blank strings (depending on the data type of the column).

#### Tractor (*tractorphot*) Catalogs

For each unique target in the `targetphot-fuji.fits` and
`targetphot-guadalupe.fits` files, we retrieve [Tractor catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS/DR9](https://www.legacysurvey.org/dr9/description). These catalogs are
"value-added" compared to the information in the [official DESI/EDR targeting
catalogs](https://data.desi.lbl.gov/public/edr/target/catalogs) in a couple
ways:

* First, the delivered *tractorphot* catalogs contain *all* the photometric
  quantities measured by *Tractor* (documented
  [here](https://datalab.noirlab.edu/query.php?name=ls_dr9.tractor)), not just
  the measurements included in the light-weight [sweep
  catalogs](https://www.legacysurvey.org/dr9/files/#sweep-catalogs-region-sweep)
  used to select DESI targets (see also
  [here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)).

* Second, the *tractorphot* catalogs include
  [LS/DR9](https://www.legacysurvey.org/dr9/description) photometry for targets
  which were not necessarily targeted by DESI, such as *secondary* targets and
  *targets of opportunity*, using positional matching. Specifically, if the
  `targetid` of a *secondary* target cannot be decoded to determine the LS/DR9
  source from which that target was selected (see [Meyers et
  al. 2022](https://desi.lbl.gov/DocDB/cgi-bin/private/ShowDocument?docid=6693)),
  then we return the *closest* LS/DR9 source within 1 arcsec of the targeted
  position.

* Finally, we add two additional columns to the *tractorphot* catalogs to make
it easier to cross-reference with the DESI redshift catalogs: `TARGETID`
(`numpy.int64`) and `LS_ID` (`numpy.int64`), the latter of which is documented
[here](https://datalab.noirlab.edu/query.php?name=ls_dr9.tractor).

Now, because the `tractorphot` catalogs can become prohibitively large, we
divide them into nested (not ring) `nside=4`
[healpixels](https://healpy.readthedocs.io/en/latest/) in a dedicated
subdirectory. We summarize the location (relative to the top-level directory) of
these files as well as some additional details regarding the files in the
following table:

| Data Release | Location of *tractorphot* files | Number of files | Data volume | Total number of objects |
|------------|-----|:-----:|:-----:|:-----:|
| fuji      | observed-targets/tractorphot/tractorphot-nside4-hp[0-9][0-9][0-9]-fuji.fits      | 71 | 3.9GB | 1,979,269 |
| guadalupe | observed-targets/tractorphot/tractorphot-nside4-hp[0-9][0-9][0-9]-guadalupe.fits | 43 | 5.2GB | 2,603,942 |

In these file names, `hp[0-9][0-9][0-9]` corresponds to the `nside=4` healpixel
number (using the *nested* pixelization scheme; see [this *healpy*
tutorial](https://healpix.jpl.nasa.gov/pdf/intro.pdf)), which corresponds to
roughly 14.7 sq. degrees on the sky.

**Note:**

* In fuji, there are 2,005,503 observed targets but 1,979,269 objects with
  LS/DR9 photometry; the "missing" 21,361 objects have no LS/DR9 source within 1
  arcsec of the targeted position and therefore do not exist in any of the fuji
  *tractorphot* files.

* In guadalupe, the number of observed targets with missing LS/DR9 photometry is
  just 626.

#### Potential Targets

When assigning fibers to targets, DESI *fiber-assignment* also records the
*potential* targets, namely the set of targets *which could have been observed*
by a given fiber (*including targets which end up being observed*).

As part of these VACs, we include `targetphot` and `tractorphot` catalogs (as
documented above) for all these potential targets:

```
targetphot-potential-fuji.fits [6.3GB, N=6,191,755]
tractorphot/tractorphot-potential-nside4-hp[0-9][0-9][0-9]-fuji.fits
```

Contact & Required Acknowledgement
----------------------------------

For questions (or problems) regarding these catalogs or its construction, please
contact [John Moustakas](jmoustakas@siena.edu) ([Siena
College](https://siena.edu)). 




**Need to acknowledge other contributors / members of the DESI Data Team.**


