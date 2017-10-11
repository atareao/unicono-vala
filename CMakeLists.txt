
# We define the minimum cmake. This is standard for elementary OS
cmake_minimum_required (VERSION 2.8)

#We define the minimum policy version
cmake_policy(VERSION 2.8)


#We define out project name.
SET(CMAKE_INSTALL_PREFIX "/usr" CACHE PATH "")
project(equival)

set (PACKAGE "equival")
set (RELEASE_NAME "Flow")
set (VERSION_MAJOR "0")
set (VERSION_MINOR "1")
set (VERSION_STATUS "0")
set (VERSION_BUILD "0extras17.04.0")

set (NAME "Unicono")
set (COMMENT "An example")
set (GENERICNAME "Un icono en Vala")
set (DESCRIPTION "Un ejemplo en Vala")


set (VERSION "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_STATUS}-${VERSION_BUILD}")
set (AUTHOR "Lorenzo Carbonell <lorenzo.carbonell.cerezo@gmail.com>")
set (COPYRIGHT "Copyright (c) 2017 Lorenzo Carbonell")
set (LICENSE "GPLV3")
set (URL "http://www.atareao.es")

set (BINDIR "${CMAKE_INSTALL_PREFIX}/bin")
set (DATADIR "${CMAKE_INSTALL_PREFIX}/share")
set (PKGDATADIR "${DATADIR}/${PACKAGE}")
set (GETTEXT_PACKAGE "${PACKAGE}")
add_definitions(-DGETTEXT_PACKAGE=\"${GETTEXT_PACKAGE}\")
set (CMAKE_C_FLAGS "-ggdb")

configure_file (${CMAKE_SOURCE_DIR}/src/config.vala.cmake ${CMAKE_SOURCE_DIR}/src/config.vala)
configure_file (${CMAKE_SOURCE_DIR}/data/${PACKAGE}.desktop.cmake ${CMAKE_SOURCE_DIR}/data/${PACKAGE}.desktop.in)

#Here we load the modules we downloaded earlier and placed into the ./cmake directory
list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake )

# Vala
find_package (Vala REQUIRED)
include (ValaVersion)
ensure_vala_version ("0.34" MINIMUM)
include (ValaPrecompile)

find_package(PkgConfig)

configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY
)

add_custom_target(
    uninstall
    COMMAND
        ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake
    COMMAND
        ${glib_schema_compiler} ${GSETTINGS_DIR}
)


#Aquí añadimos los directorios que queremos que mire CMake y que contengan el directorio CMakeLists.txt
add_subdirectory(data)
