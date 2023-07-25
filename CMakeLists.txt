cmake_minimum_required(VERSION 3.5)

# 设置VCPKG_ROOT所在的位置
set(VCPKG_ROOT "E:/vcpkg" CACHE PATH "")
set(CMAKE_TOOLCHAIN_FILE "${VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake")

project(paraFlow)

find_package(VTK COMPONENTS
  CommonColor
  CommonCore
  CommonDataModel
  FiltersExtraction
  FiltersSources
  InteractionStyle
  RenderingContextOpenGL2
  RenderingCore
  RenderingFreeType
  RenderingGL2PSOpenGL2
  RenderingOpenGL2
)

if (NOT VTK_FOUND)
  message(FATAL_ERROR "ExtractSelectionCells: Unable to find the VTK build folder.")
endif()

# Prevent a "command line is too long" failure in Windows.
set(CMAKE_NINJA_FORCE_RESPONSE_FILE "ON" CACHE BOOL "Force Ninja to use response files.")
add_executable(ExtractSelectionCells MACOSX_BUNDLE main.cpp )
  target_link_libraries(ExtractSelectionCells PRIVATE ${VTK_LIBRARIES}
)
# vtk_module_autoinit is needed
vtk_module_autoinit(
  TARGETS ExtractSelectionCells
  MODULES ${VTK_LIBRARIES}
)
