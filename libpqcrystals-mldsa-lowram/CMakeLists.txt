cmake_minimum_required(VERSION 3.22)
project(libpqcrystals-mldsa-lowram LANGUAGES C ASM)
set(LIB pqcrystals-mldsa-lowram)
add_library(${LIB} STATIC 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/sign.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/packing.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/polyvec.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/poly.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/ntt.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/reduce.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/rounding.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/lowram.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/smallpoly.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/smallntt_3329.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/fips202.c 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/symmetric-shake.c
  )

target_include_directories(${LIB}
  PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# Compiler options
target_compile_options(${LIB} PRIVATE
    ${cpu_PARAMS}
    ${compiler_OPTS}
    -Wall
    -Wextra
    -Wpedantic
    -Wno-unused-parameter
    $<$<COMPILE_LANGUAGE:C>: >
    $<$<COMPILE_LANGUAGE:CXX>:

    # -Wno-volatile
    # -Wold-style-cast
    # -Wuseless-cast
    # -Wsuggest-override
    >
    $<$<COMPILE_LANGUAGE:ASM>:-x assembler-with-cpp -MMD -MP>
    $<$<CONFIG:Debug>:-Og -g3 -ggdb>
    $<$<CONFIG:Release>:-Ofast -g0>
)

include("${CMAKE_CURRENT_LIST_DIR}/cmake/git_version.cmake")
add_definitions( -DGIT_VERSION=${GIT_VERSION} )
add_definitions( -DVERSION_TIMESTAMP=${VERSION_TIMESTAMP} )
add_definitions( -DBUILD_TYPE=${CMAKE_BUILD_TYPE})
add_definitions( -DMLDSA_PSET=${MLDSA_PSET})
#add_definitions( -DDILITHIUM_RANDOMIZED_SIGNING)

