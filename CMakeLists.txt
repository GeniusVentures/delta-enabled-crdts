cmake_minimum_required(VERSION 3.2)
set(
    CMAKE_TOOLCHAIN_FILE
    "${CMAKE_SOURCE_DIR}/cmake/toolchain/cxx17.cmake"
    CACHE
    FILEPATH
    "Default toolchain"
)
include(GNUInstallDirs)

#---------------------------------------------------------------------------------------
# Compiler config
#---------------------------------------------------------------------------------------
# set(CMAKE_CXX_STANDARD 11)
# set(CMAKE_CXX_STANDARD_REQUIRED ON)
# set(CMAKE_CXX_EXTENSIONS OFF)
if(NOT EXISTS "${CMAKE_TOOLCHAIN_FILE}")
    set(CMAKE_CXX_STANDARD 17)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    set(CMAKE_CXX_EXTENSIONS OFF)
endif()
# Since delta-enabled-crdts is a template library we only need to copy headers to install derectory
project(delta-enabled-crdts VERSION 0.1.0 LANGUAGES C CXX)


include_directories(
    ${CMAKE_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}/include/crdts
)
add_subdirectory(${CMAKE_SOURCE_DIR}/src)

# Install Headers
install(DIRECTORY "${CMAKE_SOURCE_DIR}/include/crdts" DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}" FILES_MATCHING PATTERN "*.h*")
# Install LIB
install(TARGETS delta-enabled-crdts
    EXPORT delta-enabled-crdtsTargets DESTINATION "${CMAKE_INSTALL_LIBDIR}")
	
set(delta-enabled-crdts_CONFIG_DESTINATION_DIR "lib/cmake/delta-enabled-crdts")
install(EXPORT delta-enabled-crdtsTargets
    FILE delta-enabled-crdtsTargets.cmake
    DESTINATION ${delta-enabled-crdts_CONFIG_DESTINATION_DIR}
)

include(CMakePackageConfigHelpers)

# generate the config file that is includes the exports
configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/cmake/config.cmake.in
    "${CMAKE_CURRENT_BINARY_DIR}/delta-enabled-crdtsConfig.cmake"
    INSTALL_DESTINATION "lib/cmake/"
    NO_SET_AND_CHECK_MACRO
    NO_CHECK_REQUIRED_COMPONENTS_MACRO
)

# generate the version file for the config file
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/delta-enabled-crdtsVersion.cmake"
    VERSION "${VERSION_STRING}.${SUBVERSION_STRING}"
    COMPATIBILITY AnyNewerVersion
)
# install the configuration file
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/delta-enabled-crdtsConfig.cmake
    DESTINATION ${delta-enabled-crdts_CONFIG_DESTINATION_DIR}
)




