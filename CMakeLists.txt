##---------------------------------------------------------------------------
## Author:      Jon Gough
##***************************************************************************
## *   Copyright (C) 2010 by Sean d'Epagnier                              *
## *   License:     GPLv3+
## *   This program is free software; you can redistribute it and/or modify  *
## *   it under the terms of the GNU General Public License as published by  *
## *   the Free Software Foundation; either version 2 of the License, or     *
## *   (at your option) any later version.                                   *
## *                                                                         *
## *   This program is distributed in the hope that it will be useful,       *
## *   but WITHOUT ANY WARRANTY; without even the implied warranty of        *
## *   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the         *
## *   GNU General Public License for more details.                          *
## *                                                                         *
## *   You should have received a copy of the GNU General Public License     *
## *   along with this program; if not, write to the                         *
## *   xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,                                       *
## *   xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx                                       *
#-----------------------------------------------------------------------------
# Frontend2 Author: Jon Gough 
# Testplugin(TP) v1.0.254.11 06/09/2023 DD/MM/YYYY format
# buildCI:yes useODAPI:yes validateJSON:yes
# ---------------------------------------------------------------------------
## -- When changing this file do NOT change the order of the sections.    -- ##
## -- Make changes only between the section blocks identified             -- ##
## -- The whole configuration process relies on this sequence for success.-- ##
##
## -- Modify section below to include all the details for your plugin.    -- ##

set(CMLOC "CMakeLists: ")

# define minimum cmake version
cmake_minimum_required(VERSION 3.5.1)
if(COMMAND cmake_policy)
    if(POLICY CMP0043)
        cmake_policy(SET CMP0043 NEW)
    endif(POLICY CMP0043)
    cmake_policy(SET CMP0048 NEW)
# Need cmake 3.13 to implement this, but raspbian builds are at 3.7.2 and bionic at 3.10 and xenial builds at 3.5.1
#    cmake_policy(SET CMP0076 NEW)
    if(POLICY CMP0077)
        cmake_policy(SET CMP0077 NEW)
    endif(POLICY CMP0077)
endif(COMMAND cmake_policy)

# define plugin name, owner and versions
set(VERBOSE_NAME "Testplugin")
set(COMMON_NAME "Testplugin")
set(TITLE_NAME "testplugin")
set(PACKAGE_CONTACT "Jon Gough")
set(PACKAGE "testplugin")
set(SHORT_DESCRIPTION "Plugin to test examples of the ODAPI and JSON interface for ODRAW")
set(LONG_DESCRIPTION "testplugin Plugin is used to test out the ODraw API and demonstrate how to use it successfully from another plugin")

set(VERSION_MAJOR "1")
set(VERSION_MINOR "0")
set(VERSION_PATCH "254")
set(VERSION_TWEAK "13")
set(VERSION_DATE "08/09/2023") # DD/MM/YYYY format
set(OCPN_MIN_VERSION "ov58")
set(OCPN_API_VERSION_MAJOR "1")
set(OCPN_API_VERSION_MINOR "16")
set(TP_COMMENT "  * Release for O571 or o58 using CI")

set(PARENT "opencpn")

#Use to set up a local webserver with git installed.
#Default is github.com. Now set in pluginconfigure.cmake
#set(GIT_REPOSITORY_SERVER "github.com")

## Specifies Cloudsmith upload repository suffix for each catalog
set(PROD "prod")   #Standard Repos
set(BETA "beta")   #Standard Repos
set(ALPHA "alpha") #Standard Repos

# Set if  Cloudsmith Base Repository name does not match your Git Repository name.
#set(CLOUDSMITH_BASE_REPOSITORY "${GIT_REPOSITORY_NAME}")
#set(CLOUDSMITH_BASE_REPOSITORY "testplugin_pi")
set(CLOUDSMITH_BASE_REPOSITORY "testplugin")

# Set the following to use OpenCPN cloudsmith repositories instead of own repositories
set(CLOUDSMITH_USER "opencpn")
#set(CLOUDSMITH_USER "jon-gough")

set(XML_INFO_URL "https://opencpn.org/wiki/dokuwiki/doku.php?id=opencpn:opencpn_user_manual:plugins:safety:odraw1.6_pi")
set(XML_SUMMARY ${SHORT_DESCRIPTION})
set(XML_DESCRIPTION ${LONG_DESCRIPTION})

## ----- Modify section above to include all the details for your plugin ----- ##
## ----- Modify section below if there are special requirements for the plugin ----- ##

option(PLUGIN_USE_SVG "Use SVG graphics" ON)

set(CMAKE_CXX_STANDARD 11)

# Use local version of GLU library
# requires libs/glu directory
set(USE_LOCAL_GLU TRUE)
# Prefer libGL.so to libOpenGL.so, see CMP0072
set(OpenGL_GL_PREFERENCE "LEGACY")

option(USE_GL "Enable OpenGL support" ON)
message(STATUS "${CMLOC}USE_GL: ${USE_GL}")

if("${CMAKE_BUILD_TYPE}" STREQUAL "")
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build" FORCE)
endif("${CMAKE_BUILD_TYPE}" STREQUAL "")
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "" "Debug" "Release" "RelWithDebInfo" "MinSizeRel")
message(STATUS "${CMLOC}Build type: ${CMAKE_BUILD_TYPE}")

## ----- Statements below used to setup standard variables required by the CMAKE process -- Do not remove - ##

project(${PACKAGE})

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

#========================================================
include("PluginSetup")
#========================================================
## ----- Above used to setup standard variables required by CMAKE process - Do not remove ----- ##
## ----- Modify section below if there are special requirements for the plugin ----- ##

set(CMAKE_VERBOSE_MAKEFILE "Activate verbose mode for make files" ON)

option(Plugin_CXX11 "Use c++11" OFF)

## ----- Modify section above if there are special requirements for the plugin ----- ##
## ----- do not change next section - needed to configure build process ----- ##
#===========================================================
include("PluginConfigure")
#===========================================================
## ----- do not change section above - needed to configure build process ----- ##
## ----- Change below to match project requirements for source, headers, etc. ----- ##

add_definitions(-DUSE_S57)

## ---- Section Below - Add your project SET(SRCS and SET(HDRS  etc..

set(SRCS
    src/tpdc.cpp
    src/tpControlDialogDef.cpp
    src/tpControlDialogImpl.cpp
    src/tpicons.cpp
    src/tpJSON.cpp
    src/tpShaders.cpp
    src/testplugin_pi.cpp
    src/tpUtils.cpp)

set(HDRS
    include/testplugin_pi.h
    include/tpicons.h
    include/tpControlDialogDef.h
    include/tpControlDialogImpl.h
    include/tpIconCombo.h
    include/tpJSON.h
    include/tpdc.h
    include/tpShaders.h
    include/tpUtils.h)
	
set(OCPNSRC
    ocpnsrc/cutil.cpp
    ocpnsrc/TexFont.cpp
)

set(OCPNHDRS
    ocpninclude/cutil.h     #referenced could not find unless used include_directories
    ocpninclude/pluginmanager.h
    ocpninclude/shaders.h
#    ocpninclude/TexFont.h   #in libs/pluginDC/include
    ocpninclude/vector2D.h
#    libs/ocpn-api/ocpn_plugin.h
)

set(LIBSSRC 
    libs/wxJSON/src/jsonreader.cpp
    libs/wxJSON/src/jsonval.cpp
    libs/wxJSON/src/jsonwriter.cpp)

set(LIBHDRS
    libs/GL/gl.h
    libs/GL/gl_private.h
    libs/GL/glext.h
    libs/GL/glu.h
    libs/wxJSON/include/json_defs.h
    libs/wxJSON/include/jsonreader.h
    libs/wxJSON/include/jsonval.h
    libs/wxJSON/include/jsonwriter.h)

add_definitions(-DPLUGIN_USE_SVG)


## Below seems very convoluted, why is it necessary?
#set(EXTINCLUDE_DIR 
#    ${EXTINCLUDE_DIR} 
#     extinclude 
#     libs/ocpn-api/)

## ----- Section above - Add your project SET(SRCS and SET(HDRS  etc..
## ----- If using JSON validation in plugin section below is needed ----- ##

####
####     TRYING TO GET IT BUILDING WITHOUT JSON SCHEMA
####

#if(APPLE)
#    option(PI_JSON_SCHEMA_VALIDATOR "Use JSON Schema validator" OFF)
#else(APPLE)
#    option(PI_JSON_SCHEMA_VALIDATOR "Use JSON Schema validator" ON)
#endif(APPLE)

#if(PI_JSON_SCHEMA_VALIDATOR)
#    message(STATUS "${CMLOC}Using JSON Schema validation - warning this changes the 'flavour' of the executable and it may not be compatible with OpenCPN")
#    add_definitions(-DPI_JSON_SCHEMA_VALIDATOR)
#    message(STATUS "${CMLOC}Adding JSON Schema validation")
	
	## I tried to get this working and it seems to compile but there are warnings.
	## I think this also could be a lot simpler. Maybe there is a better way...

#    set(EXTSRC ${EXTSRC} 
#	      libs/json-schema-validator/json-schema-draft7.json.cpp 
#		  libs/json-schema-validator/json-uri.cpp 
#		  libs/json-schema-validator/json-validator.cpp
#		  )
#    set(EXTINCLUDE_DIR 
#	      ${EXTINCLUDE_DIR} 
#	      libs/json-schema-validator/nlohmann 
#	      libs/json-schema-validator)
	
    #include_directories(BEFORE ${PROJECT_SOURCE_DIR}/extinclude/json-schema-validator)
	
#else(PI_JSON_SCHEMA_VALIDATOR)
#    message(STATUS "${CMLOC}Not using JSON Schema validation - warning invalid JSON 'may' cause unexpected crashes")
#endif(PI_JSON_SCHEMA_VALIDATOR)

## ----- If using JSON validation in plugin section above is needed ----- ##


##================================================================================
## Change as required to collect all the set( headers and SRCS
## This add_library  can be used in conditionals below.
##================================================================================

SET(SRC ${SRCS} ${HDRS} ${OCPNHDRS} ${OCPNSRC} )  

##=============================================================
## Modify/configure 'Include' Directories below for the Plugin
##=============================================================

include_directories(BEFORE ${PROJECT_SOURCE_DIR}/include)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/ocpninclude) #needed to find cutil.h, fails otherwise
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/json-schema-validator) #tried to fix json-schema undefined.
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/json-schema-validator/nlohmann) #needed to find json-schema.hpp

#include_directories(BEFORE ${PROJECT_SOURCE_DIR}/ocpnsrc/glu/include)
#include_directories(BEFORE ${PROJECT_SOURCE_DIR}/${EXTINCLUDE_DIR})

include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/GL)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/glu)      #added with plugDC
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/ocpn-api)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/ODAPI)

include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/wxJSON)   #with json schema getting some wxjson error
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/wxJSON/include)

include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/pluginDC/include)



## Change target_sources to match your project needs.
##=================================================
target_sources(${PACKAGE_NAME} PUBLIC ${SRC} )
##=================================================

if(NOT OCPN_FLATPAK_CONFIG)
    # Build environment not available when flatpak is being configured so following statements will not work
    message(STATUS "${CMLOC}Adding target link libraries to ${PACKAGE_NAME}")

    add_subdirectory(libs/tinyxml)
    add_subdirectory(libs/pluginDC)

    target_link_libraries(${PACKAGE_NAME} ocpn::tinyxml)
    target_link_libraries(${PACKAGE_NAME} ocpn::pluginDC)
endif(NOT OCPN_FLATPAK_CONFIG)

add_definitions(-DTIXML_USE_STL)

##
## ----- If using JSON validation section below is needed ----- ##
##

#if(UNIX AND NOT APPLE AND NOT QT_ANDRIOD)
#    if(PI_JSON_SCHEMA_VALIDATOR)
#        # Change OS/ABI to be UNIX - System V to make it run with OCPN
#        set(lib_name lib${PACKAGE_NAME}.so)
#        message(STATUS "${CMLOC}lib-name: ${lib_name}")
#        find_file(elfedit elfedit)
#        if(NOT (${elfedit} STREQUAL "elfedit-NOTFOUND"))
#            message(STATUS "${CMLOC}Will ensure OS/ABI is set to 0 to match OCPN")
#            add_custom_command(
#                TARGET ${PACKAGE_NAME}
#                POST_BUILD
#                WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
#                DEPENDS ${PACKAGE_NAME}
#                COMMENT " Running post build action on ${lib_name}."
#                COMMAND sh -c 'elfedit --input-osabi=Linux --output-osabi=none ${lib_name} || (echo 0) ')
#        else(NOT (${elfedit} STREQUAL "elfedit-NOTFOUND"))
#            message(STATUS "${CMLOC}Cannot correct OS/ABI to match OCPN if it is generated incorrectly. Package binutils needed")
#        endif(NOT (${elfedit} STREQUAL "elfedit-NOTFOUND"))
#    endif(PI_JSON_SCHEMA_VALIDATOR)
#endif(UNIX AND NOT APPLE AND NOT QT_ANDRIOD)

#
# ----- If using JSON validation in plugin section above is needed ----- ##
#

##
## ----- Change above to match project requirements ----- ##
##


##------- Change below to match project requirements --------------
# Needed for android builds
if(QT_ANDROID)
    include_directories(BEFORE ${qt_android_include})
endif(QT_ANDROID)

## Needed for all builds Do not change - needed to build app --- ##
#========================================================
include("PluginInstall")
include("PluginLocalization")
include("PluginPackage")
#========================================================
## ----- Do not change section above - needed to configure build process ----- ##

message(STATUS "${CMLOC}include directories: ")
get_property(dirs DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY INCLUDE_DIRECTORIES)
foreach(dir ${dirs})
    message(STATUS "${CMLOC}dir='${dir}'")
endforeach()
