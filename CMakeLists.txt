cmake_minimum_required(VERSION 3.23)
project(cmakemetal)

# Apple에서 제공하는 5개의 Library가 필요합니다
find_library(AppKit AppKit)
find_library(Metal Metal)
find_library(MetalKit MetalKit)
find_library(QuartzCore QuartzCore)
find_library(Foundation Foundation)

set(CUR_TARGET cmakemetal)

# macos에서 동작하기 위해선 Bundle의 형태로 빌드해야 합니다. 이때 MACOSX_BUNDLE 키워드를 이용할 수 있습니다.
add_executable(${CUR_TARGET} MACOSX_BUNDLE
        ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/renderer.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/view_delegate.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/app_delegate.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/test.cpp
        )

# Bundle의 properties를 설정합니다.
set_target_properties(${CUR_TARGET} PROPERTIES
        BUNDLE True
        MACOSX_BUNDLE_GUI_IDENTIFIER my.domain.style.identifier.${CUR_TARGET}
        MACOSX_BUNDLE_NAME ${CUR_TARGET}
        MACOSX_BUNDLE_BUNDLE_VERSION "0.1"
        MACOSX_BUNDLE_SORT_VERSION_STRING "0.1"
        MACOSX_BUNDLE_INFO_PLIST ${CMAKE_SOURCE_DIR}/MacOSXBundleInfo.plist.in
        )

target_include_directories(${CUR_TARGET} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${CMAKE_CURRENT_SOURCE_DIR}/dependencies/metal-cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/dependencies/metal-cpp-extensions
        )

# metal-cpp은 c++ 17로 작성되었습니다. 따라서 현재 빌드가 c++17을 사용하도록 명시해야 합니다.
target_compile_features(${CUR_TARGET} PRIVATE cxx_std_17)

target_link_libraries(
        ${CUR_TARGET}
        ${AppKit}
        ${Foundation}
        ${QuartzCore}
        ${Metal}
        ${MetalKit}
        )

