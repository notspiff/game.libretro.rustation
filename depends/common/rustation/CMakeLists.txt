project(rustation)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

include(ExternalProject)

set(btype "release")
if(CMAKE_BUILD_TYPE MATCHES Debug)
  set(btype "debug")
endif()

if("${CORE_SYSTEM_NAME}" STREQUAL "windows")
  message(FATAL_ERROR "${PROJECT_NAME} needs windows build command in CMakeLists.txt!")
elseif("${CORE_SYSTEM_NAME}" STREQUAL "linux")
  set(BUILD_COMMAND cargo build --${btype})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "darwin" OR "${CORE_SYSTEM_NAME}" STREQUAL "osx")
  message(FATAL_ERROR "${PROJECT_NAME} needs OSX build command in CMakeLists.txt!")
elseif("${CORE_SYSTEM_NAME}" STREQUAL "ios")
  message(FATAL_ERROR "${PROJECT_NAME} needs iOS build command in CMakeLists.txt!")
elseif("${CORE_SYSTEM_NAME}" STREQUAL "android")
  message(FATAL_ERROR "${PROJECT_NAME} needs Android build command in CMakeLists.txt!")
elseif("${CORE_SYSTEM_NAME}" STREQUAL "rbpi")
  message(FATAL_ERROR "${PROJECT_NAME} needs RPi build command in CMakeLists.txt!")
elseif("${CORE_SYSTEM_NAME}" STREQUAL "freebsd")
  message(FATAL_ERROR "${PROJECT_NAME} needs FreeBSD build command in CMakeLists.txt!")
else()
  message(FATAL_ERROR "${PROJECT_NAME} - Unknown system: ${CORE_SYSTEM_NAME}")
endif()

externalproject_add(${PROJECT_NAME}
                    SOURCE_DIR ${PROJECT_SOURCE_DIR}
                    PREFIX ${PROJECT_NAME}
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND ${BUILD_COMMAND}
                    BUILD_IN_SOURCE 1)

#install the generated shared library
install(FILES ${PROJECT_SOURCE_DIR}/target/${btype}/librustation_retro${CMAKE_SHARED_LIBRARY_SUFFIX}
        DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)

# write the config.cmake script
file(WRITE ${CMAKE_INSTALL_PREFIX}/libretro-rustation-config.cmake "set(RUSTATION_LIB ${CMAKE_INSTALL_PREFIX}/lib/librustation_retro${CMAKE_SHARED_LIBRARY_SUFFIX})")
