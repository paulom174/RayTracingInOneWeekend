#---------------------------------------------------------------------------------------------------
# CMake Build Configuration for the Ray Tracing Weekend Series
#
# See README.md for guidance.
#---------------------------------------------------------------------------------------------------

cmake_minimum_required ( VERSION 3.1.0...4.0.0 )

project ( RTWeekend LANGUAGES CXX )

# Set to C++11
set ( CMAKE_CXX_STANDARD          11 )
set ( CMAKE_CXX_STANDARD_REQUIRED ON )
set ( CMAKE_CXX_EXTENSIONS        OFF )

# Source

set ( EXTERNAL
  src/external/stb_image.h
)

set ( SOURCE_ONE_WEEKEND
  src/InOneWeekend/main.cc
  src/InOneWeekend/includes/camera.h
  src/InOneWeekend/includes/color.h
  src/InOneWeekend/includes/hittable.h
  src/InOneWeekend/includes/hittable_list.h
  src/InOneWeekend/includes/interval.h
  src/InOneWeekend/includes/material.h
  src/InOneWeekend/includes/ray.h
  src/InOneWeekend/includes/rtweekend.h
  src/InOneWeekend/includes/sphere.h
  src/InOneWeekend/includes/vec3.h
)

set ( SOURCE_NEXT_WEEK
  src/TheNextWeek/main.cc
  src/TheNextWeek/includes/aabb.h
  src/TheNextWeek/includes/bvh.h
  src/TheNextWeek/includes/camera.h
  src/TheNextWeek/includes/color.h
  src/TheNextWeek/includes/constant_medium.h
  src/TheNextWeek/includes/hittable.h
  src/TheNextWeek/includes/hittable_list.h
  src/TheNextWeek/includes/interval.h
  src/TheNextWeek/includes/material.h
  src/TheNextWeek/includes/perlin.h
  src/TheNextWeek/includes/quad.h
  src/TheNextWeek/includes/ray.h
  src/TheNextWeek/includes/rtweekend.h
  src/TheNextWeek/includes/texture.h
  src/TheNextWeek/includes/sphere.h
  src/TheNextWeek/includes/vec3.h
)


include_directories(src)

# Specific compiler flags below. We're not going to add options for all possible compilers, but if
# you're new to CMake (like we are), the following may be a helpful example if you're using a
# different compiler or want to set different compiler options.

message (STATUS "Compiler ID: " ${CMAKE_CXX_COMPILER_ID})
message (STATUS "Release flags: " ${CMAKE_CXX_FLAGS_RELEASE})
message (STATUS "Debug flags: " ${CMAKE_CXX_FLAGS_DEBUG})

if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    # /wd #### - Disable warning
    # /we #### - treat warning as error
    add_compile_options("/W4")      # Enable level-4 warnings
    add_compile_options("/we 4265") # Class has virtual functions, but its non-trivial destructor is not virtual
    add_compile_options("/we 5204") # Class has virtual functions, but its trivial destructor is not virtual
    add_compile_options("/wd 4100") # unreferenced formal parameter
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    add_compile_options(-Wnon-virtual-dtor) # Class has virtual functions, but its destructor is not virtual
    add_compile_options(-Wreorder) # Data member will be initialized after [other] data member
    add_compile_options(-Wmaybe-uninitialized) # Variable improperly initialized
    add_compile_options(-Wunused-variable) # Variable is defined but unused
elseif (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    add_compile_options(-Wnon-virtual-dtor) # Class has virtual functions, but its destructor is not virtual
    add_compile_options(-Wreorder) # Data member will be initialized after [other] data member
    add_compile_options(-Wsometimes-uninitialized) # Variable improperly initialized
    add_compile_options(-Wunused-variable) # Variable is defined but unused
endif()

# Executables
add_executable(inOneWeekend      ${SOURCE_ONE_WEEKEND})
add_executable(theNextWeek       ${EXTERNAL} ${SOURCE_NEXT_WEEK})
