cmake_minimum_required(VERSION 3.0)
project(AbletonLink
        VERSION 3.0.3
        LANGUAGES CXX)

#set (CMAKE_CXX_STANDARD 11)

set(TARGET_NAME ${PROJECT_NAME})

add_library(${TARGET_NAME} INTERFACE)
target_include_directories(${TARGET_NAME}
    INTERFACE
      "$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>"
      "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}>"
)

target_compile_features(${TARGET_NAME} INTERFACE cxx_generalized_initializers)
target_compile_definitions(${TARGET_NAME} INTERFACE LINK_PLATFORM_UNIX=1)
#target_link_libraries(${TARGET_NAME} INTERFACE AsioStandalone::AsioStandalone)

# install steps
include(GNUInstallDirs)

install(
    DIRECTORY ${CMAKE_SOURCE_DIR}/include/ableton
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})

install(TARGETS ${TARGET_NAME}  EXPORT ${PROJECT_NAME}-targets)

install(EXPORT ${PROJECT_NAME}-targets
  NAMESPACE  ${PROJECT_NAME}::
  DESTINATION  "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}"
)
configure_file(cmake_include/${PROJECT_NAME}Config.cmake.in cmake/${PROJECT_NAME}Config.cmake @ONLY)

include(CMakePackageConfigHelpers)
write_basic_package_version_file("${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config-version.cmake" COMPATIBILITY SameMajorVersion)
install(FILES       "${CMAKE_CURRENT_BINARY_DIR}/cmake/${TARGET_NAME}Config.cmake"
                    "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config-version.cmake"
        DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}")
