# 1. Requirements
cmake_minimum_required(VERSION 2.8.11)
project(notice_send_global)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 2.Configuration
add_compile_options(-Wno-deprecated)  # This is just to avoid some spammy warnings
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)  # This line is optional. It's just for certain IDEs to use

# 3. Boilerplate
set(USD_LIBRARY_DIRECTORY $ENV{USD_INSTALL_ROOT}/lib)
set(USD_INCLUDE_DIRECTORY $ENV{USD_INSTALL_ROOT}/include)

find_library(USD_ARCH arch HINTS ${USD_LIBRARY_DIRECTORY})
find_library(USD_BOOST_PYTHON
    boost_python NAMES boost_python36
    HINTS ${USD_LIBRARY_DIRECTORY}
)
find_library(USD_SDF sdf HINTS ${USD_LIBRARY_DIRECTORY})
find_library(USD_TF tf HINTS ${USD_LIBRARY_DIRECTORY})
find_library(USD_USD usd HINTS ${USD_LIBRARY_DIRECTORY})
find_library(USD_USDGEOM usdGeom HINTS ${USD_LIBRARY_DIRECTORY})

find_package(PythonLibs REQUIRED)

# 4. Include/Link Everything
add_executable(notice_send_global_executable
    main.cpp
)

set_target_properties(notice_send_global_executable
    PROPERTIES OUTPUT_NAME "run_it"
)

target_include_directories(notice_send_global_executable
PUBLIC
    ${PYTHON_INCLUDE_PATH}
    ${USD_INCLUDE_DIRECTORY}
)

target_link_libraries(
notice_send_global_executable
    ${PYTHON_LIBRARY}
    ${USD_ARCH}
    ${USD_BOOST_PYTHON}
    ${USD_SDF}
    ${USD_TF}
    ${USD_USDGEOM}
    ${USD_USD}
)
