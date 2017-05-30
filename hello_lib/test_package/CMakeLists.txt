project(PackageTest CXX)
cmake_minimum_required(VERSION 2.8.12)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

add_executable(example example.cpp)

conan_find_libraries_abs_path("${CONAN_LIBS}" "${CONAN_LIB_DIRS}" CONAN_FULLPATH_LIBS)
target_link_libraries(example ${CONAN_FULLPATH_LIBS})
