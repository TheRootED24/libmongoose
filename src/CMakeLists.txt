cmake_minimum_required(VERSION 3.27)

#-------------------------------------------------------------------------------
# Build Environment
#-------------------------------------------------------------------------------

if (CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
  add_definitions(-fpic -W -DMG_ENABLE_LINES)
  add_compile_options(-W -Wall -Werror -Wextra -DMG_TLS=MG_TLS_BUILTIN)
endif()

project(
  mongoose
  VERSION 7.2.0
  LANGUAGES C
  )

add_library(mongoose SHARED mongoose.c)
set_target_properties(mongoose PROPERTIES VERSION ${PROJECT_VERSION} )
set_target_properties (mongoose PROPERTIES SOVERSION ${PROJECT_VERSION_MAJOR} )
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 -DMG_TLS=MG_TLS_BUILTIN")


set_target_properties(mongoose PROPERTIES PUBLIC_HEADER "mongoose.h")
INSTALL(TARGETS mongoose 
        LIBRARY DESTINATION /usr/lib/
        PUBLIC_HEADER DESTINATION /usr/include/libmongoose/
)
