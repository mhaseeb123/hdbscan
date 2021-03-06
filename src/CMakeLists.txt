
file(GLOB HDBSCAN_CSRC "*.c")
file(GLOB HDBSCAN_CPPSRC "*.cpp")

add_library(${HDBSCAN_LIBRARY}-${HDBSCAN_VERSION} SHARED ${HDBSCAN_CSRC} ${HDBSCAN_CPPSRC})
target_link_libraries(${HDBSCAN_LIBRARY}-${HDBSCAN_VERSION} LINK_PUBLIC ${GLIB2_LIBRARIES} ${GSL_LIBRARIES} m LINK_PRIVATE ${LISTLIB_LIBRARY}_static ${GNULIB_LIBRARY}_static)

add_library(${HDBSCAN_LIBRARY}_static STATIC ${HDBSCAN_CSRC} ${HDBSCAN_CPPSRC})
target_link_libraries(${HDBSCAN_LIBRARY}_static LINK_PUBLIC ${GLIB2_LIBRARIES} ${GSL_LIBRARIES} m LINK_PRIVATE ${LISTLIB_LIBRARY}_static ${GNULIB_LIBRARY}_static)

include_directories(${HDBSCAN_INCLUDE_DIR} ${GLIB2_INCLUDE_DIRS} ${LISTLIB_INCLUDE_DIR} ${GNULIB_INCLUDE_DIR})
  
install(TARGETS ${HDBSCAN_LIBRARY}-${HDBSCAN_VERSION}
  # IMPORTANT: Add the hdbscan library to the "export-set"
  EXPORT HdbscanTargets
  RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin
  LIBRARY DESTINATION "${INSTALL_LIB_DIR}" COMPONENT shlib)
  
INSTALL(CODE "execute_process( \
  COMMAND ${CMAKE_COMMAND} -E create_symlink \
  ${INSTALL_LIB_DIR}/lib${HDBSCAN_LIBRARY}-${HDBSCAN_VERSION}.so \
  ${INSTALL_LIB_DIR}/lib${HDBSCAN_LIBRARY}.so   \
  )"
)