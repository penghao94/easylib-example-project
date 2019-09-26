cmake_minimum_required(VERSION 3.0)
project(example)
#Regrad the project as a library
set(USE_${PROJECT_NAME}_AS_LIBRARY FALSE)
##Find and include easylib
set(EASYLIB_ROOT_DIR "${EASYLIB_ROOT_DIR}" CACHE PATH "Easylib root directory" FORCE)

##options

if(NOT EASYLIB_ROOT_DIR)
    message(FATAL_ERROR "You have to specify easylib root directory before call it ...")
else(NOT EASYLIB_ROOT_DIR)
    list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR} ${EASYLIB_ROOT_DIR}/cmake)
    include(easylib)
endif(NOT EASYLIB_ROOT_DIR)

# Add a library <project name>
set(${PROJECT_NAME}_INCLUDE_DIR ${CMAKE_CURRENT_LIST_DIR}/include)
file(GLOB ${PROJECT_NAME}_SOURCE "${CMAKE_CURRENT_LIST_DIR}/src/*.cc" "${CMAKE_CURRENT_LIST_DIR}/src/*.cpp" "${CMAKE_CURRENT_LIST_DIR}/src/*.cxx")
add_library(${PROJECT_NAME}_core ${${PROJECT_NAME}_SOURCE})

target_include_directories(${PROJECT_NAME}_core PUBLIC ${${PROJECT_NAME}_INCLUDE_DIR})
target_link_libraries(${PROJECT_NAME}_core elib::core elib::ceres) #And any library you need in easylib
add_library(${PROJECT_NAME}::core ALIAS ${PROJECT_NAME}_core)

if(NOT USE_${PROJECT_NAME}_AS_LIBRARY)
    add_executable(${PROJECT_NAME}_bin main.cpp)
    target_link_libraries(${PROJECT_NAME}_bin ${PROJECT_NAME}::core)
endif()

