cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(libaiml)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

find_package(libXml2 REQUIRED)

find_package(PkgConfig)
#pkg_check_modules(PCRECPP pcrecpp)

find_path(PCRECPP_INCLUDE_DIR pcrecpp.h
          HINTS ${PCRECPP_INCLUDEDIR} ${PCRECPP_INCLUDE_DIRS})
find_library(PCRECPP_LIBRARY NAMES pcrecpp
          HINTS ${PCRECPP_LIBDIR} ${PCRECPP_LIBRARY_DIRS})

add_library(std_utils 
	std_utils/src/std_util.h 
	std_utils/src/std_utils.cpp 
	std_utils/src/cconfig.h 
	std_utils/src/cconfig.cpp
)

add_executable(test_app 
	test_app/main.cpp
)

add_library(aiml 
        src/aiml.cpp
        src/aiml.h
        src/aiml_parser.cpp
        src/aiml_parser.h
        src/template_parser.cpp
        src/config_parser.cpp
        src/config_parser.h
        src/errors.cpp
        src/errors.h
        src/graphmaster.cpp
        src/graphmaster.h
        src/serializer.cpp
        src/serializer.h
        src/user.cpp
        src/user.h
        src/user_manager.cpp
        src/user_manager.h
        src/core.cpp
        src/core.h
        src/global.h
        src/stuff.cpp
)

include_directories(
	${CMAKE_SOURCE_DIR}/src/std_utils
	${PCRECPP_INCLUDE_DIR}
        ${LIBXML2_INCLUDE_DIRS}
)

add_definitions(-DENABLE_PCRECPP=1 -DVERSION="0.1")

target_link_libraries(aiml ${LIBXML2_LIBRARIES})

target_link_libraries(test_app aiml std_utils ${PCRECPP_LIBRARY})
