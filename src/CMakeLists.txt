cmake_minimum_required(VERSION 2.6)
project(tapioca)
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
include(cmake/tags.cmake)

find_package(BDB REQUIRED)
find_package(LIBEVENT REQUIRED)
find_package(LIBPAXOS REQUIRED)
find_package(LIBUUID REQUIRED)
find_package(MSGPACK REQUIRED)

# System libraries
set(TAPIOCA_LINKER_LIBS ${TAPIOCA_LINKER_LIBS} pthread )
if(NOT APPLE)
	set(TAPIOCA_LINKER_LIBS ${TAPIOCA_LINKER_LIBS} m rt pthread)
endif()

SET (CMAKE_C_FLAGS_DEBUG " -g3 ")

add_subdirectory(util)
add_subdirectory(db)
add_subdirectory(libtapioca)
add_subdirectory(app/cm)
add_subdirectory(app/rec)
add_subdirectory(app/shell)
add_subdirectory(app/tapioca)
add_subdirectory(test)

if (BUILD_UNIT)
	find_package(GTEST REQUIRED)
	find_package(GSL REQUIRED)
	add_subdirectory(unit)
endif()

INSTALL(FILES ../script/launchers.sh ../script/launch_all.sh ../script/acc.sh ../script/cm_tap.sh DESTINATION scripts)
INSTALL(FILES ../config/1.cfg ../config/2.cfg ../config/paxos_config.cfg DESTINATION config)
