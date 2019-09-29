#
cmake_minimum_required (VERSION 3.0)

##
# Project info
project (opentraveldata)
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
find_path (OPTD_POR_DIR
  NAMES "test_optd_por_public.csv"
  PATHS "${WITH_OPENTREP_PREFIX}/share/opentrep/data/por"
  DOC "Full path to the OpenTravelData (OPTD) file")

if (OPTD_POR_DIR-NOTFOUND)
  set (ERROR_MSG "The OPTD POR data dir cannot be found. If OPTD is")
  set (ERROR_MSG "${ERROR_MSG} in a non standard directory, just invoke")
  set (ERROR_MSG "${ERROR_MSG} 'cmake' specifying the -DWITH_OPENTREP_PREFIX=")
  set (ERROR_MSG "${ERROR_MSG}<OPTD install path> variable.")
  message (STATUS "${ERROR_MSG}")
  #message (FATAL_ERROR "${ERROR_MSG}")
else (OPTD_POR_DIR-NOTFOUND)
  #
  message (STATUS "Found OPTD POR data dir: ${OPTD_POR_DIR}")
endif (OPTD_POR_DIR-NOTFOUND)

#
function (download_file url filename)
  if (NOT EXISTS ${filename})
    file (DOWNLOAD ${url} ${filename}
      TIMEOUT 60  # seconds
      TLS_VERIFY ON
      SHOW_PROGRESS
    )
  endif()
  execute_process (COMMAND bash -c "bunzip2 ${filename}")
endfunction (download_file)

#
set (OPTD_DATA_DIR ${CMAKE_INSTALL_PREFIX}/share/${PROJECT_NAME}/data/por)
install (FILES ${CMAKE_CURRENT_BINARY_DIR}/${OPTD_FILE} DESTINATION ${OPTD_DATA_DIR})

#
message (STATUS "Downloading ${OPTD_FILE} from ${OPTD_PJCT_URL}...")
download_file (
  ${OPTD_PJCT_URL}/${OPTD_PJCT_PATH}/${OPTD_FILE}.bz2
  ./${OPTD_FILE}.bz2
)
message (STATUS "... ${OPTD_FILE} downloaded")

##
# Reporting
message (STATUS)
message (STATUS "Project information:")
message (STATUS "Name ................. : ${PROJECT_NAME}")
message (STATUS "Version .............. : ${PROJECT_VERSION}")
message (STATUS "Data directory ....... : ${OPTD_DATA_DIR}")
message (STATUS "POR data file ........ : ${OPTD_FILE}")
message (STATUS)


