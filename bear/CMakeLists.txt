configure_file(${CMAKE_CURRENT_SOURCE_DIR}/main.py.in ${CMAKE_CURRENT_BINARY_DIR}/bear)

include(GNUInstallDirs)
install(PROGRAMS ${CMAKE_CURRENT_BINARY_DIR}/bear ${CMAKE_CURRENT_SOURCE_DIR}/make
    DESTINATION ${CMAKE_INSTALL_BINDIR})
