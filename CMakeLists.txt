cmake_minimum_required(VERSION 3.2)
project (acme_lw_client)

include(CheckIncludeFileCXX)
check_include_file_cxx(filesystem STD_FILESYSTEM)
if (STD_FILESYSTEM)
   set (CMAKE_CXX_STANDARD 17)
else()
   set (CMAKE_CXX_STANDARD 14)
endif()

OPTION(STAGING "Run against the Let's Encrypt staging environment" OFF)
if (STAGING)
   add_definitions (-DSTAGING)
endif ()

OPTION(ASAN "Build with asan" OFF)
if (ASAN)
   set(CMAKE_BUILD_TYPE "Debug")
   set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fsanitize=address,leak -fno-omit-frame-pointer")
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address,leak -fno-omit-frame-pointer")
endif ()

add_compile_options (-Wall -Wextra -pedantic -Wshadow -Wpointer-arith -Wcast-qual -Werror)

add_subdirectory (lib)
add_subdirectory (main)
