# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)


#FFMpeg配置
#FFmpeg配置目录
set(distribution_DIR ${CMAKE_SOURCE_DIR}/../../../../src/main/jniLibs)

# 编解码(最重要的库)
add_library(
            avcodec
            STATIC
            IMPORTED)
set_target_properties(
            avcodec
            PROPERTIES IMPORTED_LOCATION
            ../../../../src/main/jniLibs/armeabi/libavcodec.a)


# 滤镜特效处理库
add_library(
            avfilter
            STATIC
            IMPORTED)
set_target_properties(
            avfilter
            PROPERTIES IMPORTED_LOCATION
            ../../../../src/main/jniLibs/armeabi/libavfilter.a)

# 封装格式处理库
add_library(
            avformat
            STATIC
            IMPORTED)
set_target_properties(
            avformat
            PROPERTIES IMPORTED_LOCATION
            ../../../../src/main/jniLibs/armeabi/libavformat.a)

# 工具库(大部分库都需要这个库的支持)
add_library(
            avutil
            STATIC
            IMPORTED)
set_target_properties(
            avutil
            PROPERTIES IMPORTED_LOCATION
            ../../../../src/main/jniLibs/armeabi/libavutil.a)

add_library(
            postproc
            STATIC
            IMPORTED)
set_target_properties(
            postproc
            PROPERTIES IMPORTED_LOCATION
            ../../../../src/main/jniLibs/armeabi/libpostproc.a)

# 音频采样数据格式转换库
add_library(
            swresample
            STATIC
            IMPORTED)
set_target_properties(
            swresample
            PROPERTIES IMPORTED_LOCATION
            ../../../../src/main/jniLibs/armeabi/libswresample.a)

# 视频像素数据格式转换
add_library(
            swscale
            STATIC
            IMPORTED)
set_target_properties(
            swscale
            PROPERTIES IMPORTED_LOCATION
            ../../../../src/main/jniLibs/armeabi/libswscale.a)

add_library(
            x264
            STATIC
            IMPORTED)
set_target_properties(
            x264
            PROPERTIES IMPORTED_LOCATION
            ../../../../src/main/jniLibs/armeabi/libx264.a)

#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")
#判断编译器类型,如果是gcc编译器,则在编译选项中加入c++11支持
if(CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS "-std=c++11 ${CMAKE_CXX_FLAGS}")
    message(STATUS "optional:-std=c++11")
endif(CMAKE_COMPILER_IS_GNUCXX)

#配置编译的头文件
include_directories(src/main/jniLibs/include)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.
#设置生成的so动态库最后输出的路径
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/../jniLibs/${ANDROID_ABI})

add_library( # Sets the name of the library.
             live

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/live.cpp )

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
                       live avcodec avfilter avformat avutil postproc swresample swscale x264

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )