Legacy Surveys DR9 Photometric Catalogs for DESI Productions Iron, Guadalupe, and Fuji
======================================================================================

DESI Value-Added Catalogs
Iron (Data Release 1)
Guadalupe (Data Release 1 Supplement)
Fuji (Early Data Release)

Description
-----------

This document describes the content and construction of [Legacy Surveys DR9
(LS/DR9)](https://www.legacysurvey.org/dr9/description) value-added photometric
catalogs for the following [DESI](https://desi.lbl.gov/) spectroscopic productions:

* [DESI Early Data Release (EDR)](https://data.desi.lbl.gov/public/edr) (release date May 2023)
  * **Fuji**
* [DESI Data Release 1 (DR1)](https://data.desi.lbl.gov/public/dr1) (release date TBD)
  * **Iron**
  * **Guadalupe**

In short, the delivered files include merged [DESI targeting
catalogs](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1)
and [Tractor catalog
photometry](https://www.legacysurvey.org/dr9/description/#tractor-catalogs-1)
from [LS/DR9](https://www.legacysurvey.org/dr9/description) for *observed* and
*potential* DESI targets (excluding sky fibers).

Getting Started Quickly
-----------------------

This [example
notebook](https://github.com/moustakas/desi-photometry/blob/main/example-sv.ipynb)
shows how to quickly grab targeting and Tractor photometry from the Fuji
value-added catalog for a hypothetical set of observed targets. However, be sure
to read the documentation below for all the details!

Content, Organization, & Data Model
-----------------------------------

The LS/DR9 value-added catalogs (VACs) can be accessed at the following links:
| Data Release | URL |
|------------|-----|
| Fuji (EDR) | [https://data.desi.lbl.gov/public/edr/vac/lsdr9-photometry/fuji/v2.0](https://data.desi.lbl.gov/public/edr/vac/lsdr9-photometry/fuji/v2.0) |
| Iron (DR1) | [https://data.desi.lbl.gov/public/dr1/vac/lsdr9-photometry/iron/v1.0](https://data.desi.lbl.gov/public/dr1/vac/lsdr9-photometry/iron/v1.0) |
| Guadalupe (DR1 supplement) | [https://data.desi.lbl.gov/public/dr1/vac/lsdr9-photometry/guadalupe/v2.0](https://data.desi.lbl.gov/public/dr1/vac/lsdr9-photometry/guadalupe/v2.0) |

> **For DESI Collaborators:** At NERSC, the catalogs can also be accessed at the
    following top-level directories:
  ```
  /global/cfs/cdirs/desi/public/edr/vac/lsdr9-photometry/fuji/v2.0
  /global/cfs/cdirs/desi/public/dr1/vac/lsdr9-photometry/iron/v1.0
  /global/cfs/cdirs/desi/public/dr1/vac/lsdr9-photometry/guadalupe/v2.0
  ```

The VAC contains two basic kinds of files: targeting (*targetphot*) catalogs,
and photometric or Tractor (*tractorphot*) catalogs, which we now describe in
more detail.

### Targeting (*targetphot*) Catalogs

In each DESI data release, the targeting catalogs used for DESI target selection
are organized in a variety of files and locations and with a different data
model depending on the kind of target observed (e.g., *primary* versus
*secondary* targets; see [Myers et
al. 2022](https://arxiv.org/abs/2208.08518)). However,
for some applications, it is convenient to have a merged targeting catalog for
all targets and with a common data model, which is precisely what our VACs
provide.

The data model for each *targetphot* catalog is documented
[here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1),
but with a handful of additional columns documented below.

#### Fuji

In Fuji, there are six *targetphot* catalogs, one for each
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

#### Iron

In Iron, there are seven *targetphot* catalogs, five for the Commissioning and
Survey Validation periods of the project (see the survey definitions
[here](https://data.desi.lbl.gov/doc/organization), one for the [Main
Survey](https://data.desi.lbl.gov/doc/glossary/#main-survey), and one which is a
simple stack of all six catalogs:

| File Name | File Size | Number of Targets | Notes |
|-----------|:---------:|:-----------------:|-------|
| observed-targets/targetphot-cmx-iron.fits | 4.39 MB | 4,146 | Commissioning Survey |
| observed-targets/targetphot-special-iron.fits | 177 MB | 168,328 | Special targets |
| observed-targets/targetphot-sv1-iron.fits | 755 MB | 716,948 | Survey Validation 1 |
| observed-targets/targetphot-sv2-iron.fits | 129 MB | 122,189 | Survey Validation 2 |
| observed-targets/targetphot-sv3-iron.fits | 1.92 GB | 1,865,908 | Survey Validation 3 |
| observed-targets/targetphot-main-iron.fits | 22.6 GB | 22,019,411 | Main Survey |
| observed-targets/targetphot-iron.fits | 25.6 GB | 24,896,930 | Stack of the preceding 6 catalogs. |

**Note:**

* The data model for these catalogs is defined
  [here](https://desidatamodel.readthedocs.io/en/latest/DESI_TARGET/TARG_DIR/DR/VERSION/targets/PHASE/RESOLVE/OBSCON/PHASEtargets-OBSCON-RESOLVE-hp-HP.html#hdu1). In
  addition, the merged catalog (`targetphot-guadalupe.fits`) contains a `SURVEY`
  (`<U7`), `PROGRAM` (`<U6`), and `TILEID` (`np.int32`) column.

* As for Fuji, some targets have partial or minimal targeting information (e.g.,
  *secondary* targets), in which case we populate "missing" columns with zeros
  or blank strings (depending on the data type of the column).

#### Guadalupe

In Guadalupe, there are just three *targetphot* catalogs based on the first two
months of [Main Survey](https://data.desi.lbl.gov/doc/glossary/#main-survey)
observations.

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

* As for Fuji and Iron, some targets have partial or minimal targeting
  *information (e.g., secondary* targets), in which case we populate "missing"
  *columns with zeros or blank strings (depending on the data type of the
  *column).

### Tractor (*tractorphot*) Catalogs

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
  source from which that target was selected (see [Myers et
  al. 2022](https://arxiv.org/abs/2208.08518)),
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
| Fuji | observed-targets/tractorphot/tractorphot-nside4-hp[0-9][0-9][0-9]-fuji.fits | 71 | 3.86 GB | 1,957,907 |
| iron | observed-targets/tractorphot/tractorphot-nside4-hp[0-9][0-9][0-9]-iron.fits | 104 | 43.2 GB | 21,896,601 |
| Guadalupe | observed-targets/tractorphot/tractorphot-nside4-hp[0-9][0-9][0-9]-guadalupe.fits | 43 | 5.14 GB | 2,603,942 |

**Note:**

* In Fuji, there are 1,979,269 unique observed targets (the 2,786,841 number
  tabulated above includes duplicate targets observed in different surveys), but
  just 1,957,907 unique objects with LS/DR9 photometry; the "missing" 21,362
  objects have no LS/DR9 source within 1 arcsec of the targeted position and
  therefore do not exist in any of the Fuji *tractorphot* files.

* In Guadalupe, the number of observed targets with missing LS/DR9 photometry is
  just 626.

### Potential Targets

When assigning fibers to targets, DESI *fiber-assignment* also records the
*potential* targets, namely the set of targets *which could have been observed*
by a given fiber (*including targets which end up being observed*).

As part of these VACs, we include `targetphot` and `tractorphot` catalogs for
all these potential targets as documented above and as summarized in the tables
below:

#### Fuji (*targetphot*)

| File Name | File Size | Number of Targets | Notes |
|-----------|:---------:|:-----------------:|-------|
| potential-targets/targetphot-potential-cmx-fuji.fits | 22.1 MB | 20,956 | Commissioning Survey |
| potential-targets/targetphot-potential-special-fuji.fits | 378 MB | 358,817 | Special targets |
| potential-targets/targetphot-potential-sv1-fuji.fits | 4.78 GB | 4,645,741 | Survey Validation 1 |
| potential-targets/targetphot-potential-sv2-fuji.fits | 790 MB | 750,431 | Survey Validation 2 |
| potential-targets/targetphot-potential-sv3-fuji.fits | 11 GB | 10,684,616 | Survey Validation 3 |
| potential-targets/targetphot-potential-fuji.fits | 16.9 GB | 16,460,561 | Stack of the preceding 5 catalogs. |

#### Iron (*targetphot*)

| File Name | File Size | Number of Targets | Notes |
|-----------|:---------:|:-----------------:|-------|
| potential-targets/targetphot-potential-cmx-fuji.fits | 22.1 MB | 20,956 | Commissioning Survey |
| potential-targets/targetphot-potential-special-fuji.fits | 378 MB | 358,817 | Special targets |
| potential-targets/targetphot-potential-sv1-fuji.fits | 4.78 GB | 4,645,741 | Survey Validation 1 |
| potential-targets/targetphot-potential-sv2-fuji.fits | 790 MB | 750,431 | Survey Validation 2 |
| potential-targets/targetphot-potential-sv3-fuji.fits | 11 GB | 10,684,616 | Survey Validation 3 |
| potential-targets/targetphot-potential-nside2-hp[0-9][0-9]-main-iron.fits | 137 GB | 133,235,021 | Main Survey |

#### Guadalupe (*targetphot*)

| File Name | File Size | Number of Targets | Notes |
|-----------|:---------:|:-----------------:|-------|
| potential-targets/targetphot-potential-special-guadalupe.fits | 84.4 MB | 80,182 | Special targets |
| potential-targets/targetphot-potential-main-guadalupe.fits | 17.1 GB | 16,603,258 | Main Survey |
| potential-targets/targetphot-potential-guadalupe.fits | 17.2 GB | 16,683,440 | Stack of the preceding 2 catalogs. |

#### Tractor (*tractorphot*) Catalogs

| Data Release | Relative Location of *tractorphot* Files | Number of Files | Total Data Volume | Total Number of Objects |
|--------------|------------------------------------------|:---------------:|:-----------------:|:-----------------------:|
| Fuji | potential-targets/tractorphot/tractorphot-potential-nside4-hp[0-9][0-9][0-9]-fuji.fits | 71 | 11.9 GB | 6,031,271 |
| iron | potential-targets/tractorphot/tractorphot-potential-nside4-hp[0-9][0-9][0-9]-iron.fits | 59 | 82.4 GB | 41,779,045 |
| Guadalupe | potential-targets/tractorphot/tractorphot-potential-nside4-hp[0-9][0-9][0-9]-guadalupe.fits | 43 | 31.1 GB | 15,758,409 |

Reproducibility
---------------

DESI collaborators (or others with all the necessary underlying files and
software dependencies) can reproduce either of the VACs presented here by
invoking the following commands at [NERSC](https://nersc.gov/) (illustrated here
just for Fuji).

1. First, set up your software environment:
```bash
source /global/common/software/desi/desi_environment.sh 23.1
module unload desispec
module load desispec/0.56.4
git clone https://github.com/moustakas/desi-photometry.git
cd desi-photometry && git checkout tags/v2.0 && cd ..
```

2. Next, set up the `perlmutter` interactive node to run the code:
```bash
salloc -N 1 -C cpu -A desi -t 04:00:00 --qos interactive -L SCRATCH,cfs
```

3. Next, gather targeting and Tractor photometry for *observed* targets:
```bash
time /path/to/desi/code/desi-photometry/lsdr9-photometry --reduxdir $DESI_ROOT/spectro/redux/fuji \
  -o ${SCRATCH}/lsdr9/fuji --specprod fuji --mp 128 --targetphot --tractorphot
```

4. Finally gather targeting and Tractor photometry for *potential* targets (you may want to start another interactive node):
```bash
time /path/to/desi/code/desi-photometry/lsdr9-photometry --reduxdir $DESI_ROOT/spectro/redux/fuji \
  -o ${SCRATCH}/lsdr9/fuji --specprod fuji --mp 128 --targetphot --tractorphot --potential
```

Known Issues
------------

None currently known.

Contact & Contributors
----------------------

For questions or problems regarding these catalogs or their construction, please
file a ticket at the [desi-photometry
repository](https://github.com/moustakas/desi-photometry/issues) and/or contact
[John Moustakas](jmoustakas@siena.edu) ([Siena College](https://siena.edu)).

John Moustakas gratefully acknowledges funding support for this work from the
U.S. Department of Energy, Office of Science, Office of High Energy Physics
under Award Number DE-SC0020086.

We are also grateful for important contributions to the VACs presented herein
from the following individuals:

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

```
DESI research is supported by the Director, Office of Science, Office of
High Energy Physics of the U.S. Department of Energy under Contract
No. DE–AC02–05CH11231, and by the National Energy Research Scientific Computing
Center, a DOE Office of Science User Facility under the same contract;
additional support for DESI is provided by the U.S. National Science Foundation,
Division of Astronomical Sciences under Contract No. AST-0950945 to the NSF’s
National Optical-Infrared Astronomy Research Laboratory; the Science and
Technologies Facilities Council of the United Kingdom; the Gordon and Betty
Moore Foundation; the Heising-Simons Foundation; the French Alternative Energies
and Atomic Energy Commission (CEA); the National Council of Science and
Technology of Mexico (CONACYT); the Ministry of Science and Innovation of Spain
(MICINN), and by the DESI Member Institutions
(https://www.desi.lbl.gov/collaborating-institutions).
```
