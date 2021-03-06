###############################################################################
### General CMake setup.
#
# TINYSPLINE_HAS_PARENT
#   Does this script have a parent?
###############################################################################
cmake_minimum_required(VERSION 3.4)
project(tinyspline)

# Turn on folders.
# https://cmake.org/cmake/help/v3.0/prop_gbl/USE_FOLDERS.html
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# Add additional CMake modules located in the source directory of this script.
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}")

# Does this script have a parent?
get_directory_property(TINYSPLINE_HAS_PARENT PARENT_DIRECTORY)



###############################################################################
### Project-related constants.
#
# TINYSPLINE_PACKAGE_NAME
#   The name of the package.
#
# TINYSPLINE_AUTHOR
#   The author of TinySpline.
#
# TINYSPLINE_MAIL
#   The contact email address.
#
# TINYSPLINE_URL
#   The URL of the repository managing TinySpline.
#
# TINYSPLINE_LICENSE
#   TinySpline's license.
#
# TINYSPLINE_VERSION
#   The current version of TinySpline.
#
# TINYSPLINE_SUMMARY
#   A short describtion.
#
# TINYSPLINE_DESCRIPTION
#   The long describtion.
###############################################################################
set(TINYSPLINE_PACKAGE_NAME "tinyspline" CACHE INTERNAL "")

set(TINYSPLINE_AUTHOR "Marcel Steinbeck" CACHE INTERNAL "")

set(TINYSPLINE_MAIL "tinyspline@retux.de" CACHE INTERNAL "")

set(TINYSPLINE_URL "https://github.com/msteinbeck/tinyspline" CACHE INTERNAL "")

set(TINYSPLINE_LICENSE "MIT" CACHE INTERNAL "")

set(TINYSPLINE_VERSION "0.3.0" CACHE INTERNAL "")

set(TINYSPLINE_SUMMARY "NURBS Library for a Multitude of Programming Languages" CACHE INTERNAL "")

set(TINYSPLINE_DESCRIPTION "TinySpline is a small, yet powerful library for interpolating, transforming, and querying arbitrary NURBS, B-Splines, and Bézier curves. The library is implemented in ANSI C (C89) and provides a wrapper for C++ along with auto-generated bindings for C#, D, Go, Java, Lua, Octave, PHP, Python, R, and Ruby." CACHE INTERNAL "")



###############################################################################
### Location-related constants.
#
# TINYSPLINE_BINARY_OUTPUT_DIRECTORY
#   Location of compiled binaries.
#
# TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY
#   Location of the compiled libraries.
#
# TINYSPLINE_***_SOURCE_DIRECTORY
#   The directory where the auto-generated source code of binding *** is
#   located.
#
# TINYSPLINE_***_INTERFACE_FILE
#   Location of the auto-generated interface file of binding *** (if any).
#
# TINYSPLINE_DLANG_INTERMEDIATE_FILE
#   Location of the auto-generated intermediate file of D.
###############################################################################
set(TINYSPLINE_BINARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin CACHE INTERNAL "")
set(TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib CACHE INTERNAL "")

# First for the generic no-config case (e.g. with MinGW)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${TINYSPLINE_BINARY_OUTPUT_DIRECTORY} CACHE INTERNAL "")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY} CACHE INTERNAL "")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY} CACHE INTERNAL "")
# Second, for multi-config builds (e.g. MSVC)
foreach(TINYSPLINE_CONFIG ${CMAKE_CONFIGURATION_TYPES})
		string(TOUPPER ${TINYSPLINE_CONFIG} TINYSPLINE_CONFIG)
		set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_${TINYSPLINE_CONFIG} ${TINYSPLINE_BINARY_OUTPUT_DIRECTORY} CACHE INTERNAL "")
		set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_${TINYSPLINE_CONFIG} ${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY} CACHE INTERNAL "")
		set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${TINYSPLINE_CONFIG} ${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY} CACHE INTERNAL "")
endforeach()

set(TINYSPLINE_CSHARP_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/csharp")
set(TINYSPLINE_DLANG_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/dlang")
set(TINYSPLINE_GO_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/go")
set(TINYSPLINE_JAVA_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/java")
set(TINYSPLINE_LUA_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/lua")
set(TINYSPLINE_NODEJS_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/nodejs")
set(TINYSPLINE_OCTAVE_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/octave")
set(TINYSPLINE_PHP_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/php")
set(TINYSPLINE_PYTHON_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/python")
set(TINYSPLINE_R_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/r")
set(TINYSPLINE_RUBY_SOURCE_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/ruby")

set(TINYSPLINE_CSHARP_INTERFACE_FILE "${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY}/TinySpline.dll" CACHE INTERNAL "")
set(TINYSPLINE_DLANG_INTERFACE_FILE "${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY}/tinyspline.d" CACHE INTERNAL "")
set(TINYSPLINE_GO_INTERFACE_FILE "${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY}/tinyspline.go" CACHE INTERNAL "")
set(TINYSPLINE_JAVA_INTERFACE_FILE "${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY}/tinyspline.jar" CACHE INTERNAL "")
set(TINYSPLINE_PHP_INTERFACE_FILE "${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY}/tinyspline.php" CACHE INTERNAL "")
set(TINYSPLINE_PYTHON_INTERFACE_FILE "${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY}/tinyspline.py" CACHE INTERNAL "")
set(TINYSPLINE_R_INTERFACE_FILE "${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY}/tinyspline.R" CACHE INTERNAL "")

set(TINYSPLINE_DLANG_INTERMEDIATE_FILE "${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY}/tinyspline_im.d" CACHE INTERNAL "")

# Add additional files and directories to delete with
# 'cmake --build . --target clean'.
list(APPEND TINYSPLINE_ADDITIONAL_MAKE_CLEAN_FILES
	${TINYSPLINE_CSHARP_SOURCE_DIRECTORY}
	${TINYSPLINE_CSHARP_INTERFACE_FILE}
	${TINYSPLINE_DLANG_SOURCE_DIRECTORY}
	${TINYSPLINE_DLANG_INTERFACE_FILE}
	${TINYSPLINE_DLANG_INTERMEDIATE_FILE}
	${TINYSPLINE_GO_SOURCE_DIRECTORY}
	${TINYSPLINE_JAVA_SOURCE_DIRECTORY}
	${TINYSPLINE_JAVA_INTERFACE_FILE}
	${TINYSPLINE_LUA_SOURCE_DIRECTORY}
	${TINYSPLINE_NODEJS_SOURCE_DIRECTORY}
	${TINYSPLINE_OCTAVE_SOURCE_DIRECTORY}
	${TINYSPLINE_PHP_SOURCE_DIRECTORY}
	${TINYSPLINE_PHP_INTERFACE_FILE}
	${TINYSPLINE_PYTHON_SOURCE_DIRECTORY}
	${TINYSPLINE_PYTHON_INTERFACE_FILE}
	${TINYSPLINE_R_SOURCE_DIRECTORY}
	${TINYSPLINE_R_INTERFACE_FILE}
	${TINYSPLINE_RUBY_SOURCE_DIRECTORY}
)
set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES
	"${TINYSPLINE_ADDITIONAL_MAKE_CLEAN_FILES}")



###############################################################################
### CMake-related/Platform-related constants.
#
# TINYSPLINE_***_CMAKE_TARGET
#   The CMake buid target for binding ***.
#
# TINYSPLINE_PLATFORM_NAME
#   Name of the platform in lowercase. Supported values are: 'linux', 'macosx',
#   'windows', and 'generic'.
#
# TINYSPLINE_PLATFORM_ARCH
#   Architecture of target system, e. g. 'x86_64'.
#
# TINYSPLINE_PLATFORM
#   Platform specific identifier, e. g. 'linux-x86_64'.
###############################################################################
set(TINYSPLINE_CSHARP_CMAKE_TARGET "tinysplinecsharp" CACHE INTERNAL "")
set(TINYSPLINE_DLANG_CMAKE_TARGET "tinysplinedlang" CACHE INTERNAL "")
set(TINYSPLINE_GO_CMAKE_TARGET "tinysplinego" CACHE INTERNAL "")
set(TINYSPLINE_JAVA_CMAKE_TARGET "tinysplinejava" CACHE INTERNAL "")
set(TINYSPLINE_LUA_CMAKE_TARGET "tinysplinelua" CACHE INTERNAL "")
set(TINYSPLINE_NODEJS_CMAKE_TARGET "tinysplinenodejs" CACHE INTERNAL "")
set(TINYSPLINE_OCTAVE_CMAKE_TARGET "tinysplineoctave" CACHE INTERNAL "")
set(TINYSPLINE_PHP_CMAKE_TARGET "tinysplinephp" CACHE INTERNAL "")
set(TINYSPLINE_PYTHON_CMAKE_TARGET "tinysplinepython" CACHE INTERNAL "")
set(TINYSPLINE_R_CMAKE_TARGET "tinyspliner" CACHE INTERNAL "")
set(TINYSPLINE_RUBY_CMAKE_TARGET "tinysplineruby" CACHE INTERNAL "")

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
	set(TINYSPLINE_PLATFORM_NAME "linux" CACHE INTERNAL "")
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "Darwin")
	set(TINYSPLINE_PLATFORM_NAME "macosx" CACHE INTERNAL "")
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "Windows")
	set(TINYSPLINE_PLATFORM_NAME "windows" CACHE INTERNAL "")
else()
	set(TINYSPLINE_PLATFORM_NAME "generic" CACHE INTERNAL "")
endif()

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
	set(TINYSPLINE_PLATFORM_ARCH "x86_64" CACHE INTERNAL "")
else()
	set(TINYSPLINE_PLATFORM_ARCH "x86" CACHE INTERNAL "")
endif()

set(TINYSPLINE_PLATFORM
	"${TINYSPLINE_PLATFORM_NAME}-${TINYSPLINE_PLATFORM_ARCH}"
	CACHE INTERNAL "")



###############################################################################
### Add different options to this script.
#
# TINYSPLINE_FLOAT_PRECISION - default: OFF
#   Use single (float) instead of double precision.
#
# TINYSPLINE_PYTHON_VERSION - default: ANY
#   Force Python version.
#
# TINYSPLINE_ENABLE_*** - default: TRUE for CXX, FALSE otherwise
#   These options allow to enable certain interfaces.
#
# TINYSPLINE_ENABLE_ALL_INTERFACES
#   If set, all interfaces are enabled automatically.
###############################################################################
# TINYSPLINE_FLOAT_PRECISION
option(TINYSPLINE_FLOAT_PRECISION "Build TinySpline with single (float) precision." OFF)

# TINYSPLINE_PYTHON_VERSION
set(TINYSPLINE_PYTHON_VERSION "ANY" CACHE STRING "Force Python version. Supported values are: '2', '3', and 'ANY' (fallback for unknown values).")

# TINYSPLINE_ENABLE_***
option(TINYSPLINE_ENABLE_CXX "Enable the C++ interface." TRUE)
option(TINYSPLINE_ENABLE_CSHARP "Enable the C# interface." FALSE)
option(TINYSPLINE_ENABLE_DLANG "Enable the D interface." FALSE)
option(TINYSPLINE_ENABLE_GO "Enable the Go interface." FALSE)
option(TINYSPLINE_ENABLE_JAVA "Enable the Java interface." FALSE)
option(TINYSPLINE_ENABLE_LUA "Enable the Lua interface." FALSE)
option(TINYSPLINE_ENABLE_NODEJS "Enable the Javascript (Node.js) interface." FALSE)
option(TINYSPLINE_ENABLE_OCTAVE "Enable the Octave interface." FALSE)
option(TINYSPLINE_ENABLE_PHP "Enable the PHP interface." FALSE)
option(TINYSPLINE_ENABLE_PYTHON "Enable the Python interface." FALSE)
option(TINYSPLINE_ENABLE_R "Enable the R interface." FALSE)
option(TINYSPLINE_ENABLE_RUBY "Enable the Ruby interface." FALSE)

option(TINYSPLINE_ENABLE_ALL_INTERFACES "Enable all interfaces." FALSE)
if (TINYSPLINE_ENABLE_ALL_INTERFACES)
	set(TINYSPLINE_ENABLE_CXX TRUE)
	set(TINYSPLINE_ENABLE_CSHARP TRUE)
	set(TINYSPLINE_ENABLE_DLANG TRUE)
	set(TINYSPLINE_ENABLE_GO TRUE)
	set(TINYSPLINE_ENABLE_JAVA TRUE)
	set(TINYSPLINE_ENABLE_LUA TRUE)
	set(TINYSPLINE_ENABLE_NODEJS TRUE)
	set(TINYSPLINE_ENABLE_OCTAVE TRUE)
	set(TINYSPLINE_ENABLE_PHP TRUE)
	set(TINYSPLINE_ENABLE_PYTHON TRUE)
	set(TINYSPLINE_ENABLE_R TRUE)
	set(TINYSPLINE_ENABLE_RUBY TRUE)
endif()



###############################################################################
### Map supported environment variables to CMake variables. The following
### variables will be processed and may overwrite previously defined options
### (only if the option's default value has been selected).
#
# TINYSPLINE_FLOAT_PRECISION
#   See corresponding option above.
#
# TINYSPLINE_PYTHON_VERSION
#   See corresponding option above.
#
# CMAKE_TOOLCHAIN_FILE
#   The CMake tool chain file for cross-compiling.
#
# SWIG_EXECUTABLE
#   The SWIG executable.
###############################################################################
# TINYSPLINE_FLOAT_PRECISION
if(NOT TINYSPLINE_FLOAT_PRECISION AND DEFINED ENV{TINYSPLINE_FLOAT_PRECISION})
	message(STATUS "Using environment variable 'TINYSPLINE_FLOAT_PRECISION'")
	set(TINYSPLINE_FLOAT_PRECISION $ENV{TINYSPLINE_FLOAT_PRECISION})
endif()

# TINYSPLINE_PYTHON_VERSION
if(${TINYSPLINE_PYTHON_VERSION} STREQUAL "ANY" AND DEFINED ENV{TINYSPLINE_PYTHON_VERSION})
	message(STATUS "Using environment variable 'TINYSPLINE_PYTHON_VERSION'")
	set(TINYSPLINE_PYTHON_VERSION $ENV{TINYSPLINE_PYTHON_VERSION})
endif()

# CMAKE_TOOLCHAIN_FILE
if(DEFINED ENV{CMAKE_TOOLCHAIN_FILE})
	message(STATUS "Using environment variable 'CMAKE_TOOLCHAIN_FILE'")
	set(CMAKE_TOOLCHAIN_FILE $ENV{CMAKE_TOOLCHAIN_FILE})
endif()

# SWIG_EXECUTABLE
if(DEFINED ENV{SWIG_EXECUTABLE})
	message(STATUS "Using environment variable 'SWIG_EXECUTABLE'")
	set(SWIG_EXECUTABLE $ENV{SWIG_EXECUTABLE})
endif()



###############################################################################
### Setup compiler suite.
#
# TINYSPLINE_DEBUG_POSTFIX
#   Postfix of debug builds.
#
# TINYSPLINE_DEFINITIONS (set in parent scope)
#   Preprocessor definitions that should be added to parent file with
#   'add_definitions("${TINYSPLINE_DEFINITIONS}")'.
#
# TINYSPLINE_CXX11_COMPLIANT
#   Does the C++ compiler support C++11? (Required by some bindings)
#
# TINYSPLINE_LIBRARIES (set in parent scope)
#   Additional libraries to link against.
#
# TINYSPLINE_LIBRARY_C_FLAGS
#   C library specific flags.
#
# TINYSPLINE_LIBRARY_CXX_FLAGS
#   C++ library specific flags.
#
# TINYSPLINE_BINDING_CXX_FLAGS
#   C++ binding specific flags.
###############################################################################
# TINYSPLINE_DEBUG_POSTFIX
# TINYSPLINE_DEFINITIONS
if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
	# It is common practice for MSVC builds to add a debug postfix.
	set(TINYSPLINE_DEBUG_POSTFIX "d")
	if(TINYSPLINE_FLOAT_PRECISION)
		set(TINYSPLINE_DEFINITIONS "${TINYSPLINE_DEFINITIONS} /DTINYSPLINE_FLOAT_PRECISION")
	endif()
else()
	set(TINYSPLINE_DEBUG_POSTFIX "")
	if(TINYSPLINE_FLOAT_PRECISION)
		set(TINYSPLINE_DEFINITIONS "${TINYSPLINE_DEFINITIONS} -DTINYSPLINE_FLOAT_PRECISION")
	endif()
endif()
# Remove leading and trailing spaces from variables.
string(STRIP "${TINYSPLINE_DEBUG_POSTFIX}" TINYSPLINE_DEBUG_POSTFIX)
string(STRIP "${TINYSPLINE_DEFINITIONS}" TINYSPLINE_DEFINITIONS)
# Export relevant variables.
if(TINYSPLINE_HAS_PARENT)
	set(TINYSPLINE_DEFINITIONS "${TINYSPLINE_DEFINITIONS}" PARENT_SCOPE)
else()
	set(TINYSPLINE_DEFINITIONS "${TINYSPLINE_DEFINITIONS}")
endif()
add_definitions("${TINYSPLINE_DEFINITIONS}")

# TINYSPLINE_CXX11_COMPLIANT
# TINYSPLINE_LIBRARIES
# TINYSPLINE_LIBRARY_C_FLAGS
# TINYSPLINE_LIBRARY_CXX_FLAGS
# TINYSPLINE_BINDING_CXX_FLAGS
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
	# TINYSPLINE_CXX11_COMPLIANT
	# http://stackoverflow.com/questions/10984442/how-to-detect-c11-support-of-a-compiler-with-cmake
	if(CMAKE_CXX_COMPILER_VERSION VERSION_GREATER 4.7 OR
			CMAKE_CXX_COMPILER_VERSION VERSION_EQUAL 4.7)
		set(TINYSPLINE_CXX11_COMPLIANT TRUE)
	else()
		set(TINYSPLINE_CXX11_COMPLIANT FALSE)
	endif()

	# CMAKE_C_FLAGS
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c89")

	# TINYSPLINE_LIBRARIES
	set(TINYSPLINE_LIBRARIES "${TINYSPLINE_LIBRARIES} -lm")

	# TINYSPLINE_LIBRARY_C_FLAGS
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -Werror")
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -Wall")
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -Wextra")
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -Wfloat-equal")
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -pedantic")

	# TINYSPLINE_LIBRARY_CXX_FLAGS
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} -Werror")
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} -Wall")
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} -Wextra")
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} -Wfloat-equal")

	# TINYSPLINE_BINDING_CXX_FLAGS
	set(TINYSPLINE_BINDING_CXX_FLAGS "${TINYSPLINE_BINDING_CXX_FLAGS} -w")
	set(TINYSPLINE_BINDING_CXX_FLAGS "${TINYSPLINE_BINDING_CXX_FLAGS} -DSWIG")
elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
	# TINYSPLINE_CXX11_COMPLIANT
	# http://stackoverflow.com/questions/10408849/how-can-i-use-c-11-features-in-clang
	if(CMAKE_CXX_COMPILER_VERSION VERSION_GREATER 3.3 OR
			CMAKE_CXX_COMPILER_VERSION VERSION_EQUAL 3.3)
		set(TINYSPLINE_CXX11_COMPLIANT TRUE)
	else()
		set(TINYSPLINE_CXX11_COMPLIANT FALSE)
	endif()

	# CMAKE_C_FLAGS
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c89")

	# TINYSPLINE_LIBRARIES
	set(TINYSPLINE_LIBRARIES "${TINYSPLINE_LIBRARIES} -lm")

	# TINYSPLINE_LIBRARY_C_FLAGS
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -Werror")
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -Wall")
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -Wextra")
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -Wfloat-equal")
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} -pedantic")

	# TINYSPLINE_LIBRARY_CXX_FLAGS
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} -Werror")
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} -Wall")
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} -Wextra")
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} -Wfloat-equal")

	# TINYSPLINE_BINDING_CXX_FLAGS
	set(TINYSPLINE_BINDING_CXX_FLAGS "${TINYSPLINE_BINDING_CXX_FLAGS} -w")
	set(TINYSPLINE_BINDING_CXX_FLAGS "${TINYSPLINE_BINDING_CXX_FLAGS} -DSWIG")
elseif(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
	# TINYSPLINE_CXX11_COMPLIANT
	if(CMAKE_CXX_COMPILER_VERSION VERSION_GREATER 18 OR
			CMAKE_CXX_COMPILER_VERSION VERSION_EQUAL 18)
		set(TINYSPLINE_CXX11_COMPLIANT TRUE)
	else()
		set(TINYSPLINE_CXX11_COMPLIANT FALSE)
	endif()

	# TINYSPLINE_LIBRARY_C_FLAGS
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} /W4")
	set(TINYSPLINE_LIBRARY_C_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS} /WX")

	# TINYSPLINE_LIBRARY_CXX_FLAGS
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} /W4")
	set(TINYSPLINE_LIBRARY_CXX_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS} /WX")

	# TINYSPLINE_BINDING_CXX_FLAGS
	set(TINYSPLINE_BINDING_CXX_FLAGS "${TINYSPLINE_BINDING_CXX_FLAGS} /w")
	set(TINYSPLINE_BINDING_CXX_FLAGS "${TINYSPLINE_BINDING_CXX_FLAGS} /DSWIG")

	# Used to automatically create a .def file with all the global symbols for
	# a shared library.
	set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
endif()
# Remove leading and trailing spaces from variables.
string(STRIP "${CMAKE_C_FLAGS}" CMAKE_C_FLAGS)
string(STRIP "${CMAKE_CXX_FLAGS}" CMAKE_CXX_FLAGS)
string(STRIP "${TINYSPLINE_LIBRARIES}" TINYSPLINE_LIBRARIES)
string(STRIP "${TINYSPLINE_LIBRARY_C_FLAGS}" TINYSPLINE_LIBRARY_C_FLAGS)
string(STRIP "${TINYSPLINE_LIBRARY_CXX_FLAGS}" TINYSPLINE_LIBRARY_CXX_FLAGS)
string(STRIP "${TINYSPLINE_BINDING_CXX_FLAGS}" TINYSPLINE_BINDING_CXX_FLAGS)
# Export relevant variables.
if(TINYSPLINE_HAS_PARENT)
	set(TINYSPLINE_LIBRARIES "${TINYSPLINE_LIBRARIES}" PARENT_SCOPE)
else()
	set(TINYSPLINE_LIBRARIES "${TINYSPLINE_LIBRARIES}")
endif()



###############################################################################
### Create the C/C++ library. We explicitly set the prefix of the static
### library to 'lib' to prevent naming collisions when compiling with MSVC.
#
# TINYSPLINE_INCLUDE_DIRS (set in parent scope)
#   Include directories of the C/C++ library. The listed directories contain
#   all header files as well as all source files (.c/.cpp files).
#
# TINYSPLINE_C_SOURCE_FILES (set in parent scope)
#   List of source files (absolute paths) that are required to build the C
#   library. Does not contain header files.
#
# TINYSPLINE_CXX_SOURCE_FILES (set in parent scope)
#   List of source files (absolute paths) that are required to build the C++
#   library. Does not contain header files, but all source files listed in
#   TINYSPLINE_C_SOURCE_FILES.
###############################################################################
# TINYSPLINE_INCLUDE_DIRS
if(TINYSPLINE_HAS_PARENT)
	set(TINYSPLINE_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}" PARENT_SCOPE)
else()
	set(TINYSPLINE_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}")
endif()

# TINYSPLINE_C_SOURCE_FILES
list(APPEND TINYSPLINE_C_SOURCE_FILES
	"${CMAKE_CURRENT_SOURCE_DIR}/tinyspline.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/parson.c"
)
if(TINYSPLINE_HAS_PARENT)
	set(TINYSPLINE_C_SOURCE_FILES ${TINYSPLINE_C_SOURCE_FILES} PARENT_SCOPE)
else()
	set(TINYSPLINE_C_SOURCE_FILES ${TINYSPLINE_C_SOURCE_FILES})
endif()

# TINYSPLINE_CXX_SOURCE_FILES
list(APPEND TINYSPLINE_CXX_SOURCE_FILES
	${TINYSPLINE_C_SOURCE_FILES}
	"${CMAKE_CURRENT_SOURCE_DIR}/tinysplinecpp.cpp"
)
if(TINYSPLINE_HAS_PARENT)
	set(TINYSPLINE_CXX_SOURCE_FILES ${TINYSPLINE_CXX_SOURCE_FILES} PARENT_SCOPE)
else()
	set(TINYSPLINE_CXX_SOURCE_FILES ${TINYSPLINE_CXX_SOURCE_FILES})
endif()

# C library
add_library(tinyspline_shared SHARED ${TINYSPLINE_C_SOURCE_FILES})
set_target_properties(tinyspline_shared PROPERTIES
	OUTPUT_NAME "tinyspline"
	DEBUG_POSTFIX "${TINYSPLINE_DEBUG_POSTFIX}"
	COMPILE_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS}"
)
install(TARGETS tinyspline_shared
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
	RUNTIME DESTINATION bin
)
add_library(tinyspline_static STATIC ${TINYSPLINE_C_SOURCE_FILES})
set_target_properties(tinyspline_static PROPERTIES
	OUTPUT_NAME "tinyspline"
	DEBUG_POSTFIX "${TINYSPLINE_DEBUG_POSTFIX}"
	PREFIX "lib"
	COMPILE_FLAGS "${TINYSPLINE_LIBRARY_C_FLAGS}"
)
install(TARGETS tinyspline_static
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
)
install(FILES tinyspline.h
	DESTINATION include
)
target_include_directories(tinyspline_static
	PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
)

# C++ library
if(TINYSPLINE_ENABLE_CXX)
	add_library(tinysplinecpp_shared SHARED ${TINYSPLINE_CXX_SOURCE_FILES})
	set_target_properties(tinysplinecpp_shared PROPERTIES
		OUTPUT_NAME "tinysplinecpp"
		DEBUG_POSTFIX "${TINYSPLINE_DEBUG_POSTFIX}"
		COMPILE_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS}"
	)
	install(TARGETS tinysplinecpp_shared
		LIBRARY DESTINATION lib
		ARCHIVE DESTINATION lib
		RUNTIME DESTINATION bin
	)
	add_library(tinysplinecpp_static STATIC ${TINYSPLINE_CXX_SOURCE_FILES})
	set_target_properties(tinysplinecpp_static PROPERTIES
		OUTPUT_NAME "tinysplinecpp"
		DEBUG_POSTFIX "${TINYSPLINE_DEBUG_POSTFIX}"
		PREFIX "lib"
		COMPILE_FLAGS "${TINYSPLINE_LIBRARY_CXX_FLAGS}"
	)
	install(TARGETS tinysplinecpp_static
		LIBRARY DESTINATION lib
		ARCHIVE DESTINATION lib
	)
	install(FILES tinysplinecpp.h
		DESTINATION include
	)
	target_include_directories(tinysplinecpp_static
		PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
	)
endif()



###############################################################################
### Setup SWIG and create bindings.
#
# TINYSPLINE_BINDINGS_FOLDER_NAME
#   Name of the IDE project folder containing the bindings.
#   ( set_property(GLOBAL PROPERTY USE_FOLDERS ON) )
#
# TINYSPLINE_SWIG_DEFINES
#   List of defines passed to Swig.
#
# TINYSPLINE_BINDING_REQUESTED
#   At least one binding should be created.
###############################################################################
# TINYSPLINE_BINDINGS_FOLDER_NAME
set(TINYSPLINE_BINDINGS_FOLDER_NAME "bindings")

# TINYSPLINE_SWIG_DEFINES
if(TINYSPLINE_FLOAT_PRECISION)
	list(APPEND TINYSPLINE_SWIG_DEFINES -DTINYSPLINE_FLOAT_PRECISION)
endif()

# TINYSPLINE_BINDING_REQUESTED
if(${TINYSPLINE_ENABLE_CSHARP}
		OR ${TINYSPLINE_ENABLE_DLANG}
		OR ${TINYSPLINE_ENABLE_GO}
		OR ${TINYSPLINE_ENABLE_JAVA}
		OR ${TINYSPLINE_ENABLE_LUA}
		OR ${TINYSPLINE_ENABLE_NODEJS}
		OR ${TINYSPLINE_ENABLE_OCTAVE}
		OR ${TINYSPLINE_ENABLE_PHP}
		OR ${TINYSPLINE_ENABLE_PYTHON}
		OR ${TINYSPLINE_ENABLE_R}
		OR ${TINYSPLINE_ENABLE_RUBY})
	set(TINYSPLINE_BINDING_REQUESTED TRUE)
else()
	set(TINYSPLINE_BINDING_REQUESTED FALSE)
endif()

# Utility function for creating bindings.
function (tinyspline_add_swig_library)
	cmake_parse_arguments(ARGS
		"CXX11"
		"TARGET;LANG;OUTPUT;LIBS;NAME"
		"SWIG_ARGS;FLAGS"
		${ARGN}
	)
	set_source_files_properties("${ARGS_TARGET}.i" PROPERTIES CPLUSPLUS ON)
	set(CMAKE_SWIG_FLAGS ${ARGS_SWIG_ARGS} -O ${TINYSPLINE_SWIG_DEFINES})
	set(CMAKE_SWIG_OUTDIR ${ARGS_OUTPUT})
	if (${CMAKE_VERSION} VERSION_LESS "3.8.0")
		swig_add_module(${ARGS_TARGET} ${ARGS_LANG} "${ARGS_TARGET}.i"
			${TINYSPLINE_CXX_SOURCE_FILES})
	else()
		swig_add_library(${ARGS_TARGET} LANGUAGE ${ARGS_LANG}
			SOURCES "${ARGS_TARGET}.i" ${TINYSPLINE_CXX_SOURCE_FILES})
	endif()
	swig_link_libraries(${ARGS_TARGET} ${ARGS_LIBS})
	# For some reasons Python's make target starts with '_'.
	set(ACTUAL_TARGET ${ARGS_TARGET})
	if(${ARGS_LANG} STREQUAL python)
		set(ACTUAL_TARGET _${ACTUAL_TARGET})
	endif()
	set(TINYSPLINE_SWIG_CMAKE_TARGET ${ACTUAL_TARGET} PARENT_SCOPE)
	set_target_properties(${ACTUAL_TARGET} PROPERTIES
		FOLDER ${TINYSPLINE_BINDINGS_FOLDER_NAME}
		COMPILE_FLAGS "${TINYSPLINE_BINDING_CXX_FLAGS} ${ARGS_FLAGS}"
    )
	if(${ARGS_CXX11})
		set_property(TARGET ${ACTUAL_TARGET}
			APPEND PROPERTY CXX_STANDARD 11
		)
		set_property(TARGET ${ACTUAL_TARGET}
			APPEND PROPERTY CXX_STANDARD_REQUIRED ON
		)
		set_property(TARGET ${ACTUAL_TARGET}
			APPEND PROPERTY CXX_EXTENSIONS OFF
		)
	endif()
	if (NOT ${ARGS_NAME} STREQUAL "")
        set_property(TARGET ${ACTUAL_TARGET}
			APPEND PROPERTY OUTPUT_NAME "${ARGS_NAME}"
        )
    endif()
endfunction()

if(${TINYSPLINE_BINDING_REQUESTED})
	find_package(SWIG 3.0.1 REQUIRED)
	include(${SWIG_USE_FILE})
	include_directories(${CMAKE_CURRENT_SOURCE_DIR})

	# C#
	if(${TINYSPLINE_ENABLE_CSHARP})
		tinyspline_add_swig_library(
			TARGET ${TINYSPLINE_CSHARP_CMAKE_TARGET}
			LANG csharp
			OUTPUT ${TINYSPLINE_CSHARP_SOURCE_DIRECTORY}
			SWIG_ARGS -namespace TinySpline
		)
		# DLL
		find_program(TINYSPLINE_CSHARP_COMPILER
			NAMES csc mcs
			PATHS "C:/Windows/Microsoft.NET/Framework/v4.5"
		)
		if(TINYSPLINE_CSHARP_COMPILER)
			if(${TINYSPLINE_CSHARP_COMPILER} MATCHES "csc$|csc.exe$")
				# csc neither supports the 'sdk' flag nor '/' in 'recursive'
				add_custom_command(
					TARGET ${TINYSPLINE_SWIG_CMAKE_TARGET}
					POST_BUILD
					COMMAND
						${TINYSPLINE_CSHARP_COMPILER}
						/target:library
						/out:"${TINYSPLINE_CSHARP_INTERFACE_FILE}"
						/recurse:"${TINYSPLINE_CSHARP_SOURCE_DIRECTORY}\\*.cs"
				)
			else()
				add_custom_command(
					TARGET ${TINYSPLINE_SWIG_CMAKE_TARGET}
					POST_BUILD
					COMMAND
						${TINYSPLINE_CSHARP_COMPILER}
						-sdk:4.5
						-target:library
						-out:"${TINYSPLINE_CSHARP_INTERFACE_FILE}"
						-recurse:"${TINYSPLINE_CSHARP_SOURCE_DIRECTORY}/*.cs"
				)
			endif()
		endif()
	endif()

	# D
	if(${TINYSPLINE_ENABLE_DLANG})
		tinyspline_add_swig_library(
			TARGET ${TINYSPLINE_DLANG_CMAKE_TARGET}
			LANG d
			OUTPUT ${TINYSPLINE_DLANG_SOURCE_DIRECTORY}
			SWIG_ARGS -d2 -wrapperlibrary ${TINYSPLINE_DLANG_CMAKE_TARGET}
				-module ${TINYSPLINE_PACKAGE_NAME}
		)
		add_custom_command(
			TARGET ${TINYSPLINE_SWIG_CMAKE_TARGET}
			POST_BUILD
			COMMAND
				${CMAKE_COMMAND}
				-E copy
				"${TINYSPLINE_DLANG_SOURCE_DIRECTORY}/${TINYSPLINE_PACKAGE_NAME}.d"
				"${TINYSPLINE_DLANG_INTERFACE_FILE}"
			COMMAND
				${CMAKE_COMMAND}
				-E copy
				"${TINYSPLINE_DLANG_SOURCE_DIRECTORY}/${TINYSPLINE_PACKAGE_NAME}_im.d"
				"${TINYSPLINE_DLANG_INTERMEDIATE_FILE}"
		)
	endif()

	# Go
	if(${TINYSPLINE_ENABLE_GO})
		math(EXPR TINYSPLINE_GO_INTGOSIZE "${CMAKE_SIZEOF_VOID_P} * 8")
		tinyspline_add_swig_library(
			TARGET ${TINYSPLINE_GO_CMAKE_TARGET}
			LANG go
			OUTPUT ${TINYSPLINE_GO_SOURCE_DIRECTORY}
			SWIG_ARGS -cgo -intgosize ${TINYSPLINE_GO_INTGOSIZE} -package tinyspline
		)
		add_custom_command(
			TARGET ${TINYSPLINE_SWIG_CMAKE_TARGET}
			POST_BUILD
			COMMAND
			${CMAKE_COMMAND} -E copy
			"${TINYSPLINE_GO_SOURCE_DIRECTORY}/${TINYSPLINE_GO_CMAKE_TARGET}.go"
			"${TINYSPLINE_GO_INTERFACE_FILE}"
		)
	endif()

	# Java
	if(${TINYSPLINE_ENABLE_JAVA})
		find_package(JNI REQUIRED)
		include_directories(${JNI_INCLUDE_DIRS})
		tinyspline_add_swig_library(
			TARGET ${TINYSPLINE_JAVA_CMAKE_TARGET}
			LANG java
			OUTPUT "${TINYSPLINE_JAVA_SOURCE_DIRECTORY}/org/tinyspline"
			LIBS ${JNI_LIBRARIES}
			SWIG_ARGS -package "org.tinyspline"
		)
		# Jar archive
		find_package(Java COMPONENTS Development)
		if(${Java_Development_FOUND})
			include(UseJava)
			add_custom_command(
				TARGET ${TINYSPLINE_SWIG_CMAKE_TARGET}
				POST_BUILD
				COMMAND
					${CMAKE_COMMAND}
					-E make_directory
					"${TINYSPLINE_JAVA_SOURCE_DIRECTORY}/classes"
				COMMAND
					${CMAKE_COMMAND}
					-E copy
					"${TINYSPLINE_LIBRARY_OUTPUT_DIRECTORY}/$<TARGET_FILE_NAME:${TINYSPLINE_JAVA_CMAKE_TARGET}>"
					"${TINYSPLINE_JAVA_SOURCE_DIRECTORY}/classes/${TINYSPLINE_PLATFORM}"
				COMMAND
					"${Java_JAVAC_EXECUTABLE}"
					-d "${TINYSPLINE_JAVA_SOURCE_DIRECTORY}/classes"
					"${TINYSPLINE_JAVA_SOURCE_DIRECTORY}/org/tinyspline/*.java"
				COMMAND
					"${Java_JAR_EXECUTABLE}"
					-cfM "${TINYSPLINE_JAVA_INTERFACE_FILE}"
					-C "${TINYSPLINE_JAVA_SOURCE_DIRECTORY}/classes" .
			)
		endif()
	endif()

	# Lua
	if(${TINYSPLINE_ENABLE_LUA})
		find_package(Lua REQUIRED)
		include_directories(${LUA_INCLUDE_DIR})
		tinyspline_add_swig_library(
			TARGET ${TINYSPLINE_LUA_CMAKE_TARGET}
			LANG lua
			OUTPUT ${TINYSPLINE_LUA_SOURCE_DIRECTORY}
			LIBS ${LUA_LIBRARIES}
		)
	endif()

	# Node.js
	if(${TINYSPLINE_ENABLE_NODEJS})
		find_path(TINYSPLINE_NODEJS_NODE_H_PATH
				node.h
				HINTS "/usr/include/node")
		find_path(TINYSPLINE_NODEJS_V8_H_PATH
				v8.h
				HINTS "/usr/include/node")
		if(NOT TINYSPLINE_NODEJS_NODE_H_PATH)
			message(FATAL_ERROR "Missing node.h")
		endif()
		if(NOT TINYSPLINE_NODEJS_V8_H_PATH)
			message(FATAL_ERROR "Missing v8.h")
		endif()
		include_directories(${TINYSPLINE_NODEJS_NODE_H_PATH}
				${TINYSPLINE_NODEJS_V8_H_PATH})
		tinyspline_add_swig_library(
			TARGET ${TINYSPLINE_NODEJS_CMAKE_TARGET}
			LANG javascript
			OUTPUT ${TINYSPLINE_NODEJS_SOURCE_DIRECTORY}
			LIBS ${LUA_LIBRARIES}
			SWIG_ARGS -node
			CXX11
		)
	endif()

	# Octave
	if(${TINYSPLINE_ENABLE_OCTAVE})
		find_package(Octave REQUIRED)
		if((${OCTAVE_VERSION_STRING} VERSION_GREATER 4.2
					OR ${OCTAVE_VERSION_STRING} VERSION_EQUAL 4.2)
				AND ${SWIG_VERSION} VERSION_LESS 3.0.12)
			message(FATAL_ERROR "Octave 4.2 requires Swig 3.0.12 or later")
		elseif(NOT ${TINYSPLINE_CXX11_COMPLIANT})
			message(FATAL_ERROR "Octave requires C++11")
		else()
			include_directories(${OCTAVE_INCLUDE_DIRS})
			tinyspline_add_swig_library(
				TARGET ${TINYSPLINE_OCTAVE_CMAKE_TARGET}
				LANG octave
				OUTPUT ${TINYSPLINE_OCTAVE_SOURCE_DIRECTORY}
				LIBS ${OCTAVE_LIBRARIES}
                NAME ${TINYSPLINE_PACKAGE_NAME}
				SWIG_ARGS -module ${TINYSPLINE_PACKAGE_NAME}
				CXX11
			)
		endif()
	endif()

	# PHP
	if(${TINYSPLINE_ENABLE_PHP})
		find_package(PHP COMPONENTS Zend REQUIRED)
		set(TINYSPLINE_PHP_VERSION "php5")
		if(${PHP_VERSION_NUMBER} MATCHES "^7")
			set(TINYSPLINE_PHP_VERSION "php7")
		endif()
		if(${TINYSPLINE_PHP_VERSION} STREQUAL "php7"
				AND ${SWIG_VERSION} VERSION_LESS 3.0.11)
			message(FATAL_ERROR "PHP 7 requires Swig 3.0.11 or later")
		elseif(${TINYSPLINE_PHP_VERSION} STREQUAL "php5"
				AND (${SWIG_VERSION} VERSION_GREATER 4.0.0
					OR ${SWIG_VERSION} VERSION_EQUAL 4.0.0))
			message(FATAL_ERROR "Swig 4 does not support PHP 5 anymore")
		else()
			include_directories(${PHP_INCLUDE_DIRS})
			tinyspline_add_swig_library(
				TARGET ${TINYSPLINE_PHP_CMAKE_TARGET}
				LANG ${TINYSPLINE_PHP_VERSION}
				OUTPUT ${TINYSPLINE_PHP_SOURCE_DIRECTORY}
				FLAGS ${PHP_INCLUDE_DIRS}
			)
			add_custom_command(
				TARGET ${TINYSPLINE_SWIG_CMAKE_TARGET}
				POST_BUILD
				COMMAND
					${CMAKE_COMMAND}
					-E copy
					"${TINYSPLINE_PHP_SOURCE_DIRECTORY}/${TINYSPLINE_PHP_CMAKE_TARGET}.php"
					"${TINYSPLINE_PHP_INTERFACE_FILE}"
			)
		endif()
	endif()

	# Python
	if(${TINYSPLINE_ENABLE_PYTHON})
		if(${TINYSPLINE_PYTHON_VERSION} STREQUAL "2")
			set(Python_ADDITIONAL_VERSIONS
				2.7 2.6 2.5 2.4 2.3 2.2 2.1 2.0)
		elseif(${TINYSPLINE_PYTHON_VERSION} STREQUAL "3")
			set(Python_ADDITIONAL_VERSIONS
				3.9 3.8 3.7 3.6 3.5 3.4 3.3 3.2 3.1 3.0)
		endif()
		find_package(PythonLibs REQUIRED)
		if(${TINYSPLINE_PYTHON_VERSION} STREQUAL "2"
					AND NOT ${PYTHONLIBS_VERSION_STRING} MATCHES "^2."
				OR ${TINYSPLINE_PYTHON_VERSION} STREQUAL "3"
					AND NOT ${PYTHONLIBS_VERSION_STRING} MATCHES "^3.")
			message(FATAL_ERROR "Unable to find requested Python version")
		endif()
		include_directories(${PYTHON_INCLUDE_DIRS})
		if(${PYTHONLIBS_VERSION_STRING} MATCHES "^3.")
			set(TINYSPLINE_PYTHON_DEFINES "-py3")
		endif()
		tinyspline_add_swig_library(
			TARGET ${TINYSPLINE_PYTHON_CMAKE_TARGET}
			LANG python
			OUTPUT ${TINYSPLINE_PYTHON_SOURCE_DIRECTORY}
			LIBS ${PYTHON_LIBRARIES}
			SWIG_ARGS ${TINYSPLINE_PYTHON_DEFINES}
		)
		add_custom_command(
			TARGET ${TINYSPLINE_SWIG_CMAKE_TARGET}
			POST_BUILD
			COMMAND
				${CMAKE_COMMAND} -E copy
				"${TINYSPLINE_PYTHON_SOURCE_DIRECTORY}/${TINYSPLINE_PYTHON_CMAKE_TARGET}.py"
				"${TINYSPLINE_PYTHON_INTERFACE_FILE}"
		)
	endif()

	# R
	if(${TINYSPLINE_ENABLE_R})
		find_package(R COMPONENTS Rcpp REQUIRED)
		include_directories(${R_INCLUDE_DIRS})
		tinyspline_add_swig_library(
			TARGET ${TINYSPLINE_R_CMAKE_TARGET}
			LANG r
			OUTPUT ${TINYSPLINE_R_SOURCE_DIRECTORY}
			LIBS ${R_LIBRARIES}
		)
		add_custom_command(
			TARGET ${TINYSPLINE_SWIG_CMAKE_TARGET}
			POST_BUILD
			COMMAND
				${CMAKE_COMMAND}
				-E copy
				"${TINYSPLINE_R_SOURCE_DIRECTORY}/${TINYSPLINE_R_CMAKE_TARGET}.R"
				"${TINYSPLINE_R_INTERFACE_FILE}"
		)
	endif()

	# Ruby
	if(${TINYSPLINE_ENABLE_RUBY})
		find_package(Ruby REQUIRED)
		include_directories(${RUBY_INCLUDE_DIRS})
		tinyspline_add_swig_library(
			TARGET ${TINYSPLINE_RUBY_CMAKE_TARGET}
			LANG ruby
			OUTPUT ${TINYSPLINE_RUBY_SOURCE_DIRECTORY}
			LIBS ${RUBY_LIBRARY}
		)
	endif()
endif()



###############################################################################
### Setup constants that are used to determine whether a certain interface is
### available, that is, the corresponding TINYSPLINE_ENABLE_*** option is true
### and all requirements to build interface *** are met.
#
# TINYSPLINE_WITH_***
#   Is 'ON' if interface *** is available, 'OFF' otherwise.
###############################################################################
function (tinyspline_set_on_off varName varValue)
	if(${varValue})
		set(${varName} ON PARENT_SCOPE)
	else()
		set(${varName} OFF PARENT_SCOPE)
	endif()
endfunction()
function (tinyspline_set_on_off_swig varName varValue)
	if(SWIG_FOUND AND ${varValue})
		set(${varName} ON PARENT_SCOPE)
	else()
		set(${varName} OFF PARENT_SCOPE)
	endif()
endfunction()
tinyspline_set_on_off(TINYSPLINE_WITH_CXX TINYSPLINE_ENABLE_CXX)
tinyspline_set_on_off_swig(TINYSPLINE_WITH_CSHARP TINYSPLINE_ENABLE_CSHARP)
tinyspline_set_on_off_swig(TINYSPLINE_WITH_DLANG TINYSPLINE_ENABLE_DLANG)
tinyspline_set_on_off(TINYSPLINE_WITH_GOLANG FALSE)
tinyspline_set_on_off(TINYSPLINE_WITH_JAVA JNI_FOUND)
tinyspline_set_on_off(TINYSPLINE_WITH_LUA LUA_FOUND)
tinyspline_set_on_off(TINYSPLINE_WITH_OCTAVE OCTAVE_FOUND)
tinyspline_set_on_off(TINYSPLINE_WITH_PHP PHP_FOUND)
tinyspline_set_on_off(TINYSPLINE_WITH_PYTHON PYTHONLIBS_FOUND)
tinyspline_set_on_off(TINYSPLINE_WITH_RUBY RUBY_FOUND)
tinyspline_set_on_off(TINYSPLINE_WITH_R R_FOUND)



###############################################################################
### Configure language-specific build systems.
###############################################################################
# Dub
file(REMOVE "${CMAKE_BINARY_DIR}/dub.json")
if(${TINYSPLINE_WITH_DLANG})
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/dub.json.in"
		"${CMAKE_BINARY_DIR}/dub.json"
		@ONLY
	)
endif()

# Luarocks
file(REMOVE "${CMAKE_BINARY_DIR}/tinyspline-${TINYSPLINE_VERSION}-1.rockspec")
if(${TINYSPLINE_WITH_LUA})
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/tinyspline.rockspec.in"
		"${CMAKE_BINARY_DIR}/tinyspline-${TINYSPLINE_VERSION}-1.rockspec"
		@ONLY
	)
endif()

# Nuget
file(REMOVE "${CMAKE_BINARY_DIR}/.nuspec")
if(${TINYSPLINE_WITH_CSHARP})
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/.nuspec.in"
		"${CMAKE_BINARY_DIR}/.nuspec"
		@ONLY
	)
endif()

# Maven
file(REMOVE "${CMAKE_BINARY_DIR}/pom.xml")
if(${TINYSPLINE_WITH_JAVA})
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/pom.xml.in"
		"${CMAKE_BINARY_DIR}/pom.xml"
		@ONLY
	)
endif()

# Distutils
file(REMOVE "${CMAKE_BINARY_DIR}/setup.py")
if(${TINYSPLINE_WITH_PYTHON})
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/setup.py.in"
		"${CMAKE_BINARY_DIR}/setup.py"
		@ONLY
	)
endif()

# Gem
file(REMOVE "${CMAKE_BINARY_DIR}/tinyspline.gemspec")
if(${TINYSPLINE_WITH_RUBY})
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/tinyspline.gemspec.in"
		"${CMAKE_BINARY_DIR}/tinyspline.${TINYSPLINE_PLATFORM}.gemspec"
		@ONLY
	)
endif()



###############################################################################
### Print summary.
###############################################################################
string(STRIP "${CMAKE_C_FLAGS} ${TINYSPLINE_LIBRARY_C_FLAGS}" TINYSPLINE_SUMMARY_C_FLAGS)
string(STRIP "${CMAKE_CXX_FLAGS} ${TINYSPLINE_LIBRARY_CXX_FLAGS}" TINYSPLINE_SUMMARY_CXX_FLAGS)
string(STRIP "${CMAKE_CXX_FLAGS} ${TINYSPLINE_BINDING_CXX_FLAGS}" TINYSPLINE_SUMMARY_BINDING_FLAGS)

message(STATUS
"
******************* TinySpline Configuration Summary *******************
Platform:
  Id: ${TINYSPLINE_PLATFORM}

Interface Configuration:
  With single precision  (default: OFF): ${TINYSPLINE_FLOAT_PRECISION}

Compiler Configuration:
  Compiler:       ${CMAKE_CXX_COMPILER}
  C flags:        ${TINYSPLINE_SUMMARY_C_FLAGS}
  C++ flags:      ${TINYSPLINE_SUMMARY_CXX_FLAGS}
  Binding flags:  ${TINYSPLINE_SUMMARY_BINDING_FLAGS}
  Definitions:    ${TINYSPLINE_DEFINITIONS}
  Debug postfix:  ${TINYSPLINE_DEBUG_POSTFIX}
  Toolchain file: ${CMAKE_TOOLCHAIN_FILE}

Available Interfaces:
  C++:          ${TINYSPLINE_WITH_CXX}

  C\#:           ${TINYSPLINE_WITH_CSHARP}
    Compiler: ${TINYSPLINE_CSHARP_COMPILER}

  D:            ${TINYSPLINE_WITH_DLANG}

  Go:           ${TINYSPLINE_WITH_GOLANG}
    (Not supported yet)

  Java:         ${TINYSPLINE_WITH_JAVA}
    Version:  ${Java_VERSION_STRING}
    Include:  ${JNI_INCLUDE_DIRS}
    Library:  ${JNI_LIBRARIES}
    Compiler: ${Java_JAVAC_EXECUTABLE}
    Archiver: ${Java_JAR_EXECUTABLE}

  Lua:          ${TINYSPLINE_WITH_LUA}
    Version:  ${LUA_VERSION_STRING}
    Include:  ${LUA_INCLUDE_DIR}
    Library:  ${LUA_LIBRARIES}

  Octave:       ${TINYSPLINE_WITH_OCTAVE}
    Version:  ${OCTAVE_VERSION_STRING}
    Include:  ${OCTAVE_INCLUDE_DIRS}
    Library:  ${OCTAVE_LIBRARIES}

  PHP:          ${TINYSPLINE_WITH_PHP}
    Version:  ${PHP_VERSION_STRING}
    Include:  ${PHP_INCLUDE_DIRS}

  Python:       ${TINYSPLINE_WITH_PYTHON}
    Version:  ${PYTHONLIBS_VERSION_STRING}
    Include:  ${PYTHON_INCLUDE_DIRS}
    Library:  ${PYTHON_LIBRARIES}

  R:            ${TINYSPLINE_WITH_R}
    Version:  ${R_VERSION}
    Include:  ${R_INCLUDE_DIRS}
    Library:  ${R_LIBRARIES}

  Ruby:         ${TINYSPLINE_WITH_RUBY}
    Version:  ${RUBY_VERSION}
    Include:  ${RUBY_INCLUDE_DIRS}
    Library:  ${RUBY_LIBRARY}

Installation:
  Prefix: ${CMAKE_INSTALL_PREFIX}
")
