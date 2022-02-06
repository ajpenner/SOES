

set(soes_includes
  esc.h
  esc_coe.h
  esc_foe.h
  esc_eoe.h
  esc_eep.h
)

# NOTE: add headers to make them show up in an IDE
add_library (soes
  esc.c
  esc_coe.c
  esc_foe.c
  esc_eoe.c
  esc_eep.c
  ecat_slv.c
  ecat_slv.h
  options.h
  ${soes_includes}
  ${HAL_SOURCES}
  )

include_directories(${HAL_INCLUDES})

install (TARGETS soes DESTINATION lib)
install (FILES
  ${soes_includes}
  DESTINATION include)

# first we can indicate the documentation build as an option and set it to ON by default
option(BUILD_DOC "Build documentation" ON)

# check if Doxygen is installed
find_package(Doxygen)
if (DOXYGEN_FOUND)
    # set input and output files
    set(DOXYGEN_IN ${CMAKE_CURRENT_SOURCE_DIR}/../docs/Doxyfile.in)
    set(DOXYGEN_OUT ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile)

    # request to configure the file
    configure_file(${DOXYGEN_IN} ${DOXYGEN_OUT} @ONLY)
    message("Doxygen build started")

    # note the option ALL which allows to build the docs together with the application
    add_custom_target( doc ALL
        COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYGEN_OUT}
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating API documentation with Doxygen"
        VERBATIM )
else (DOXYGEN_FOUND)
  message("Doxygen need to be installed to generate the doxygen documentation")
endif (DOXYGEN_FOUND)
