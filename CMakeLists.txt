cmake_minimum_required(VERSION 3.14)

project(BoostPFR
        VERSION 2.0.1
        DESCRIPTION "std::tuple like methods for user defined types without any macro or boilerplate code"
        HOMEPAGE_URL "https://github.com/boostorg/pfr"
        LANGUAGES CXX)

# ---- Warning guard ----

# Protect dependents from this project's warnings if the guard isn't disabled
set(BoostPFR_warning_guard SYSTEM)
if(BoostPFR_INCLUDE_WITHOUT_SYSTEM)
  set(BoostPFR_warning_guard "")
endif()

# ---- Declare library ----

add_library(pfr INTERFACE)
add_library(Boost::pfr ALIAS pfr)

target_include_directories(pfr
        ${BoostPFR_warning_guard}
        INTERFACE
        "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>")

target_compile_features(pfr INTERFACE cxx_std_14)

# ---- Install ----

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

set(pfr_directory "BoostPFR-${PROJECT_VERSION}")
set(pfr_include_directory
        "${CMAKE_INSTALL_INCLUDEDIR}/${pfr_directory}")

install(DIRECTORY "${PROJECT_SOURCE_DIR}/include/"
        DESTINATION "${pfr_include_directory}"
        COMPONENT BoostPFR_Development)

install(TARGETS pfr
        EXPORT BoostPFRTargets
        INCLUDES DESTINATION "${pfr_include_directory}")

write_basic_package_version_file(
        BoostPFRConfigVersion.cmake
        COMPATIBILITY SameMajorVersion
        ARCH_INDEPENDENT)

set(pfr_install_cmakedir
        "${CMAKE_INSTALL_LIBDIR}/cmake/${pfr_directory}")

install(FILES "${PROJECT_BINARY_DIR}/BoostPFRConfigVersion.cmake"
        DESTINATION "${pfr_install_cmakedir}"
        COMPONENT BoostPFR_Development)

install(EXPORT BoostPFRTargets
        FILE BoostPFRConfig.cmake
        NAMESPACE Boost::
        DESTINATION "${pfr_install_cmakedir}"
        COMPONENT BoostPFR_Development)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
  include(CPack)
endif()
