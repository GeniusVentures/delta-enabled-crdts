cmake_minimum_required(VERSION 3.2)

include(GNUInstallDirs)

#---------------------------------------------------------------------------------------
# Compiler config
#---------------------------------------------------------------------------------------
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Since delta-enabled-crdts is a template library we only need to copy headers to install derectory
project(delta-enabled-crdts VERSION 0.1.0)


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




