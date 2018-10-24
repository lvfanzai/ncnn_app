# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

include_directories(${CMAKE_SOURCE_DIR}/src/main/cpp/include/)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

add_library( # Sets the name of the library.
        squeezencnn

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        src/main/cpp/squeezencnn_jni.cpp)

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -fvisibility=hidden -fomit-frame-pointer -fstrict-aliasing -ffunction-sections -fdata-sections -ffast-math -fopenmp")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -fvisibility=hidden -fvisibility-inlines-hidden -fomit-frame-pointer -fstrict-aliasing -ffunction-sections -fdata-sections -ffast-math -fopenmp")
set(CMAKE_LD_FLAGS "${CMAKE_LD_FLAGS} -Wl,--gc-sections -fopenmp")

add_library(ncnn STATIC IMPORTED )
set_target_properties(ncnn PROPERTIES
        IMPORTED_LOCATION "${CMAKE_SOURCE_DIR}/src/main/jnilibs/${ANDROID_ABI}/libncnn.a")


# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries(squeezencnn ncnn jnigraphics log)