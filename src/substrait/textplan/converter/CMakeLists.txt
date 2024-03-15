# SPDX-License-Identifier: Apache-2.0

include(../../../../third_party/datetime.cmake)

set(TEXTPLAN_HDRS InitialPlanProtoVisitor.h PipelineVisitor.h LoadBinary.h
                  SaveBinary.h ParseBinary.h)

set(TEXTPLAN_SRCS InitialPlanProtoVisitor.cpp PipelineVisitor.cpp
                  LoadBinary.cpp SaveBinary.cpp ParseBinary.cpp)

add_library(substrait_base_proto_visitor BasePlanProtoVisitor.cpp
                                         BasePlanProtoVisitor.h)

target_link_libraries(
  substrait_base_proto_visitor
  substrait_common
  substrait_proto
  error_listener
  fmt::fmt-header-only
  absl::status
  absl::statusor)

add_library(substrait_textplan_converter ${TEXTPLAN_SRCS} ${TEXTPLAN_HDRS})

target_link_libraries(
  substrait_textplan_converter
  substrait_base_proto_visitor
  substrait_common
  substrait_proto
  symbol_table
  error_listener
  fmt::fmt-header-only
  absl::status
  absl::statusor)

if(${SUBSTRAIT_CPP_BUILD_TESTING})
  add_subdirectory(tests)
endif()

add_executable(planconverter Tool.cpp)

target_link_libraries(planconverter substrait_textplan_converter substrait_io)

set(NORMALIZER_SRCS ReferenceNormalizer.cpp ReferenceNormalizer.h)

add_library(substrait_textplan_normalizer ${NORMALIZER_SRCS})

target_link_libraries(substrait_textplan_normalizer
                      substrait_textplan_converter)

install(
  TARGETS substrait_textplan_converter substrait_base_proto_visitor
          substrait_textplan_normalizer
  DESTINATION ${CMAKE_INSTALL_LIBDIR}
  EXPORT SubstraintTexplanConverterTargets)
install(FILES ${TEXTPLAN_HDRS}
        DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/substrait/textplan/converter")
install(EXPORT SubstraintTexplanConverterTargets
        DESTINATION lib/cmake/Substrait)
