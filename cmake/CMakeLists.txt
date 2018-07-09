# proj-config.cmake for the install tree.  It's installed in
# ${INSTALL_CMAKE_DIR} and @PROJECT_ROOT_DIR@ is the relative
# path to the root from there.  (Note that the whole install tree can
# be relocated.)
file(RELATIVE_PATH PROJECT_ROOT_DIR ${CMAKE_INSTALL_PREFIX}/${CMAKECONFIGDIR} ${CMAKE_INSTALL_PREFIX})

string(TOLOWER "${PROJECT_NAME}" PROJECT_NAME_LOWER)

# Make information about the cmake targets (the library and the tools)
# available.
install (EXPORT targets
  NAMESPACE ${PROJECT_NAME_LOWER}_
  FILE ${PROJECT_NAME_LOWER}ConfigInternal.cmake
  DESTINATION "${CMAKECONFIGDIR}")

include(CMakePackageConfigHelpers)

configure_package_config_file(
  "${CMAKE_CURRENT_LIST_DIR}/project-config.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/project-config.cmake"
  INSTALL_DESTINATION "${CMAKECONFIGDIR}")
configure_file(
  "${CMAKE_CURRENT_LIST_DIR}/project-config-version.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/project-config-version.cmake"
  @ONLY)

install (FILES
  "${CMAKE_CURRENT_BINARY_DIR}/project-config.cmake"
  DESTINATION "${CMAKECONFIGDIR}"
  RENAME "${PROJECT_NAME_LOWER}Config.cmake")
install (FILES
  "${CMAKE_CURRENT_BINARY_DIR}/project-config-version.cmake"
  DESTINATION "${CMAKECONFIGDIR}"
  RENAME "${PROJECT_NAME_LOWER}ConfigVersion.cmake")
