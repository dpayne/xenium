project(xenium)
cmake_minimum_required(VERSION 3.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Threads REQUIRED)
find_package(Doxygen)

option(BUILD_DOCUMENTATION "Create the HTML based documentation (requires Doxygen)" ${DOXYGEN_FOUND})

file(GLOB_RECURSE XENIUM_FILES xenium/*.hpp)

include_directories(
	.
	./3rdParty/json/include
	./3rdParty/config/include
)

if(CMAKE_BUILD_TYPE MATCHES Debug)
	add_definitions(-DDEBUG)
elseif(CMAKE_BUILD_TYPE MATCHES RelWithDebug)
	add_definitions(-DNDEBUG)
elseif(CMAKE_BUILD_TYPE MATCHES Release)
	add_definitions(-DNDEBUG)
endif()

if(BUILD_DOCUMENTATION)
	if(NOT DOXYGEN_FOUND)
		message(FATAL_ERROR "Doxygen is needed to build the documentation.")
	endif()

	add_custom_target(doc
			COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/doc/Doxyfile
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/doc
			COMMENT "Generating API documentation with Doxygen"
			VERBATIM)
endif()
