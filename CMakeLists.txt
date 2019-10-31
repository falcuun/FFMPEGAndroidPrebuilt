cmake_minimum_required(VERSION 3.10.1)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")

include_directories(.)
include_directories(${CMAKE_SOURCE_DIR}/libs/${ANDROID_ABI}/include)
include_directories(${CMAKE_SOURCE_DIR}/libs/${ANDROID_ABI})

add_library(avcodec SHARED IMPORTED)
set_target_properties(avcodec
        PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libs/${ANDROID_ABI}/libavcodec.so)

add_library(avutil SHARED IMPORTED)
set_target_properties(avutil
        PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libs/${ANDROID_ABI}/libavutil.so)

add_library(avformat SHARED IMPORTED)
set_target_properties(avformat
        PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libs/${ANDROID_ABI}/libavformat.so)

add_library(swresample SHARED IMPORTED)
set_target_properties(swresample
        PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libs/${ANDROID_ABI}/libswresample.so)

add_library(swscale SHARED IMPORTED)
set_target_properties(swscale
        PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libs/${ANDROID_ABI}/libswscale.so)

link_directories(${CMAKE_SOURCE_DIR}/libs/${ANDROID_ABI}/include)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -Wno-potentially-evaluated-expression")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D__ANDROID__ -DANDROID")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DGL_GLEXT_PROTOTYPES=1 -DIOAPI_NO_64 -DUSE_FILE32API ")

add_library(nativelib SHARED ${TARGET_SRC})

find_library(
        log-lib

        log)

target_link_libraries(nativelib 
        ${log-lib}
        avcodec
        avutil
        avformat
        swresample
        swscale
        )
