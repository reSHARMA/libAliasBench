include_directories(include)

add_library(AliasBench SHARED
    Benchmark.cpp
)
set_target_properties(AliasBench PROPERTIES
    SOVERSION 0
    COMPILE_FLAGS "-std=c++14 -fno-rtti"
)

include(GNUInstallDirs)
install(TARGETS AliasBench
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
