# SPDX-License-Identifier: Apache-2.0

add_subdirectory(converter)
add_subdirectory(parser)

set(SYMBOL_TABLE_HDRS
    Location.h
    PlanPrinterVisitor.h
    StringManipulation.h
    SymbolTable.h
    SymbolTablePrinter.h
    StructuredSymbolData.h
    Any.h)

set(SYMBOL_TABLE_SRCS
    Location.cpp PlanPrinterVisitor.cpp StringManipulation.cpp SymbolTable.cpp
    SymbolTablePrinter.cpp)

add_library(symbol_table ${SYMBOL_TABLE_HDRS} ${SYMBOL_TABLE_SRCS})

add_library(error_listener SubstraitErrorListener.cpp SubstraitErrorListener.h)

add_library(parse_result ParseResult.cpp ParseResult.h)

add_dependencies(symbol_table substrait_proto substrait_common absl::strings
                 fmt::fmt-header-only)

target_link_libraries(
  symbol_table
  substrait_base_proto_visitor
  substrait_proto
  substrait_common
  substrait_expression
  absl::strings
  fmt::fmt-header-only
  date::date)

# Provide access to the generated protobuffer headers hierarchy.
target_include_directories(
  symbol_table PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/../..>
                      $<INSTALL_INTERFACE:include>)

if(${SUBSTRAIT_CPP_BUILD_TESTING})
  add_subdirectory(tests)
endif()

install(
  TARGETS symbol_table error_listener parse_result
  DESTINATION lib
  EXPORT SubstraitTextplanTargets)
install(FILES ${SYMBOL_TABLE_HDRS} DESTINATION include/substrait/textplan)
install(EXPORT SubstraitTextplanTargets DESTINATION lib/cmake/Substrait)
