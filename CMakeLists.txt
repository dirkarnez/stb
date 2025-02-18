cmake_minimum_required(VERSION 3.24)
project (stb VERSION 1.0.0 LANGUAGES C CXX)

# add the stl_reader as a single target:
add_library(stb_image INTERFACE)

# see https://github.com/abseil/abseil-cpp
target_include_directories(stb_image
  INTERFACE
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/>"
  $<INSTALL_INTERFACE:include/stb>
)

add_library(stb::image ALIAS stb_image)

# set_target_properties(stb_image PROPERTIES DEFINE_SYMBOL IMAGE_EXPORTS)
# set_target_properties(stb_image PROPERTIES EXPORT_NAME stb::image)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

# generate the version file for the config file
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion # probably don't want to set this. Would need to check other options.
)

# create the cmake config files:
configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/cmake/Config.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake"
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stb
)

# install(
#   FILES
#   "${CMAKE_CURRENT_SOURCE_DIR}/stb_image.h"
#   "${CMAKE_CURRENT_SOURCE_DIR}/stb_image_resize.h"
#   "${CMAKE_CURRENT_SOURCE_DIR}/stb_image_write.h"
  
# )

# install(
#   DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/stb
#   FILES_MATCHING PATTERN "/*.h"
# )

# target_sources(stb_image INTERFACE 
#   "${CMAKE_CURRENT_SOURCE_DIR}/stb_image.h"
#   "${CMAKE_CURRENT_SOURCE_DIR}/stb_image_resize.h"
#   "${CMAKE_CURRENT_SOURCE_DIR}/stb_image_write.h")

install(FILES 
"${CMAKE_CURRENT_SOURCE_DIR}/stb_image.h"
"${CMAKE_CURRENT_SOURCE_DIR}/stb_image_resize.h"
"${CMAKE_CURRENT_SOURCE_DIR}/stb_image_write.h"
DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/stb)

install(
  TARGETS stb_image
  EXPORT stbTargets
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(
  EXPORT stbTargets 
  FILE stbTargets.cmake
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stb
  NAMESPACE stb::
)

install(
    FILES
    ${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stb
)


# install(DIRECTORY include/stl_reader DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# # define the export of our cmake target:
# install(
#   TARGETS stl_reader
#   EXPORT stl_readerTargets
#   LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
#   ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
#   RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
#   INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
# )

# install(
#   EXPORT stl_readerTargets
#   FILE stl_readerTargets.cmake
#   NAMESPACE stl_reader::
#   DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stl_reader
# )




# # install the configuration files that we using the helper macros above:
# install(
#   FILES
#   "${CMAKE_CURRENT_BINARY_DIR}/stl_readerConfig.cmake" # cmake config file
#   "${CMAKE_CURRENT_BINARY_DIR}/stl_readerConfigVersion.cmake" # cmake version file
#   DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stl_reader
# )

