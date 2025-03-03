cmake_minimum_required(VERSION 3.24)

if (DEFINED ENV{METAMODULE_SDK_DIR})
    set(METAMODULE_SDK_DIR "$ENV{METAMODULE_SDK_DIR}")
elseif("${METAMODULE_SDK_DIR}" STREQUAL "")
    set(METAMODULE_SDK_DIR "${CMAKE_CURRENT_LIST_DIR}/../metamodule-plugin-sdk")
endif()
include(${METAMODULE_SDK_DIR}/plugin.cmake)

project(Nozori VERSION 1.0.0 DESCRIPTION "Nozori Modules" LANGUAGES C CXX)

# Set C++17 standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Define source directories
set(SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/VCV_nozori)

# Main library target
add_library(Nozori STATIC)

# Compiler definitions
target_compile_definitions(Nozori PRIVATE 
    __V1x 
    __PLUGIN
)

# Compiler options
target_compile_options(Nozori PRIVATE
    -fdata-sections
    -ffunction-sections
    -fno-exceptions
    -fno-threadsafe-statics
    -fno-use-cxa-atexit
    $<$<CXX_COMPILER_ID:GNU>:-fvisibility=hidden>
    $<$<CXX_COMPILER_ID:GNU>:-fvisibility-inlines-hidden>
)

# Linker options
target_link_options(Nozori PRIVATE
    -Wl,--gc-sections
    -static-libgcc
    -static-libstdc++
)

# Source files
target_sources(Nozori PRIVATE
    ${SOURCE_DIR}/src/plugin.cpp
    ${SOURCE_DIR}/src/Nozori_68_ADSR.cpp
    ${SOURCE_DIR}/src/Nozori_68_AR.cpp
    ${SOURCE_DIR}/src/Nozori_68_CRUSH.cpp
    ${SOURCE_DIR}/src/Nozori_68_CV_REC.cpp
    ${SOURCE_DIR}/src/Nozori_68_DELAY.cpp
    ${SOURCE_DIR}/src/Nozori_68_EQ.cpp
    ${SOURCE_DIR}/src/Nozori_68_GRAN.cpp
    ${SOURCE_DIR}/src/Nozori_68_KS.cpp
    ${SOURCE_DIR}/src/Nozori_68_LFO_MOD.cpp
    ${SOURCE_DIR}/src/Nozori_68_LFO.cpp
    ${SOURCE_DIR}/src/Nozori_68_NOISE.cpp
    ${SOURCE_DIR}/src/Nozori_68_PITCH.cpp
    ${SOURCE_DIR}/src/Nozori_68_RND_LOOP.cpp
    ${SOURCE_DIR}/src/Nozori_68_SIN_MOD.cpp
    ${SOURCE_DIR}/src/Nozori_68_VCA_PAN.cpp
    ${SOURCE_DIR}/src/Nozori_68_VCF_MOD.cpp
    ${SOURCE_DIR}/src/Nozori_68_VCF_MORPH.cpp
    ${SOURCE_DIR}/src/Nozori_68_VCO_LOOP.cpp
    ${SOURCE_DIR}/src/Nozori_68_VCO_MORPH.cpp
    ${SOURCE_DIR}/src/Nozori_68_VCO_QTZ.cpp
    ${SOURCE_DIR}/src/Nozori_68_VCO_WS.cpp
    ${SOURCE_DIR}/src/Nozori_68_VCO.cpp
    ${SOURCE_DIR}/src/Nozori_68_WS.cpp
    ${SOURCE_DIR}/src/Nozori_84_8_BITS.cpp
    ${SOURCE_DIR}/src/Nozori_84_ADD.cpp
    ${SOURCE_DIR}/src/Nozori_84_ADSR.cpp
    ${SOURCE_DIR}/src/Nozori_84_CLOCK.cpp
    ${SOURCE_DIR}/src/Nozori_84_DOPPLER.cpp
    ${SOURCE_DIR}/src/Nozori_84_FM_LIN.cpp
    ${SOURCE_DIR}/src/Nozori_84_FM.cpp
    ${SOURCE_DIR}/src/Nozori_84_HARMONICS.cpp
    ${SOURCE_DIR}/src/Nozori_84_JONG_LFO.cpp
    ${SOURCE_DIR}/src/Nozori_84_JONG_VCO.cpp
    ${SOURCE_DIR}/src/Nozori_84_LFO_SEQ.cpp
    ${SOURCE_DIR}/src/Nozori_84_LFO.cpp
    ${SOURCE_DIR}/src/Nozori_84_MODULATE.cpp
    ${SOURCE_DIR}/src/Nozori_84_PARAM.cpp
    ${SOURCE_DIR}/src/Nozori_84_SEQ_4.cpp
    ${SOURCE_DIR}/src/Nozori_84_SEQ_8.cpp
    ${SOURCE_DIR}/src/Nozori_84_SIN_AM.cpp
    ${SOURCE_DIR}/src/Nozori_84_SIN_FM.cpp
    ${SOURCE_DIR}/src/Nozori_84_SIN_PM.cpp
    ${SOURCE_DIR}/src/Nozori_84_SIN_WS.cpp
    ${SOURCE_DIR}/src/Nozori_84_THOMAS.cpp
    ${SOURCE_DIR}/src/Nozori_84_TROPICAL.cpp
    ${SOURCE_DIR}/src/Nozori_84_VCF.cpp
    ${SOURCE_DIR}/src/Nozori_84_WS.cpp
)

# Include directories
target_include_directories(Nozori PRIVATE 
    ${SOURCE_DIR}/src
)

# Create the plugin
create_plugin(
    SOURCE_LIB      Nozori
    PLUGIN_NAME     Nozoid
    PLUGIN_JSON     ${SOURCE_DIR}/plugin.json
    SOURCE_ASSETS   ${CMAKE_CURRENT_LIST_DIR}/assets
    DESTINATION     ${CMAKE_CURRENT_LIST_DIR}/metamodule-plugins
) 