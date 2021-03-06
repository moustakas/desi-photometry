Legacy Surveys DR9 Photometric Catalogs for DESI Productions Fuji and Guadalupe
===============================================================================

DESI Value-Added Catalogs  
Fuji (Early Data Release)  
Guadalupe (Data Release 1 Supplement)  

**Version: 1.0**  

Description
-----------

This document describes the content and construction of [Legacy Surveys DR9
(LS/DR9)](https://www.legacysurvey.org/dr9/description) value-added photometric
catalogs for the following [DESI](https://desi.lbl.gov/) spectroscopic productions:

* **fuji**, which will be released publicly as part of the [DESI Early Data Release
(DESI/EDR)](https://data.desi.lbl.gov/public/edr) in early 2023 (exact date TBD); and
* **guadalupe**, a supplemental dataset to [DESI Data Release 1
(DESI/DR1)](https://data.desi.lbl.gov/public/dr1) (release date TBD).

In short, the delivered files include merged [DESI targeting
catalogs](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
and [Tractor catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS/DR9](https://www.legacysurvey.org/dr9/description) for *observed* and
*potential* DESI targets (excluding sky fibers), for both the fuji and guadalupe
productions.

Getting Started Quickly
-----------------------

This [example
notebook](https://github.com/moustakas/desi-photometry/blob/fujilupe-v1.0/example.ipynb)
shows how to quickly grab targeting and Tractor photometry from the fuji
value-added catalog for a hypothetical set of observed targets. However, be sure
to read the documentation below for all the details!

Content, Organization, & Data Model
-----------------------------------

The LS/DR9 value-added catalogs (VACs) can be accessed at the following links:
| Data Release | URL |
|------------|-----|
| fuji (EDR) | [https://data.desi.lbl.gov/public/edr/vac/lsdr9-photometry/fuji/v1.0](https://data.desi.lbl.gov/public/edr/vac/lsdr9-photometry/fuji/v1.0) |
| guadalupe (DR1 supplement) | [https://data.desi.lbl.gov/public/dr1/vac/lsdr9-photometry/guadalupe/v1.0](https://data.desi.lbl.gov/public/dr1/vac/lsdr9-photometry/guadalupe/v1.0) |

> **For DESI Collaborators:** At NERSC, the catalogs can also be accessed at the
    following top-level directories:
  ```
  /global/cfs/cdirs/desi/public/edr/vac/lsdr9-photometry/fuji/v1.0
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
[here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1),
but with a handful of additional columns documented below.

##### fuji

In fuji, there are five *targetphot* catalogs, one for each
[survey](https://data.desi.lbl.gov/doc/organization) in the EDR, as well as a
simple stack of all five catalogs:

| File Name | File Size | Number of Targets | Notes |
|-----------|:---------:|:-----------------:|-------|
| observed-targets/targetphot-cmx-fuji.fits | 4.39 MB | 4,146 | Commissioning Survey |
| observed-targets/targetphot-special-fuji.fits | 69.3 MB | 65,789 | Special targets |
| observed-targets/targetphot-sv1-fuji.fits | 759 MB | 720,525 | Survey Validation 1 |
| observed-targets/targetphot-sv2-fuji.fits | 137 MB | 130,473 | Survey Validation 2 |
| observed-targets/targetphot-sv3-fuji.fits | 1.92 GB | 1,865,908 | Survey Validation 3 |
| observed-targets/targetphot-fuji.fits | 2.87 GB | 2,786,841 | Stack of the preceding 5 catalogs. |

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
  merged catalog (`targetphot-fuji.fits`) contains `SURVEY` (`<U7`), `PROGRAM`
  (`<U6`), and `TILEID` (`np.int32`) columns to make it unambiguous which
  observation each row belongs to.

* Some targets have partial or minimal targeting information (e.g., *secondary*
  targets). For these objects, we populate "missing" *targetphot* columns with
  zeros or blank strings (depending on the data type of the column). We
  emphasize that the absence of this information doesn't mean the information
  doesn't exist *somewhere*, just that it wasn't used for targeting.

* In general, the same object can appear in two different surveys but *with
  different targeting information* (particularly fuji). For example, an object
  may be a *primary* target in one survey but a *secondary* target in another
  survey. Moreover, even within a given survey, an object can appear on two
  different tiles with different targeting information (e.g., the same object
  may be a bright-time target on one tile and a dark-time target on another
  tile). Consequently, we recommend considering `TARGETID`, `SURVEY`, *and*
  `TILEID` when retrieving the targeting information for specific targets
  (depending on how that information will be used, of course).

##### guadalupe

In guadalupe, there are just two *targetphot* catalogs as well as a stack of these two catalogs:

| File Name | File Size | Number of Targets | Notes |
|-----------|:---------:|:-----------------:|-------|
| observed-targets/targetphot-special-guadalupe.fits | 17.1 MB | 16,248 | Special targets |
| observed-targets/targetphot-main-guadalupe.fits | 2.69 GB | 2,617,551 | Main Survey |
| observed-targets/targetphot-guadalupe.fits | 2.71 GB | 2,633,799 | Stack of the preceding 2 catalogs. |

**Note:**

* The data model for these catalogs is defined
  [here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1). In
  addition, the merged catalog (`targetphot-guadalupe.fits`) contains a `SURVEY`
  (`<U7`), `PROGRAM` (`<U6`), and `TILEID` (`np.int32`) column.

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
divide them into *nested* (not ring; see [this *healpy*
tutorial](https://healpix.jpl.nasa.gov/pdf/intro.pdf))) `nside=4`
[healpixels](https://healpy.readthedocs.io/en/latest/) in a dedicated
subdirectory. (One `nside=4` healpixel corresponds to roughly 14.7 sq. degrees
on the sky.) We summarize their location (relative to the top-level directory)
as well as some additional details regarding the files in the following table:

| Data Release | Relative Location of *tractorphot* Files | Number of Files | Total Data Volume | Total Number of Objects |
|--------------|------------------------------------------|:---------------:|:-----------------:|:-----------------------:|
| fuji | observed-targets/tractorphot/tractorphot-nside4-hp[0-9][0-9][0-9]-fuji.fits | 71 | 3.86 GB | 1,957,908 |
| guadalupe | observed-targets/tractorphot/tractorphot-nside4-hp[0-9][0-9][0-9]-guadalupe.fits | 43 | 5.14 GB | 2,603,942 |

**Note:**

* In fuji, there are 1,979,269 unique observed targets (the 2,005,503 number
  tabulated above includes duplicate targets observed in different surveys), but
  just 1,957,908 unique objects with LS/DR9 photometry; the "missing" 21,361
  objects have no LS/DR9 source within 1 arcsec of the targeted position and
  therefore do not exist in any of the fuji *tractorphot* files.

* In guadalupe, the number of observed targets with missing LS/DR9 photometry is
  just 626.

#### Potential Targets

When assigning fibers to targets, DESI *fiber-assignment* also records the
*potential* targets, namely the set of targets *which could have been observed*
by a given fiber (*including targets which end up being observed*).

As part of these VACs, we include `targetphot` and `tractorphot` catalogs for
all these potential targets as documented above and as summarized in the tables
below:

##### fuji (*targetphot*)

| File Name | File Size | Number of Targets | Notes |
|-----------|:---------:|:-----------------:|-------|
| potential-targets/targetphot-potential-cmx-fuji.fits | 22.1 MB | 20,956 | Commissioning Survey |
| potential-targets/targetphot-potential-special-fuji.fits | 378 MB | 358,817 | Special targets |
| potential-targets/targetphot-potential-sv1-fuji.fits | 4.78 GB | 4,645,741 | Survey Validation 1 |
| potential-targets/targetphot-potential-sv2-fuji.fits | 790 MB | 750,431 | Survey Validation 2 |
| potential-targets/targetphot-potential-sv3-fuji.fits | 11 GB | 10,684,616 | Survey Validation 3 |
| potential-targets/targetphot-potential-fuji.fits | 16.9 GB | 16,460,561 | Stack of the preceding 5 catalogs. |

##### guadalupe (*targetphot*)

| File Name | File Size | Number of Targets | Notes |
|-----------|:---------:|:-----------------:|-------|
| potential-targets/targetphot-potential-special-guadalupe.fits | 84.4 MB | 80,182 | Special targets |
| potential-targets/targetphot-potential-main-guadalupe.fits | 17.1 GB | 16,603,258 | Main Survey |
| potential-targets/targetphot-potential-guadalupe.fits | 17.2 GB | 16,683,440 | Stack of the preceding 2 catalogs. |

##### *tractorphot* 

| Data Release | Relative Location of *tractorphot* Files | Number of Files | Total Data Volume | Total Number of Objects |
|--------------|------------------------------------------|:---------------:|:-----------------:|:-----------------------:|
| fuji | potential-targets/tractorphot/tractorphot-potential-nside4-hp[0-9][0-9][0-9]-fuji.fits | 71 | 11.9 GB | 6,031,273 |
| guadalupe | potential-targets/tractorphot/tractorphot-potential-nside4-hp[0-9][0-9][0-9]-guadalupe.fits | 43 | 31.1 GB | 15,758,409 |

Reproducibility
---------------

DESI collaborators (or others with all the necessary underlying files and
software dependencies) can reproduce either of the VACs presented here by
invoking the following commands at [NERSC](https://nersc.gov/) (illustrated here
just for fuji).

1. First, set up your software environment:
```bash
source /global/common/software/desi/desi_environment.sh 22.2
module unload desispec
cd /path/to/desi/code
git clone https://github.com/desihub/desispec.git
cd desispec && git checkout tags/0.53.2 && cd ..
export PYTHONPATH=/path/to/desi/code/desispec/py:${PYTHONPATH}
git clone https://github.com/moustakas/desi-photometry.git
cd desi-photometry && git checkout tags/v1.0 && cd ..
```

2. Next, gather targeting and Tractor photometry for *observed* targets:
```bash
time /path/to/desi/code/desi-photometry/lsdr9-photometry --reduxdir $DESI_ROOT/spectro/redux/fuji \
  -o /path/to/output/fuji --specprod fuji --mp 32 --targetphot --tractorphot
```

3. Finally gather targeting and Tractor photometry for *potential* targets:
```bash
time /path/to/desi/code/desi-photometry/lsdr9-photometry --reduxdir $DESI_ROOT/spectro/redux/fuji \
  -o /path/to/output/fuji --specprod fuji --mp 32 --targetphot --tractorphot --potential
```

Contact & Contributors
----------------------

For questions (or problems) regarding these catalogs or its construction, please
file a ticket at the [desi-photometry
repository](https://github.com/moustakas/desi-photometry/issues) and/or contact
[John Moustakas](jmoustakas@siena.edu) ([Siena
College](https://siena.edu)).

We are grateful for important contributions to the VACs presented herein from
the following individuals:

* Stephen Bailey (Lawrence Berkeley National Lab)
* Stephanie Juneau (NSF's NOIRLab)
* Dustin Lang (Perimeter Institute of Theoretical Physics)
* Adam Myers (University of Wyoming)
* Ragadeepika Pucha (University of Arizona)
* Anand Raichoor (Lawrence Berkeley National Lab)
* Benjamin Weaver (NSF's NOIRLab)

Required Acknowledgement
------------------------

Any use of the data products described in this document must include the text of
the following acknowledgement verbatim:
https://data.desi.lbl.gov/doc/acknowledgements.
