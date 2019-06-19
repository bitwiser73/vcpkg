cmake_minimum_required(VERSION 3.11)
cmake_policy(VERSION 3.11)

option(SSDEEP_DISABLE_POSITION_ARRAY
    "Disable bit-parallel string operations." OFF)

option(INSTALL_HEADERS "Install public header files" ON)

project(ssdeep C)

include(CheckIncludeFile)

check_include_file(dirent.h HAVE_DIRENT_H)
check_include_file(dlfcn.h HAVE_DLFCN_H)
check_include_file(fcntl.h HAVE_FCNTL_H)
check_include_file(ftello.h HAVE_FSEEKO)
check_include_file(inttypes.h HAVE_INTTYPES_H)
check_include_file(libgen.h HAVE_LIBGEN_H)
check_include_file(memory.h HAVE_MEMORY_H)
check_include_file(stdint.h HAVE_STDINT_H)
check_include_file(stdlib.h HAVE_STDLIB_H)
check_include_file(strings.h HAVE_STRINGS_H)
check_include_file(string.h HAVE_STRING_H)
check_include_file(sys/disk.h HAVE_SYS_DISK_H)
check_include_file(sys/ioctl.h HAVE_SYS_IOCTL_H)
check_include_file(sys/mount.h HAVE_SYS_MOUNT_H)
check_include_file(sys/param.h HAVE_SYS_PARAM_H)
check_include_file(sys/stat.h HAVE_SYS_STAT_H)
check_include_file(sys/types.h HAVE_SYS_TYPES_H)
check_include_file(unistd.h HAVE_UNISTD_H)
check_include_file(wchar.h HAVE_WCHAR_H)

configure_file(config.h.in ${CMAKE_CURRENT_SOURCE_DIR}/config.h)

add_library(ssdeep
    cycles.cpp
    dig.cpp
    edit_dist.c
    edit_dist.c
    engine.cpp
    filedata.cpp
    find-file-size.c
    fuzzy.c
    fuzzy.c
    helpers.cpp
    main.cpp
    match.cpp
    ui.cpp
)

target_compile_definitions(ssdeep
    PRIVATE
        -DHAVE_CONFIG_H
)

target_include_directories(ssdeep
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

#target_compile_options(ssdeep
#    PRIVATE
#        $<$<CONFIG:RELEASE>:/guard:cf>
#)

set(PUBLIC_HEADERS
    ssdeep.h
)

if(INSTALL_HEADERS)
    set_target_properties(ssdeep PROPERTIES PUBLIC_HEADER "${PUBLIC_HEADERS}")
endif()

install(
    TARGETS ssdeep
    EXPORT ssdeep
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    PUBLIC_HEADER DESTINATION include
)

install(
    EXPORT ssdeep
    DESTINATION share/ssdeep
    FILE ssdeepConfig.cmake
    NAMESPACE ssdeep::
    CONFIGURATIONS Release
)
