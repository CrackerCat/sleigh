#
# Copyright (c) 2022-present, Trail of Bits, Inc.
# All rights reserved.
#
# This source code is licensed in accordance with the terms specified in
# the LICENSE file found in the root directory of this source tree.
#

#
# Tests from ghidra repo
#

add_executable(sleigh_ghidra_test
  ${sleigh_core_source_list}
  ${sleigh_deccore_source_list}
  ${sleigh_source_list}
  ${sleigh_extra_source_list}

  "${library_root}/test.cc"

  "${library_root}/../unittests/testcirclerange.cc"
  "${library_root}/../unittests/testfloatemu.cc"
  "${library_root}/../unittests/testtypes.cc"
  "${library_root}/../unittests/testmarshal.cc"
)

#if(sleigh_RELEASE_IS_HEAD)
#  target_sources(sleigh_ghidra_test PRIVATE
#  )
#endif()

target_compile_features(sleigh_ghidra_test PRIVATE cxx_std_11)
target_include_directories(sleigh_ghidra_test PRIVATE "${library_root}")
include(CheckIncludeFileCXX)
check_include_file_cxx(termios.h HAVE_TERMIOS_H)
if(HAVE_TERMIOS_H)
  target_compile_definitions(sleigh_ghidra_test PRIVATE
    __TERMINAL__
  )
endif()
sleigh_add_optional_defines(sleigh_ghidra_test PRIVATE)

add_test(
  NAME sleigh_ghidra_unittest
  COMMAND sleigh_ghidra_test -sleighpath "${PROJECT_BINARY_DIR}" unittests
)

add_test(
  NAME sleigh_ghidra_datatest
  COMMAND sleigh_ghidra_test -sleighpath "${PROJECT_BINARY_DIR}"
    -path "${library_root}/../datatests"
    datatests
)
