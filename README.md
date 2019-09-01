OpenTravelData (OPTD) data wrapper
==================================

[![Docker Repository on Quay](https://quay.io/repository/opentraveldata/quality-assurance/status "Docker Repository on Quay")](https://quay.io/repository/opentraveldata/quality-assurance)

Wrapper around OpenTravelData (OPTD) data sets, for instance
to be used by the OpenTREP components.

# References
* Open Travel REquest Parser (OpenTREP):
  + Source code on GitHub: https://github.com/trep
  + Docker Cloud repository: https://cloud.docker.com/u/opentrep/repository/docker/opentrep/metatrep
  + Web applications:
    - https://transport-search.org
    - https://www2.transport-search.org
* OpenTravelData (OPTD):
  + Source code on GitHub: https://opentraveldata/opentraveldata
  + Docker Cloud repository: https://cloud.docker.com/u/opentraveldata/repository/docker/opentraveldata/quality-assurance

# Manual build and installation
* Clone the Git repository (one-time task):
```bash
$ mkdir -p ~/dev/geo && \
  git https://github.com/trep/opentraveldata-wrapper.git ~/dev/geo/opentraveldata-wrapper
```

* Download and install the OPTD POR data file:
```bash
$ pushd ~/dev/geo/opentraveldata-wrapper
$ export VERSION="99.99.99"
$ cmake -DVERSION="${VERSION}" -DCMAKE_INSTALL_PREFIX=~/dev/deliveries ..
$ ls -laFh ~/dev/deliveries/opentraveldata/share/data/por
total 85368
-rw-r--r--  1 user  staff  42M Sep  1 15:11 optd_por_public_all.csv
$ popd
```


