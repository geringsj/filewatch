cmake_minimum_required(VERSION 3.13)

# Project name
project(FileWatch VERSION 0.0.1 LANGUAGES C CXX)

option(FileWatch_Build_Example "Build the example")
option(FileWatch_Build_Tests "Build the unit tests")
option(FileWatch_Build_Tests_FileSystem "Build the unit c++17 > filesystem tests")
enable_testing()

# Enable c++11
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED on)

# Add debug flags
#if(MSVC)
#  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zi /MP")
#else()
#  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -Wall -Wextra -Weffc++")
#endif()

if(APPLE)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -framework CoreServices")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address")
endif()

# dial up the warnings
if(MSVC)
#  # Force to always compile with W4
#  if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
#    string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
#  #set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /permissive")
#  else()
#    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
#  endif()
elseif(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
  # Update if necessary
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wno-long-long -pedantic")
endif()


# create and configure the unit test target
add_subdirectory(tests)


if(FileWatch_Build_Example)
    add_subdirectory(example)
endif()


set(PROJECT_SOURCES
    "FileWatch.hpp"
)
add_library(${PROJECT_NAME} INTERFACE)
target_sources(${PROJECT_NAME} INTERFACE ${PROJECT_SOURCES})
target_include_directories(${PROJECT_NAME} INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

# add_subdirectory(example)
