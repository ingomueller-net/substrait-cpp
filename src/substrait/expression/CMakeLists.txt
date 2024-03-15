# SPDX-License-Identifier: Apache-2.0

add_library(substrait_expression DecimalLiteral.cpp)

target_link_libraries(substrait_expression substrait_proto absl::numeric
                      absl::strings)

if(${SUBSTRAIT_CPP_BUILD_TESTING})
  add_subdirectory(tests)
endif()

install(
  TARGETS substrait_expression
  DESTINATION ${CMAKE_INSTALL_LIBDIR}
  EXPORT SubstraitExpressionTargets)
install(
  FILES ${TEXTPLAN_LOADER_HDRS}
  DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/substrait/type")
install(EXPORT SubstraitExpressionTargets
        DESTINATION lib/cmake/Substrait)
