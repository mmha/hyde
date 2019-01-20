# Copyright 2018 Adobe
# All Rights Reserved.

# NOTICE: Adobe permits you to use, modify, and distribute this file in
# accordance with the terms of the Adobe license agreement accompanying
# it. If you have received this file from a source other than Adobe,
# then your use, modification, or distribution of it requires the prior
# written permission of Adobe. 


cmake_minimum_required(VERSION 3.12)

project(hyde LANGUAGES CXX)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Clang REQUIRED clangTooling libClang clangASTMatchers CONFIG)
find_package(yaml-cpp REQUIRED CONFIG)

set(Boost_USE_STATIC_LIBS ON)
find_package(Boost REQUIRED system filesystem)

file(GLOB EMITTER_FILES CONFIGURE_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/emitters/*.cpp)
file(GLOB MATCHER_FILES CONFIGURE_DEPENDS ${PROJECT_SOURCE_DIR}/matchers/*.cpp)
file(GLOB SRC_FILES CONFIGURE_DEPENDS ${PROJECT_SOURCE_DIR}/sources/*.cpp)

#TODO: use target_source
add_executable(hyde ${EMITTER_FILES} ${MATCHER_FILES} ${SRC_FILES})
if (NOT LLVM_ENABLE_RTTI)
    target_compile_options (hyde PRIVATE -fno-rtti)
endif()

target_include_directories(hyde PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)

target_link_libraries(hyde
	                  yaml-cpp
					  Boost::filesystem
					  Boost::system
                      clangASTMatchers
                      clangBasic
                      clangTooling
                      )
