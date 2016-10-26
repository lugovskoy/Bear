set(CMAKE_MACOSX_RPATH 1)

include(CheckFunctionExists)
include(CheckSymbolExists)
check_function_exists(execve HAVE_EXECVE)
check_function_exists(execv HAVE_EXECV)
check_function_exists(execvpe HAVE_EXECVPE)
check_function_exists(execvp HAVE_EXECVP)
check_function_exists(execvP HAVE_EXECVP2)
check_function_exists(exect HAVE_EXECT)
check_function_exists(execl HAVE_EXECL)
check_function_exists(execlp HAVE_EXECLP)
check_function_exists(execle HAVE_EXECLE)
check_function_exists(posix_spawn HAVE_POSIX_SPAWN)
check_function_exists(posix_spawnp HAVE_POSIX_SPAWNP)
check_symbol_exists(_NSGetEnviron crt_externs.h HAVE_NSGETENVIRON)

find_package(Threads REQUIRED)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.in ${CMAKE_CURRENT_BINARY_DIR}/config.h)
include_directories(${CMAKE_CURRENT_BINARY_DIR})

# 64 bit
add_library(ear64 SHARED ear.c)
target_link_libraries(ear64 ${CMAKE_DL_LIBS})
set_target_properties(ear64 PROPERTIES LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/lib64")
set_target_properties(ear64 PROPERTIES OUTPUT_NAME "ear")
set_target_properties(ear64 PROPERTIES INSTALL_RPATH "@loader_path/${CMAKE_INSTALL_LIBDIR}64")
if(THREADS_HAVE_PTHREAD_ARG)
    set_property(TARGET ear64 PROPERTY COMPILE_OPTIONS "-pthread")
    set_property(TARGET ear64 PROPERTY INTERFACE_COMPILE_OPTIONS "-pthread")
endif()
if(CMAKE_THREAD_LIBS_INIT)
    target_link_libraries(ear64 "${CMAKE_THREAD_LIBS_INIT}")
endif()


include(GNUInstallDirs)
install(TARGETS ear64
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}64")

# 32 bit  
add_library(ear32 SHARED ear.c)
target_link_libraries(ear32 ${CMAKE_DL_LIBS})
set_target_properties(ear32 PROPERTIES LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/lib")
set_target_properties(ear32 PROPERTIES OUTPUT_NAME "ear")
set_target_properties(ear32 PROPERTIES COMPILE_FLAGS "-m32" LINK_FLAGS "-m32")
set_target_properties(ear32 PROPERTIES INSTALL_RPATH "@loader_path/${CMAKE_INSTALL_LIBDIR}")

if(THREADS_HAVE_PTHREAD_ARG)
    set_property(TARGET ear32 PROPERTY COMPILE_OPTIONS "-pthread")
    set_property(TARGET ear32 PROPERTY INTERFACE_COMPILE_OPTIONS "-pthread")
endif()
if(CMAKE_THREAD_LIBS_INIT)
    target_link_libraries(ear32 "${CMAKE_THREAD_LIBS_INIT}")
endif()

install(TARGETS ear32
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})

