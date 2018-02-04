# Copyright (c) 2012-2018, Association Scientifique pour la Geologie et ses
# Applications (ASGA). All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of ASGA nor the
#       names of its contributors may be used to endorse or promote products
#       derived from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO,
# THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
# PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL ASGA BE LIABLE FOR ANY DIRECT,
# INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
# ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF
# THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
#
#     http://www.ring-team.org
#
#     RING Project
#     Ecole Nationale Superieure de Geologie - GeoRessources
#     2 Rue du Doyen Marcel Roubault - TSA 70605
#     54518 VANDOEUVRE-LES-NANCY
#     FRANCE

# Configure project library
add_library(ringmesh_paraview SHARED "")
set_target_properties(ringmesh_paraview PROPERTIES OUTPUT_NAME RINGMesh_ParaView FOLDER "Libraries")
target_include_directories(ringmesh_paraview
    PUBLIC   
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<BUILD_INTERFACE:${PROJECT_BINARY_DIR}>
        $<INSTALL_INTERFACE:include>
)

set(ringmesh_paraview_include_dir ${PROJECT_SOURCE_DIR}/include/ringmesh_paraview)
set(ringmesh_paraview_source_dir ${PROJECT_SOURCE_DIR}/src/ringmesh_paraview)
target_sources(ringmesh_paraview
    PRIVATE
        "${ringmesh_paraview_source_dir}/common.cpp"
        "${ringmesh_paraview_source_dir}/hello_world.cpp"
        "${ringmesh_paraview_source_dir}/vtkGeoModelReader.cpp"
    PRIVATE # Could be PUBLIC from CMake 3.3
        "${ringmesh_paraview_include_dir}/common.h"
        "${ringmesh_paraview_include_dir}/hello_world.h"
        "${ringmesh_paraview_include_dir}/vtkGeoModelReader.h"
)

target_link_libraries(ringmesh_paraview 
    PUBLIC 
        RINGMesh::basic
        RINGMesh::geomodel_core
        RINGMesh::io
)

export(TARGETS ringmesh_paraview 
    NAMESPACE RINGMeshParaView:: 
    FILE cmake/RINGMesh_paraview_target.cmake
)
generate_export_header(ringmesh_paraview 
    EXPORT_MACRO_NAME ringmesh_paraview_api 
    EXPORT_FILE_NAME ${PROJECT_BINARY_DIR}/ringmesh_paraview/export.h
)
install(TARGETS ringmesh_paraview
    EXPORT ringmesh_paraview
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
install(EXPORT ringmesh_paraview
    FILE RINGMesh_paraview_target.cmake
    NAMESPACE RINGMeshParaView::
    DESTINATION cmake
)
