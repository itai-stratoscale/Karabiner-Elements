cmake_minimum_required (VERSION 3.9)

include (../../common.cmake)

project (Karabiner-Elements)

add_compile_options(
  -Wall
  -Werror
  -O2
  -fmodules
  -fobjc-arc
  -F${CMAKE_CURRENT_LIST_DIR}/../../vendor/Sparkle
)

include_directories(
  /opt/local/include
  /usr/local/include
  ../../share
  ../../vendor
  ../../lib/libkrbn/include
  ../lib/KarabinerKit/include
  ../share
)

link_directories(
  ../../lib/libkrbn/build_xcode/build
  ../lib/KarabinerKit/build_xcode/build
)

add_executable(
  Karabiner-Elements
  MACOSX_BUNDLE
  src/AppDelegate.m
  src/main.m
  Resources/MainMenu.xib
  Resources/karabiner_elements_pub.pem
  ../share/Resources/app.icns
  ../../vendor/Sparkle/Sparkle.framework
)

set_source_files_properties(
  Resources/MainMenu.xib
  Resources/karabiner_elements_pub.pem
  ../share/Resources/app.icns
  PROPERTIES
  MACOSX_PACKAGE_LOCATION
  Resources
)

set_source_files_properties(
  ../../vendor/Sparkle/Sparkle.framework
  PROPERTIES
  MACOSX_PACKAGE_LOCATION
  Frameworks
)

set_target_properties(
  Karabiner-Elements
  PROPERTIES
  MACOSX_BUNDLE_INFO_PLIST
  ${CMAKE_CURRENT_LIST_DIR}/Resources/plist.in
)

set_target_properties(
  Karabiner-Elements
  PROPERTIES
  XCODE_ATTRIBUTE_LD_RUNPATH_SEARCH_PATHS
  "@loader_path @loader_path/../Frameworks"
)

target_link_libraries(
  Karabiner-Elements
  KarabinerKit
  krbn
  "-framework SystemConfiguration"
  -F${CMAKE_CURRENT_LIST_DIR}/../../vendor/Sparkle
  "-framework Sparkle"
  c++
)
