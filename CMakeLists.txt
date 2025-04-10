CMAKE_MINIMUM_REQUIRED(VERSION 3.5.0)
PROJECT(NV)
ENABLE_TESTING()

SET(NV_CMAKE_DIR "${NV_SOURCE_DIR}/cmake")
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${NV_CMAKE_DIR}")

set (CMAKE_CXX_STANDARD 11)

INCLUDE(${NV_CMAKE_DIR}/OptimalOptions.cmake)
MESSAGE(STATUS "Setting optimal options")
MESSAGE(STATUS "  Processor: ${NV_SYSTEM_PROCESSOR}")
MESSAGE(STATUS "  Compiler Flags: ${CMAKE_CXX_FLAGS}")

IF(CMAKE_BUILD_TYPE MATCHES "debug")
    SET(CMAKE_DEBUG_POSTFIX "_d" CACHE STRING "Postfix for debug build libraries.")
    ADD_DEFINITIONS(-D_DEBUG=1)
ENDIF()


ADD_SUBDIRECTORY(extern)

ADD_SUBDIRECTORY(src)

# These files should only be installed when creating packages.
INSTALL(FILES 
	LICENSE
	README.md
	DESTINATION share/doc/nvtt)

# Add packaging support
INCLUDE(InstallRequiredSystemLibraries)

IF(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	SET(CPACK_GENERATOR "TGZ;DEB")
ENDIF(CMAKE_SYSTEM_NAME STREQUAL "Linux")

SET(CPACK_PACKAGE_NAME "nvidia-texture-tools")
SET(CPACK_PACKAGE_VERSION_MAJOR "2")
SET(CPACK_PACKAGE_VERSION_MINOR "1")
SET(CPACK_PACKAGE_VERSION_PATCH "3")
SET(CPACK_PACKAGE_VERSION "2.1.3")
SET(CPACK_PACKAGE_CONTACT "Julian Gro� <julian.gro@overte.org>")
SET(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Texture processing tools with support for Direct3D 10 and 11 formats.")

SET(CPACK_PACKAGE_DESCRIPTION_FILE "${NV_SOURCE_DIR}/README.md")
SET(CPACK_RESOURCE_FILE_LICENSE "${NV_SOURCE_DIR}/LICENSE")

# NSIS options:
IF(WIN32)
	SET(CPACK_NSIS_DISPLAY_NAME "${CPACK_PACKAGE_VENDOR}\\\\NVIDIA Texture Tools 2.1")
	SET(CPACK_PACKAGE_INSTALL_DIRECTORY "${CPACK_PACKAGE_VENDOR}\\\\NVIDIA Texture Tools 2.1")
	SET(CPACK_PACKAGE_ICON "${NV_SOURCE_DIR}\\\\project\\\\vc2017\\\\nvcompress\\\\nvidia.ico")
ENDIF(WIN32)

INCLUDE(CPack)

