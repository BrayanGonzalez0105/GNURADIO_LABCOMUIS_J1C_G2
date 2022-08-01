# Copyright 2020 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

########################################################################
# Check if there is C++ code at all
########################################################################
if(NOT Modulos_J1C_02_sources)
    MESSAGE(STATUS "No C++ sources... skipping python bindings")
    return()
endif(NOT Modulos_J1C_02_sources)

########################################################################
# Check for pygccxml
########################################################################
GR_PYTHON_CHECK_MODULE_RAW(
    "pygccxml"
    "import pygccxml"
    PYGCCXML_FOUND
    )

include(GrPybind)

########################################################################
# Python Bindings
########################################################################

list(APPEND Modulos_J1C_02_python_files
    python_bindings.cc)

GR_PYBIND_MAKE_OOT(Modulos_J1C_02
   ../..
   gr::Modulos_J1C_02
   "${Modulos_J1C_02_python_files}")

# copy in bindings .so file for use in QA test module
add_custom_target(
  copy_bindings_for_tests ALL
  COMMAND
    ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_BINARY_DIR}/*.so"
    ${CMAKE_BINARY_DIR}/test_modules/Modulos_J1C_02/
  DEPENDS Modulos_J1C_02_python)

install(TARGETS Modulos_J1C_02_python DESTINATION ${GR_PYTHON_DIR}/Modulos_J1C_02 COMPONENT pythonapi)
