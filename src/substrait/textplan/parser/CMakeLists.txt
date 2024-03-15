# SPDX-License-Identifier: Apache-2.0

include(../../../../third_party/datetime.cmake)

add_subdirectory(grammar)

set(TEXTPLAN_LOADER_HDRS
    SubstraitPlanVisitor.h
    SubstraitPlanPipelineVisitor.h
    SubstraitPlanRelationVisitor.h
    SubstraitPlanSubqueryRelationVisitor.h
    SubstraitPlanTypeVisitor.h
    LoadText.h
    ParseText.h)

set(TEXTPLAN_LOADER_SRCS
    SubstraitPlanVisitor.cpp
    SubstraitPlanPipelineVisitor.cpp
    SubstraitPlanRelationVisitor.cpp
    SubstraitPlanSubqueryRelationVisitor.cpp
    SubstraitPlanTypeVisitor.cpp
    LoadText.cpp
    ParseText.cpp
    SubstraitParserErrorListener.cpp)

add_library(substrait_textplan_loader ${TEXTPLAN_LOADER_HDRS}
                                      ${TEXTPLAN_LOADER_SRCS})

target_link_libraries(
  substrait_textplan_loader
  symbol_table
  error_listener
  substrait_proto
  substrait_type
  substrait_expression
  textplan_grammar
  fmt::fmt-header-only
  date::date
  date::date-tz
  absl::status
  absl::statusor)

add_executable(planparser Tool.cpp)

target_link_libraries(planparser substrait_textplan_loader error_listener)

if(${SUBSTRAIT_CPP_BUILD_TESTING})
  add_subdirectory(tests)
endif()

install(
  TARGETS substrait_textplan_loader
  DESTINATION ${CMAKE_INSTALL_LIBDIR}
  EXPORT SubstraitTextplanParserTargets)
install(FILES ${TEXTPLAN_LOADER_HDRS}
        DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/substrait/textplan/parser")
install(EXPORT SubstraitTextplanParserTargets DESTINATION lib/cmake/Substrait)
