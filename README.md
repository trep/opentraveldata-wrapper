OpenTravelData (OPTD) data injector
===================================

[![Docker Repository on Quay](https://quay.io/repository/opentraveldata/quality-assurance/status "Docker Repository on Quay")](https://quay.io/repository/opentraveldata/quality-assurance)

Wrapper around OpenTravelData (OPTD) data sets,
to be used by the OpenTREP components.

# References
* Open Travel REquest Parser (OpenTREP):
  + GitHub: https://trep.github.io/opentrep
  + Docker Cloud repository: https://cloud.docker.com/u/opentrep/repository/docker/opentrep/metatrep
  + Web applications:
    - https://transport-search.org
    - https://www2.transport-search.org
* OpenTravelData (OPTD):
  + GitHub: https://opentraveldata.github.io/opentraveldata
  + Docker Cloud repository: https://cloud.docker.com/u/opentraveldata/repository/docker/opentraveldata/quality-assurance
  + Published data sets: https://transport-search.org/data/optd/por/
* Meta-TREP umbrella project (to build and install OpenTREP and
  get and use OPTD data):
  + GitHub: https://github.com/trep/metatrep

# Manual build and installation

## OpenTREP
* See the
  [OpenTREP README](https://github.com/trep/opentrep/blob/master/README.md#native-installation-without-docker)
  for more details on how to install OpenTREP on most platforms.
* OpenTREP may also be installed by the
  [meta-TREP project](https://github.com/trep/metatrep),
  which moreover downloads and injects OPTD data into it
* Typically, OpenTREP installs a
  [small sample data file](https://github.com/trep/opentrep/blob/master/data/por/test_optd_por_public.csv)
  for the POR (points of reference) in its data directory
  (_e.g._, `/usr/share/opentrep/data/por`,
  `~/dev/deliveries/opentrep/share/opentrep/data/por`)
  for testing purposes.
* [OpenTravelData (OPTD)](https://opentraveldata.github.io/opentraveldata)
  regularly publishes POR data files, both on
  [GitHub](https://github.com/opentraveldata/opentraveldata/blob/master/opentraveldata/)
  and on
  [transport-search.org](https://transport-search.org/data/optd/por/)
* [This project (OPTD data injector)](https://github.com/trep/opentraveldata-wrapper)
  downloads the `optd_por_public_all.csv` POR data set from the OPTD sites
  (see above for the URLs)
  and injects it into the OpenTREP local installation directory,
  so that OpenTREP can then use it as a reference (for indexing
  and searching)
* The [Meta-TREP project](https://github.com/trep/metatrep) eases the full
  process:
  + Download and install OpenTREP
  + Download and inject the OPTD POR data set into the OpenTREP
    installation directory

## OPTD injector
* Clone the Git repository (one-time task):
```bash
$ mkdir -p ~/dev/geo && \
  git https://github.com/trep/opentraveldata-wrapper.git ~/dev/geo/opentraveldata-wrapper
```

* Download and install the OPTD POR data file (where `<opentrep-install-dir>`
  is the installation directory of OpenTREP, for instance `/usr`, `/usr/llocal`,
  `${HOME}/deliveries/opentrep-<version>`, _etc_):
```bash
$ pushd ~/dev/geo/opentraveldata-wrapper
$ export VERSION="99.99.99"
$ cmake -DVERSION="${VERSION}" -DCMAKE_INSTALL_PREFIX=${HOME}/dev/deliveries -DWITH_OPENTREP_PREFIX=<opentrep-install-dir> ..
$ ls -laFh ~/dev/deliveries/opentraveldata/share/data/por
total 85368
-rw-r--r--  1 user  staff  42M Sep  1 15:11 optd_por_public_all.csv
$ popd
```

# Use cases

## Indexing

### Local installation

### Meta-TREP installation

```bash
TREP_DIR="workspace/install/opentrep"
POR_FILE="${TREP_DIR}/share/opentrep/data/por/optd_por_public_all.csv"
${TREP_DIR}/bin/opentrep-indexer -p ${POR_FILE} -t sqlite
```

### Web application

```bash
TREP_DIR="/var/www/webapps/opentrep/trep"
POR_FILE="${TREP_DIR}/share/opentrep/data/por/optd_por_public_all.csv"
BUILD_DIR="${HOME}/dev/trep/opentrepgit/build"
${BUILD_DIR}/opentrep/opentrep-indexer -p ${POR_FILE} -d ${TREP_DIR}/traveldb -t sqlite -s ${TREP_DIR}/sqlite_travel.db
```

## Searching

### Meta-TREP installation

```bash
TREP_DIR="workspace/install/opentrep"
POR_FILE="${TREP_DIR}/share/opentrep/data/por/optd_por_public_all.csv"
SEARCH_STR="nce sna francisco"
${TREP_DIR}/bin/opentrep-searcher -t sqlite -q "${SEARCH_STR}"
```

### Web application

```bash
TREP_DIR="/var/www/webapps/opentrep/trep"
BUILD_DIR="${HOME}/dev/trep/opentrepgit/build"
SEARCH_STR="nce sna francisco"
${BUILD_DIR}/opentrep/opentrep-searcher -d ${TREP_DIR}/traveldb -t sqlite -s ${TREP_DIR}/sqlite_travel.db -q "${SEARCH_STR}"
```


