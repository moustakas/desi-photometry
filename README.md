Legacy Surveys DR9 Photometric Catalogs
=======================================

DESI Value-Added Catalog  
Fuji / Early Data Release (EDR)  
Guadalupe supplement to Data Release 1 (DR1)  

**Version: 1.0**  
2022 July XX

Description
-----------

This document describes the content and construction of the [Legacy Surveys DR9
(LS/DR9)](https://www.legacysurvey.org/dr9/description) value-added photometric
catalogs for two distinct [DESI](https://desi.lbl.gov/) data releases: (1) the
[DESI Early Data Release (DESI/EDR)](https://data.desi.lbl.gov/public/edr) (also
called the **fuji data release**); and (2) the **guadalupe data release**, a
supplemental release to [DESI Data Release 1
(DESI/DR1)](https://data.desi.lbl.gov/public/dr1).

The fuji files will be publicly released with the DESI/EDR in early 2023 (exact
date TBD) and the guadalupe files will be released with DESI/DR1 (release date
TBD).

In short, the delivered files include merged [DESI targeting
catalogs](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
and [Tractor catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS/DR9](https://www.legacysurvey.org/dr9/description) for observed and
*potential* DESI targets (excluding sky fibers) in the fuji and guadalupe releases. 

> **Getting Started Quickly:** The [example
    notebook](https://github.com/moustakas/desi-photometry/blob/main/example.ipynb)
    shows how to quickly grab targeting and Tractor photometry from this
    value-added catalog for a hypothetical set of fuji targets, but be sure to
    read the documentation below for all the details.

Content, Organization, & Data Model
-----------------------------------

The LS/DR9 value-added catalogs (VAC) can be accessed at the following url:
```
[https://data.desi.lbl.gov/doc/releases/edr/vac/lsdr9-photometry/v1.0](https://data.desi.lbl.gov/doc/releases/edr/vac/lsdr9-photometry/v1.0)
```
and
```
[https://data.desi.lbl.gov/doc/releases/dr1/vac/lsdr9-photometry/v1.0](https://data.desi.lbl.gov/doc/releases/dr1/vac/lsdr9-photometry/v1.0)
```
for fuji and guadalupe, respectively.

> **For DESI Collaborators:** At NERSC, the catalogs can also be accessed at the
    following top-level directories:
  ```
  /global/cfs/cdirs/desi/public/edr/vac/lsdr9-photometry/v1.0
  ```
  and
  ```
  /global/cfs/cdirs/desi/public/dr1/vac/lsdr9-photometry/v1.0
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
targetphot-edr.fits [2.1GB, N=2,005,5030]
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
catalog (`targetphot-edr.fits`) contains a `SURVEY` (`<U7`) column.

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
from [LS/DR9](https://www.legacysurvey.org/dr9/description). These catalogs are
"value-added" compared to the information in the [official DESI/EDR targeting
catalogs](https://data.desi.lbl.gov/public/edr/target/catalogs) in a couple
ways:

* First, the *tractorphot* catalogs included in this VAC contain *all* the
  photometric quantities measured by *Tractor*, not just the measurements
  included in the light-weight [sweep
  catalogs](https://www.legacysurvey.org/dr9/files/#sweep-catalogs-region-sweep)
  used to select DESI targets.

* And second, the *tractorphot* catalogs in this VAC include
  [LS/DR9](https://www.legacysurvey.org/dr9/description) photometry for targets
  which were not necessarily targeted from LS/DR9, such as *secondary* targets,
  using positional matching. Specifically, if the `targetid` of a *secondary*
  target cannot be decoded to determine the LS/DR9 source from which that target
  was selected (see [Meyers et
  al. 2022](https://desi.lbl.gov/DocDB/cgi-bin/private/ShowDocument?docid=6693)),
  then we return the *closest* LS/DR9 source within 1 arcsec of the targeted
  position.

Now, because the `tractorphot` catalogs can become prohibitively large, we
divide them into `nside=4`
[healpixels](https://healpy.readthedocs.io/en/latest/) in a dedicated
subdirectory. The location (relative to the top-level directory) and form of
these files is:

```
tractorphot/tractorphot-nside4-hp[0-9][0-9][0-9]-edr.fits
```

Here, `hp[0-9][0-9][0-9]` corresponds to the `nside=4` healpixel number (using the *nested*
pixelization scheme; see [this *healpy*
tutorial](https://healpix.jpl.nasa.gov/pdf/intro.pdf)). There are 71 files
ranging in size from <1MB to roughly 0.6GB, and, for reference, an `nside=4`
healpixel is roughly 14.7 sq. degrees.

From the parent `targetphot-edr.fits` catalog of 2,005,503 objects, 1,979,269 of
these are unique and 21,361 have no LS/DR9 source within 1 arcsec of the
targeted position, leaving 1,957,908 as the total number of unique DESI targets
with Tractor photometry.

Finally, the data model of each catalog is identical to that of the [LS/DR9
Tractor
catalog](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
except that we add a `TARGETID` column to make it easier to cross-reference with
the DESI redshift catalogs.

#### Potential Targets

When assigning fibers to targets, DESI *fiber-assignment* (**reference?**) also
records the *potential* targets, namely the set of targets *which could have
been observed* by a given fiber.

As part of this VAC, we include `targetphot` and `tractorphot` catalogs (as
documented above) for all these potential targets:

```
targetphot-potential-edr.fits [6.3GB, N=6,191,755]
tractorphot/tractorphot-potential-nside4-hp[0-9][0-9][0-9]-edr.fits
```

Contact
-------

For questions (or problems) regarding these catalogs or its construction, please
contact [John Moustakas](jmoustakas@siena.edu) ([Siena
College](https://siena.edu)). **Need to acknowledge other contributors / members
of the DESI Data Team.**


