CMAKE_MINIMUM_REQUIRED(VERSION 3.24)

project( find_fltk_mwe )

set( CMAKE_MODULE_PATH ${find_fltk_mwe_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH} )



IF( LIBRARY_PATH )
    INCLUDE( ${LIBRARY_PATH}/Libraries_Config.cmake )
ENDIF( LIBRARY_PATH )



SET( ORIG_CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} )

SET( CMAKE_PREFIX_PATH ${FLTK_INSTALL_DIR} ${ORIG_CMAKE_PREFIX_PATH} )
SET( FLTK_DIR ${FLTK_INSTALL_DIR})
FIND_PACKAGE(FLTK CONFIG REQUIRED)

SET( CMAKE_PREFIX_PATH ${ORIG_CMAKE_PREFIX_PATH} )


INCLUDE(PrintAllProperties)

print_target_properties( fltk )
print_target_properties( fltk::fluid )
print_target_properties( fluid )


function(check_target tgt)
    if(TARGET ${tgt})
        message(STATUS "Target exists: ${tgt}")
        # fl_debug_target(${tgt})
    else()
        message(STATUS "Target         ${tgt} does not exist")
    endif()
endfunction(check_target tgt)

check_target( fltk::fluid )
check_target( fluid )


# ADD_FLTK_FILE is a function that roughly replaces FLTK_WRAP_UI.
# ADD_FLTK_FILE only works one file at a time -- and it is passed the base file name (without extension).
# Also, ADD_FLTK_FILE passes causes fltk to generate source files with the .C and .H extension instead of
# .cxx and .h.
function( ADD_FLTK_FILE prjname basename)
    add_custom_command(
            OUTPUT ${basename}.C ${basename}.H
            MAIN_DEPENDENCY ${basename}.fl
            COMMAND fltk::fluid -o .C -h .H -c ${CMAKE_CURRENT_SOURCE_DIR}/${basename}.fl
    )
    set( ${prjname}_FLTK_UI_SRCS
            ${${prjname}_FLTK_UI_SRCS} ${basename}.C
            PARENT_SCOPE
    )
endfunction()



ADD_FLTK_FILE( tabsscreens
        tabs
)
# This line is roughly equivalent to:
#  FLTK_WRAP_UI( tabsscreens
#  tabs.fl
#  )

INCLUDE_DIRECTORIES(
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_BINARY_DIR}
        ${FLTK_INCLUDE_DIR}
)

ADD_LIBRARY( tabsscreens
        ${tabsscreens_FLTK_UI_SRCS}
)

ADD_EXECUTABLE( tabsprogram
        dummy.cpp
)

TARGET_LINK_LIBRARIES( tabsprogram
        tabsscreens
        ${FLTK_LIBRARIES}
        ${OPENGL_LIBRARIES}
)
