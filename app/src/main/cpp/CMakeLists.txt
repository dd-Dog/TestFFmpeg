# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

#lib库工程名，并非so生成的so库名称，可以不写
project(TestFFmpeg)

# Sets the minimum version of CMake required to build the native library.
cmake_minimum_required(VERSION 3.4.1)

#包含指定目录的头文件
include_directories(include/)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

#添加库文件 参数：生成的库名称  动态库(即so库) 源文件
add_library(native-lib SHARED native-lib.cpp)

#创建导入的库目标,FFmpeg7个SO库
add_library(avutil-55 SHARED IMPORTED)
add_library(swresample-2 SHARED IMPORTED)
add_library(avcodec-57 SHARED IMPORTED)
add_library(avfilter-6 SHARED IMPORTED)
add_library(swscale-4 SHARED IMPORTED)
add_library(avdevice-57 SHARED IMPORTED)
add_library(avformat-57 SHARED IMPORTED)

#设置变量,CMAKE_SOURCE_DIR是CMakeList.txt的路径
set(LIB_DIR ${CMAKE_SOURCE_DIR}/../../../libs/${ANDROID_ABI})

#message(SEND_ERROR  "jni libs dir=${LIB_DIR}")

#设置目标属性，例如avutil-55的IMPORTED_LOCATION属性值是${LIB_DIR}/libavutil-55.so
set_target_properties(avutil-55 PROPERTIES IMPORTED_LOCATION ${LIB_DIR}/libavutil-55.so)
set_target_properties(swresample-2 PROPERTIES IMPORTED_LOCATION ${LIB_DIR}/libswresample-2.so)
set_target_properties(avcodec-57 PROPERTIES IMPORTED_LOCATION ${LIB_DIR}/libavcodec-57.so)
set_target_properties(avfilter-6 PROPERTIES IMPORTED_LOCATION ${LIB_DIR}/libavfilter-6.so)
set_target_properties(swscale-4 PROPERTIES IMPORTED_LOCATION ${LIB_DIR}/libswscale-4.so)
set_target_properties(avdevice-57 PROPERTIES IMPORTED_LOCATION ${LIB_DIR}/libavdevice-57.so )
set_target_properties(avformat-57 PROPERTIES IMPORTED_LOCATION ${LIB_DIR}/libavformat-57.so)


# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

#在指定目录下搜索一个库, 保存在变量中
find_library(
        # 变量名
        log-lib

        #搜索log库
        log)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

#目标文件与库文件进行链接
target_link_libraries(
        # Specifies the target library.
        native-lib

        # Links the target library to the log library
        # included in the NDK.
        avutil-55
        swresample-2
        avcodec-57
        avfilter-6
        swscale-4
        avdevice-57
        avformat-57
        ${log-lib})