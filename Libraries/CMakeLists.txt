CMAKE_MINIMUM_REQUIRED( VERSION 3.24 )

PROJECT( fltk_find_mwe_LIBRARIES )

set( CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH} )

INCLUDE( ExternalProject )

IF( NOT USE_SYSTEM_FLTK )
	INCLUDE( External_FLTK )
ENDIF()

# Set targeted variables in file to be read by VSP's CMake
configure_file(
   ${CMAKE_SOURCE_DIR}/cmake/Libraries_Config.cmake.in
   ${CMAKE_BINARY_DIR}/Libraries_Config.cmake
   @ONLY
)

