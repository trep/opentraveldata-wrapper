#
cmake_minimum_required (VERSION 3.0)

##
# Project info
project (opentraveldata-wrapper)
if (NOT VERSION)
  set (VERSION "99.99.99")
endif (NOT VERSION)
set (PROJECT_VERSION ${VERSION})

##
# OpenTravelData (OPTD)
set (OPTD_FILE "optd_por_public_all.csv")
set (OPTD_PJCT_URL "https://transport-search.org")
set (OPTD_PJCT_PATH "data/optd/por")

## OpenTREP
find_path (OPENTREP_DATA_DIR
  NAMES "test_optd_por_public.csv"
  PATHS "${WITH_OPENTREP_PREFIX}/share/opentrep/data/por"
  DOC "Full path to the OpenTREP POR data file")

if (OPENTREP_DATA_DIR-NOTFOUND)
  set (ERROR_MSG "The OpenTREP POR data directory cannot be found. If OpenTREP")
  set (ERROR_MSG "${ERROR_MSG} is in a non standard directory, just invoke")
  set (ERROR_MSG "${ERROR_MSG} 'cmake' specifying the -DWITH_OPENTREP_PREFIX=")
  set (ERROR_MSG "${ERROR_MSG}<OpenTREP install path> variable.")
  message (STATUS "${ERROR_MSG}")
  #message (FATAL_ERROR "${ERROR_MSG}")
else (OPENTREP_DATA_DIR-NOTFOUND)
  #
  message (STATUS "Found OpenTREP POR data directory: ${OPENTREP_DATA_DIR}")
endif (OPENTREP_DATA_DIR-NOTFOUND)

#
function (download_file url filename)
  if (NOT EXISTS ${filename})
    file (DOWNLOAD ${url} ${filename}
      TIMEOUT 120 # seconds
      TLS_VERIFY ON
      SHOW_PROGRESS
    )
  endif()
  execute_process (COMMAND bash -c "bunzip2 ${filename}")
endfunction (download_file)

#
message (STATUS "Downloading ${OPTD_FILE} from ${OPTD_PJCT_URL}...")
download_file (
  ${OPTD_PJCT_URL}/${OPTD_PJCT_PATH}/${OPTD_FILE}.bz2
  ./${OPTD_FILE}.bz2
)
message (STATUS "... ${OPTD_FILE} downloaded")

# The downloaded OPTD POR data file will be installed
# in the OpenTREP POR data directory
install (FILES ${CMAKE_CURRENT_BINARY_DIR}/${OPTD_FILE}
	DESTINATION ${OPENTREP_DATA_DIR})

##
# Reporting
message (STATUS)
message (STATUS "Project information:")
message (STATUS "Name ................. : ${PROJECT_NAME}")
message (STATUS "Version .............. : ${PROJECT_VERSION}")
message (STATUS "OpenTREP directory ... : ${WITH_OPENTREP_PREFIX}")
message (STATUS "Data directory ....... : ${OPENTREP_DATA_DIR}")
message (STATUS "OPTD POR data file ... : ${OPTD_FILE}")
message (STATUS)


