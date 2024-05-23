cmake_minimum_required(VERSION "3.22")

project("iio-hyprland")

#
# Generate `compile_commands.json` for `Neovim` LSP needed
#
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

# # Compiler and flags #
set(CMAKE_C_COMPILER "clang")
# set(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_C_FLAGS "-pedantic-errors -ansi -Wextra -Wall -Werror -std=gnu2x")
set(CMAKE_CXX_FLAGS "-pedantic-errors -Wextra -Wall -Werror -std=gnu++20")

#
# Find installed libraries as compilation dependencies
# https://cmake.org/cmake/help/latest/command/find_package.html
#
find_package(DBus1 REQUIRED)

#
# Extra include path
#
include_directories(${DBus1_INCLUDE_DIRS})

#
# Main exectuable
#
add_executable("${PROJECT_NAME}" "src/main.c")

#
# Link to libraries
#
target_link_libraries("${PROJECT_NAME}" ${DBus1_LIBRARIES})


# -----------------------------------------------------------------------------
# The settings below just copy the target into different output folder
#
# By default, `make install` copies all targets to `/usr/local` which means:
#
# /usr/local/bin
# /usr/local/lib
# /usr/local/include
#
# Of course, you can change it by running `cmake` like below if you want:
#
# `cmake .. -DCMAKE_INSTALL_PREFIX=/cpp/build`
#
#  After that, `make install` copies all targets to:
#
# /cpp/build/bin
# /cpp/build/lib
# /cpp/build/include
#
#
# syntax: install(TARGETS targets... [EXPORT <export-name>]
#       [[ARCHIVE|LIBRARY|RUNTIME|FRAMEWORK|BUNDLE|
#         PRIVATE_HEADER|PUBLIC_HEADER|RESOURCE]
#        [DESTINATION <dir>]
#        [INCLUDES DESTINATION [<dir> ...]]
#        [PERMISSIONS permissions...]
#        [CONFIGURATIONS [Debug|Release|...]]
#        [COMPONENT <component>]
#        [OPTIONAL] [NAMELINK_ONLY|NAMELINK_SKIP]
#       ] [...]) 
#
#       `Executables` are treated as `RUNTIME` targets
#
# More details from here:
# https://cmake.org/cmake/help/v3.0/command/install.html?highlight=install
# -----------------------------------------------------------------------------
install(
    TARGETS "${PROJECT_NAME}"
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
)

#
# Debug log
#
message(">>> CMAKE_EXPORT_COMPILE_COMMANDS: ${CMAKE_EXPORT_COMPILE_COMMANDS}")
message(">>> CMAKE_SYSTEM_NAME: ${CMAKE_SYSTEM_NAME}")
message(">>> CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")
message(">>> CMAKE_C_COMPILER: ${CMAKE_C_COMPILER}")
message(">>> CMAKE_C_FLAGS: ${CMAKE_C_FLAGS}")
message(">>> CMAKE_C_FLAGS_DEBUG: ${CMAKE_C_FLAGS_DEBUG}")
message(">>> CMAKE_C_FLAGS_RELEASE: ${CMAKE_C_FLAGS_RELEASE}")
message(">>> CMAKE_CXX_COMPILER: ${CMAKE_CXX_COMPILER}")
message(">>> CMAKE_CXX_FLAGS: ${CMAKE_CXX_FLAGS}")
message(">>> CMAKE_CXX_FLAGS_DEBUG: ${CMAKE_CXX_FLAGS_DEBUG}")
message(">>> CMAKE_CXX_FLAGS_RELEASE: ${CMAKE_CXX_FLAGS_RELEASE}")
message(">>> dbus-1_FOUND: ${DBus1_FOUND}")
