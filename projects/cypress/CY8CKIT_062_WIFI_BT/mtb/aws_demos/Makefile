################################################################################
# \file Makefile
# \version 1.0
#
# \brief
# Top-level application make file.
#
################################################################################
# \copyright
# Copyright 2018-2019 Cypress Semiconductor Corporation
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
################################################################################

# Choose between "aws_demos" and "aws_tests"
CY_AFR_BUILD=aws_demos

# Root location of AFR directory
CY_AFR_ROOT=../../../../..

# Build artifact location
CY_BUILD_RELATIVE_LOCATION=$(CY_AFR_ROOT)/build/cy
CY_BUILD_LOCATION=$(abspath $(CY_BUILD_RELATIVE_LOCATION))


################################################################################
# Basic Configuration
################################################################################

# Target board/hardware
TARGET=CY8CKIT-062-WIFI-BT

# Name of application (used to derive name of final linked file).
APPNAME=$(CY_AFR_BUILD)

# Name of toolchain to use. Options include:
#
# GCC_ARM -- GCC 7.2.1, provided with ModusToolbox IDE
# ARM     -- ARM Compiler (must be installed separately)
# IAR     -- IAR Compiler (must be installed separately)
#
# See also: CY_COMPILER_PATH below
TOOLCHAIN=GCC_ARM

# Default build configuration. Options include:
#
# Debug   -- build with minimal optimizations, focus on debugging.
# Release -- build with full optimizations
CONFIG=Debug

# If set to "true" or "1", display full command-lines when building.
VERBOSE=


################################################################################
# Advanced Configuration
################################################################################

# Enable optional code that is ordinarily disabled by default.
#
# Available components depend on the specific targeted hardware and firmware
# in use. In general, if you have
#
#    COMPONENTS=foo bar
#
# ... then code in directories named COMPONENT_foo and COMPONENT_bar will be 
# added to the build
#
COMPONENTS=FREERTOS 4343W CY8CKIT_062_WIFI_BT

# Like COMPONENTS, but disable optional code that was enabled by default.
DISABLE_COMPONENTS=

# By default the build system automatically looks in the Makefile's directory
# tree for source code and builds it. The SOURCES variable can be used to
# manually add source code to the build process from a location not searched
# by default, or otherwise not found by the build system.
SOURCES=

# Like SOURCES, but for include directories. Value should be paths to
# directories (without a leading -I).
INCLUDES=

# Add additional defines to the build process (without a leading -D).
DEFINES=CYBSP_WIFI_CAPABLE CY_RTOS_AWARE CY_USE_LWIP

# Select softfp or hardfp floating point. Default is softfp. 
VFP_SELECT=

# Additional / custom C compiler flags.
#
# NOTE: Includes and defines should use the INCLUDES and DEFINES variable
# above.
CFLAGS=

# Additional / custom C++ compiler flags.
#
# NOTE: Includes and defines should use the INCLUDES and DEFINES variable
# above.
CXXFLAGS=

# Additional / custom assembler flags.
#
# NOTE: Includes and defines should use the INCLUDES and DEFINES variable
# above.
ASFLAGS=

# Additional / custom linker flags.
ifeq ($(TOOLCHAIN),GCC_ARM)
LDFLAGS=-Wl,--undefined=uxTopUsedPriority
else
ifeq ($(TOOLCHAIN),IAR)
LDFLAGS=--keep uxTopUsedPriority
else
ifeq ($(TOOLCHAIN),ARM)
LDFLAGS=--undefined=uxTopUsedPriority
else
LDFLAGS=
endif
endif
endif

# Additional / custom libraries to link in to the application.
LDLIBS=

# Path to the linker script to use (if empty, use the default linker script).
LINKER_SCRIPT=

# Custom pre-build commands to run.
PREBUILD=

# Custom post-build commands to run.
POSTBUILD=


################################################################################
# Paths
################################################################################

# Relative path to the project directory (default is the Makefile's directory).
#
# This controls where automatic source code discovery looks for code.
CY_APP_PATH=

# Relative path to the "base" library. It provides the core makefile build 
# infrastructure.
CY_BASELIB_PATH=$(CY_AFR_ROOT)/vendors/cypress/psoc6/psoc6make

# Absolute path to the compiler's "bin" directory. 
#
# The default depends on the selected TOOLCHAIN (GCC_ARM uses the ModusToolbox
# IDE provided compiler by default).
CY_COMPILER_PATH=

# Include aFR configuration make file 
include ./afr.mk


################################################################################
# Tools path
################################################################################

# Locate ModusToolbox IDE helper tools folders in default installation
# locations for Windows, Linux, and macOS.
CY_WIN_HOME=$(subst \,/,$(USERPROFILE))
CY_TOOLS_PATHS ?= $(wildcard \
    $(CY_WIN_HOME)/ModusToolbox/tools_* \
    $(HOME)/ModusToolbox/tools_* \
    /Applications/ModusToolbox/tools_*)

# If you install ModusToolbox IDE in a custom location, add the path to its
# "tools_X.Y" folder (where X and Y are the version number of the tools
# folder).
CY_TOOLS_PATHS+=

# Default to the newest installed tools folder, or the users override (if it's
# found).
CY_TOOLS_DIR=$(lastword $(sort $(wildcard $(CY_TOOLS_PATHS))))

ifeq ($(CY_TOOLS_DIR),)
$(error Unable to find any of the available CY_TOOLS_PATHS -- $(CY_TOOLS_PATHS))
endif

$(info Tools Directory: $(CY_TOOLS_DIR))
include $(CY_TOOLS_DIR)/make/start.mk
.PHONY: generate_eclipse_project
generate_mtb_project:
	rm .project
	@echo -e "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<projectDescription>\n	<name>aws_demos</name>\n	<comment></comment>\n	<projects>\n	</projects>\n	<buildSpec>\n		<buildCommand>\n			<name>org.eclipse.cdt.managedbuilder.core.genmakebuilder</name>\n			<triggers>clean,full,incremental,</triggers>\n			<arguments>\n			</arguments>\n		</buildCommand>\n		<buildCommand>\n			<name>org.eclipse.cdt.managedbuilder.core.ScannerConfigBuilder</name>\n			<triggers>full,incremental,</triggers>\n			<arguments>\n			</arguments>\n		</buildCommand>\n	</buildSpec>\n	<natures>\n		<nature>org.eclipse.cdt.core.cnature</nature>\n		<nature>org.eclipse.cdt.core.ccnature</nature>\n		<nature>com.cypress.studio.app.cymodusnature</nature>\n		<nature>org.eclipse.cdt.managedbuilder.core.managedBuildNature</nature>\n		<nature>org.eclipse.cdt.managedbuilder.core.ScannerConfigNature</nature>\n	</natures>\n	<linkedResources>\n" >>.project
	@echo -e "$(foreach file,$(INCLUDES),\t\t<link>\n\t\t\t<name>$(patsubst ../../../../../%,%,$(file))</name>\n\t\t\t<type>2</type>\n\t\t\t<locationURI>$(patsubst ../../../../../%,PARENT-5-PROJECT_LOC/%,$(file))</locationURI>\n\t\t</link>\n)" >>.project
	@echo -e "$(foreach file,$(CY_SEARCH_AVAILABLE_H_INCLUDES),\t\t<link>\n\t\t\t<name>$(patsubst ../../../../../%,%,$(file))</name>\n\t\t\t<type>2</type>\n\t\t\t<locationURI>$(patsubst ../../../../../%,PARENT-5-PROJECT_LOC/%,$(file))</locationURI>\n\t\t</link>\n)" >>.project
	@echo -e "$(foreach file,$(CY_SEARCH_AVAILABLE_HPP_INCLUDES),\t\t<link>\n\t\t\t<name>$(patsubst ../../../../../%,%,$(file))</name>\n\t\t\t<type>2</type>\n\t\t\t<locationURI>$(patsubst ../../../../../%,PARENT-5-PROJECT_LOC/%,$(file))</locationURI>\n\t\t</link>\n)" >>.project
	@echo -e "$(foreach file,$(SOURCES),\t\t<link>\n\t\t\t<name>$(patsubst ../../../../../%,%,$(file))</name>\n\t\t\t<type>1</type>\n\t\t\t<locationURI>$(patsubst ../../../../../%,PARENT-5-PROJECT_LOC/%,$(file))</locationURI>\n\t\t</link>\n)" >>.project
	@echo -e "$(foreach file,$(CY_SEARCH_APP_SOURCE),\t\t<link>\n\t\t\t<name>$(patsubst ../../../../../%,%,$(file))</name>\n\t\t\t<type>1</type>\n\t\t\t<locationURI>$(patsubst ../../../../../%,PARENT-5-PROJECT_LOC/%,$(file))</locationURI>\n\t\t</link>\n)" >>.project
	@echo -e "\t</linkedResources>\n</projectDescription>" >>.project
