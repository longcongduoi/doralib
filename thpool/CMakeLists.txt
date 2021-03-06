
# set(LIB_NAME "thpool")
string(REGEX REPLACE ".*/(.*)" "\\1" LIB_NAME ${CMAKE_CURRENT_SOURCE_DIR}) 

set(LIB_SRC_DIR 
  "." 
  "../common"
)

set(LIB_INC_DIR 
  "include/"
  "../common/include/"
)

set(INSTALL_LIBDIR "${CMAKE_INSTALL_PREFIX}/doralib/${LIB_NAME}/lib")
set(INSTALL_INCLUDE_DIR "${CMAKE_INSTALL_PREFIX}/doralib/${LIB_NAME}/include")
set(INSTALL_CMKAE_DIR "${CMAKE_INSTALL_PREFIX}/doralib/${LIB_NAME}/cmake")

###########
## build ##
###########

include_directories(${LIB_INC_DIR})

foreach(FOREACH_SRC_DIR ${LIB_SRC_DIR})
    aux_source_directory(${FOREACH_SRC_DIR} TMP_SRCS)
endforeach()

add_library(${LIB_NAME} ${LIB_TYPE} ${TMP_SRCS}) # SHARED STATIC

target_link_libraries(${LIB_NAME} ${CMAKE_CXX_FLAGS})

#############
## install ##
#############

install(TARGETS ${LIB_NAME}
  EXPORT ${LIB_NAME}-targets
  LIBRARY DESTINATION ${INSTALL_LIBDIR}     # SHARED
  ARCHIVE DESTINATION ${INSTALL_LIBDIR}     # STATIC
  PUBLIC_HEADER DESTINATION ${INSTALL_INCLUDE_DIR}
)

# Mark cpp header files for installation
install(DIRECTORY ${LIB_INC_DIR}
  DESTINATION ${INSTALL_INCLUDE_DIR}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

install(
  EXPORT ${LIB_NAME}-targets
  NAMESPACE ${LIB_NAME}::
  FILE ${LIB_NAME}-config.cmake
  DESTINATION lib/cmake/${LIB_NAME})

