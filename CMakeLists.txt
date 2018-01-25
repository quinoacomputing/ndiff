cmake_minimum_required(VERSION 2.4)

PROJECT(numdiff C)

if(NOT "${CMAKE_C_FLAGS}" MATCHES "std=c99")
   if(NOT (WIN32 AND CMAKE_C_COMPILER_ID STREQUAL "Intel") AND
      NOT CMAKE_C_COMPILER_ID STREQUAL "PGI" AND
      NOT CMAKE_C_COMPILER_ID STREQUAL "Cray")
      set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")
   else()
      set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /Qstd=c99")
   endif()
endif()

file(GLOB numdiff_src src/*.c)

include_directories(src)
add_executable(numdiff ${numdiff_src})
set(OUTPUT_NAME maddiff CACHE string "Output name for executable")
set_target_properties(numdiff PROPERTIES OUTPUT_NAME ${OUTPUT_NAME})
if(UNIX)
   target_link_libraries(numdiff m)
endif()

install(TARGETS numdiff
   RUNTIME
   DESTINATION bin
   COMPONENT Runtime
   )
