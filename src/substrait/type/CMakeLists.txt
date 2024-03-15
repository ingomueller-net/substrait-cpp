# SPDX-License-Identifier: Apache-2.0

set(TYPE_SRCS Type.cpp)

add_library(substrait_type ${TYPE_SRCS})

target_link_libraries(substrait_type substrait_common)

if(${SUBSTRAIT_CPP_BUILD_TESTING})
  add_subdirectory(tests)
endif()

install(
  TARGETS substrait_type
  DESTINATION ${CMAKE_INSTALL_LIBDIR}
  EXPORT SubstraitTypeTargets)
install(
  FILES ${TEXTPLAN_LOADER_HDRS}
  DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/substrait/type")
install(EXPORT SubstraitTypeTargets
        DESTINATION lib/cmake/Substrait)
