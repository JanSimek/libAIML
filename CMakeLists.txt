cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(libaiml)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

find_package(libXml2 REQUIRED)
find_package(v8 REQUIRED)

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
	src/aisl_parser.cpp     
	src/config_parser.cpp   
	src/errors.cpp          
	src/graphmaster.h       
	src/serializer.h        
	src/user.h
	src/aiml.h              
	src/aisl_parser.h       
	src/config_parser.h     
	src/errors.h            
	src/javascript.cpp      
	src/stuff.cpp           
	src/user_manager.cpp
	src/aiml_parser.cpp     
	src/caiml_parser.cpp    
	src/core.cpp            
	src/global.h            
	src/javascript.h        
	src/template_parser.cpp src/user_manager.h
	src/aiml_parser.h       
	src/caiml_parser.h      
	src/core.h              
	src/graphmaster.cpp     
	src/serializer.cpp      
	src/user.cpp
)

include_directories(
	${CMAKE_SOURCE_DIR}/src/std_utils
	${PCRECPP_INCLUDE_DIR}
	${LIBXML2_INCLUDE_DIR}
	${V8_INCLUDE_DIR}
)

target_link_libraries(aiml ${LIBXML2_LIBRARIES})

target_link_libraries(test_app aiml ${V8_LIBRARY} std_utils ${PCRECPP_LIBRARY})
