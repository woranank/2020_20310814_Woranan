cmake_minimum_required(VERSION 3.3 FATAL_ERROR)

project(Hexahedron)

find_package(VTK COMPONENTS 
  vtkCommonColor
  vtkCommonCore
  vtkCommonDataModel
  vtkInteractionStyle
  vtkRenderingCore
  vtkRenderingFreeType
  vtkRenderingOpenGL2 QUIET)
if (NOT VTK_FOUND)
  message("Skipping Hexahedron: ${VTK_NOT_FOUND_MESSAGE}")
  return ()
endif()
message (STATUS "VTK_VERSION: ${VTK_VERSION}")
if (VTK_VERSION VERSION_LESS "8.90.0")
  # old system
  include(${VTK_USE_FILE})
  add_executable(Hexahedron MACOSX_BUNDLE Hexahedron.cpp )
  target_link_libraries(Hexahedron PRIVATE ${VTK_LIBRARIES})
else ()
  # include all components
  add_executable(Hexahedron MACOSX_BUNDLE Hexahedron.cpp )
  target_link_libraries(Hexahedron PRIVATE ${VTK_LIBRARIES})
  # vtk_module_autoinit is needed
  vtk_module_autoinit(
    TARGETS Hexahedron
    MODULES ${VTK_LIBRARIES}
    )
endif ()