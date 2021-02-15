cmake_minimum_required(VERSION 3.0.0)
project(coro-test VERSION 0.1.0)

include(CTest)
enable_testing()

#
# Target defaults
#
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "-stdlib=libc++ -fcoroutines-ts")

#
# Project definition
#
project(coro-test)
add_executable(coro-test main.cpp)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

# target_compile_features(coro-test PUBLIC cxx_std_17)
#
# Boost
#
# find_package(Boost REQUIRED COMPONENTS system)
# include_directories(${Boost_INCLUDE_DIR})

#
# find cppcoro
#
cmake_policy(SET CMP0057 NEW)
list(APPEND CMAKE_MODULE_PATH "/usr/lib/cmake/cppcoro/")
find_package(Coroutines REQUIRED COMPONENTS Experimental) #  and/or Final)
target_link_libraries(coro-test PRIVATE cppcoro)
# target_include_directories(coro-test PRIVATE "/opt/cppcoro/include")
