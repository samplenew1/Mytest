cmake_minimum_required(VERSION 3.0) # setting this is required
project(HMFAPP)            # this sets the project name

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY bin/)



###############################################################################
## file globbing ##############################################################
###############################################################################

# These instructions search the directory tree when cmake is
# invoked and put all files that match the pattern in the variables 
# `sources` and `data`.
file(GLOB_RECURSE sources      ${PROJECT_SOURCE_DIR}/src/*.cpp ${PROJECT_SOURCE_DIR}/include/*.h)
set (CEN_BASE_DIR   "${CMAKE_CURRENT_SOURCE_DIR}/../../Base")


set (CEN_HMF_DIR  ${CMAKE_CURRENT_SOURCE_DIR})

##################################################################################
#####       CEN BASE Includes and shared libraries
################################################################################


include_directories (AFTER "/cen/include")
include_directories ("/usr/local/include/hiredis")
include_directories ("/usr/include/glib-2.0")
include_directories ("/usr/lib/glib-2.0/include/")
include_directories ("/usr/lib/libzip/include/")
include_directories( include "/usr/include/libxml2")

#HMF Include for device.xml
include_directories (AFTER "../../Base/CenHandler/include/import/"
                  "/usr/include/curl/"
				  "include/"
                  "../../Base/CenHandler/CenMutex"
				  "../../Base/CenHandler/CenGarner/include/export/")

# Library directories.
set (LIB_DIRS "/cen/libs/")

###############################################################################
## target definitions #########################################################
###############################################################################
link_directories(${LIB_DIRS})
# The data is just added to the executable, because in some IDEs (QtCreator) 
# files are invisible when they are not explicitly part of the project.
add_executable(hmfservice ${sources} ) 

# Just for example add some compiler flags.
target_compile_options(hmfservice PUBLIC -std=c++11 -Wall -lpthread -Wno-write-strings )

# This allows to include files relative to the root of the src directory with a <> pair
target_include_directories(hmfservice PUBLIC src/ )

target_link_libraries(  hmfservice pthread hiredis xml2 rt curl  glib-2.0  json-c
    CenBaseService CenBaseMngr CenBaseSource CenHandler CenTcpClient CenLogger 
	CenOpcParser CenRestClient CenBaseThread CenException CenTimer CenGarner 
	CenZipper CenMutex
    )

