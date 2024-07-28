cmake_minimum_required(VERSION 3.1...3.27)

#-------------------------------------------------------------------------------
# Build Environment
#-------------------------------------------------------------------------------

if (CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
  add_definitions(-fpic -W -DMG_ENABLE_LINES -DMG_ENABLE_OPENSSL)
  add_compile_options(-Werror -Wall )
endif()

project(
  mongoose
  VERSION 7.1.4
  LANGUAGES C
  )

# Add OpenSSL include directories
include_directories(${OPENSSL_INCLUDE_DIR})

set(OS_LIBS OpenSSL::SSL OpenSSL::Crypto Threads::Threads)

if(${CMAKE_SYSTEM_NAME} MATCHES "SunOS")
  list(APPEND OS_LIBS socket)
endif()

##set(CMAKE_SHARED_LIBRARY_PREFIX "")
add_library(mongoose SHARED mongoose.c)
target_link_libraries(mongoose libssl.so)

set_target_properties(mongoose PROPERTIES PUBLIC_HEADER "mongoose.h")
INSTALL(TARGETS mongoose 
        LIBRARY DESTINATION /usr/lib/
        PUBLIC_HEADER DESTINATION /usr/include/libmongoose/
)
