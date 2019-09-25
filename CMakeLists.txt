cmake_minimum_required(VERSION 3.0)
project(example-project)
#Regrad the project as a library
set(USE_${PROJECT_NAME}_AS_LIBRARY FALSE)
##Find and include easylib
if(NOT EASYLIB_ROOT_DIR)
    message(WARNING "You have to specify easylib root directory before call it ...")
    mark_as_advanced(EASYLIB_ROOT_DIR)
    return()
else(NOT EASYLIB_ROOT_DIR)
    list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR} ${EASYLIB_ROOT_DIR}/cmake)
    include(easylib)
endif(NOT EASYLIB_ROOT_DIR)

# Add a library <project name>
set(${PROJECT_NAME}_INCLUDE_DIR ${CMAKE_CURRENT_LIST_DIR}/include)
add_library(${PROJECT_NAME} ${CMAKE_CURRENT_LIST_DIR}/src)

target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_NAME}_INCLUDE_DIR)
target_link_libraries(${PRIJECT_NAME} PUBLIC elib::core) #And any library you need in easylib


if(NOT USE_${PROJECT_NAME}_AS_LIBRARY)
    add_executable(${PROJECT_NAME}_bin main.cpp)
    target_link_libraries(${PROJECT_NAME}_bin ${PROJECT_NAME})
endif()

