cmake_minimum_required(VERSION 3.5)

set(KF5_VERSION "5.70.0") # handled by release scripts
project(PlasmaWaylandProtocols VERSION ${KF5_VERSION})

# ECM setup
include(FeatureSummary)
find_package(ECM 5.69.0  NO_MODULE)
set_package_properties(ECM PROPERTIES TYPE REQUIRED DESCRIPTION "Extra CMake Modules." URL "https://commits.kde.org/extra-cmake-modules")
feature_summary(WHAT REQUIRED_PACKAGES_NOT_FOUND FATAL_ON_MISSING_REQUIRED_PACKAGES)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake/Modules)

include(FeatureSummary)
include(ECMGenerateExportHeader)
include(CMakePackageConfigHelpers)
include(ECMSetupVersion)
include(ECMGenerateHeaders)
include(CMakeFindFrameworks)
include(ECMQtDeclareLoggingCategory)

ecm_setup_version(PROJECT VARIABLE_PREFIX PLASMA_WAYLAND_PROTOCOLS
                        VERSION_HEADER "${CMAKE_CURRENT_BINARY_DIR}/plasma_wayland_protocols_version.h"
                        PACKAGE_VERSION_FILE "${CMAKE_CURRENT_BINARY_DIR}/KF5PlasmaWaylandProtocolsConfigVersion.cmake"
                        SOVERSION 5)

# Dependencies
set(REQUIRED_QT_VERSION 5.12.0)
find_package(Qt5 ${REQUIRED_QT_VERSION} CONFIG REQUIRED Concurrent Gui WaylandClient)

find_package(QtWaylandScanner REQUIRED)

find_package(Wayland 1.15 COMPONENTS Client Server)
set_package_properties(Wayland PROPERTIES
                       TYPE REQUIRED
                      )

find_package(WaylandScanner)

find_package(WaylandProtocols 1.15)
set_package_properties(WaylandProtocols PROPERTIES TYPE REQUIRED)

include(KDEInstallDirs)
include(KDEFrameworkCompilerSettings NO_POLICY_SCOPE)
include(KDECMakeSettings)
include(CheckIncludeFile)
find_package(QtWaylandScanner REQUIRED)
add_subdirectory(src)

# create a Config.cmake and a ConfigVersion.cmake file and install them
set(CMAKECONFIG_INSTALL_DIR "${CMAKECONFIG_INSTALL_PREFIX}/KF5PlasmaWaylandProtocols")

configure_package_config_file("${CMAKE_CURRENT_SOURCE_DIR}/KF5PlasmaWaylandProtocolsConfig.cmake.in"
                              "${CMAKE_CURRENT_BINARY_DIR}/KF5PlasmaWaylandProtocolsConfig.cmake"
                              INSTALL_DESTINATION  ${CMAKECONFIG_INSTALL_DIR}
                              )

install(FILES  "${CMAKE_CURRENT_BINARY_DIR}/KF5PlasmaWaylandProtocolsConfig.cmake"
               "${CMAKE_CURRENT_BINARY_DIR}/KF5PlasmaWaylandProtocolsConfigVersion.cmake"
        DESTINATION "${CMAKECONFIG_INSTALL_DIR}"
        COMPONENT Devel )

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/plasma_wayland_protocols_version.h
        DESTINATION ${KF5_INCLUDE_INSTALL_DIR} COMPONENT Devel )

feature_summary(WHAT ALL FATAL_ON_MISSING_REQUIRED_PACKAGES)
