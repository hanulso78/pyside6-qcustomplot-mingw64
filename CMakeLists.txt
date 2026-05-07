cmake_minimum_required(VERSION 3.18)
project(PySide6QCustomPlot)

find_package(Python3 REQUIRED COMPONENTS Interpreter Development)
find_package(Qt6 REQUIRED COMPONENTS Core Widgets PrintSupport)
find_package(Shiboken6 QUIET)
find_package(PySide6 QUIET)

execute_process(
  COMMAND "${Python3_EXECUTABLE}" -c "import sysconfig; print(sysconfig.get_paths()['purelib'])"
  OUTPUT_VARIABLE SITE_PACKAGES_PATH
  OUTPUT_STRIP_TRAILING_WHITESPACE
)

message(STATUS "Found Python3:
	Executable: ${Python3_EXECUTABLE}
	Version: ${Python3_VERSION}
	site-packages Path: ${SITE_PACKAGES_PATH}")

execute_process(
  COMMAND cygpath.exe -w "$ENV{MINGW_PREFIX}"
  OUTPUT_VARIABLE MINGW_PREFIX
  OUTPUT_STRIP_TRAILING_WHITESPACE
  COMMAND_ERROR_IS_FATAL ANY
)

message(STATUS "MINGW_PREFIX: ${MINGW_PREFIX}")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_PREFIX_PATH ${MINGW_PREFIX})
set(CMAKE_BUILD_TYPE Release)

qt_standard_project_setup()

set(SHIBOKEN_OUTPUT_FOLDER "${CMAKE_CURRENT_BINARY_DIR}/OUTPUTDIR")

# Do not use a GLOB here, to allow for usage as OUTPUT in custom command
set(SHIBOKEN_GENERATED_SOURCES
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/pyside6qcustomplot_module_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/pyside6qcustomplot_python.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcp_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcp_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpabstractplottable_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpabstractplottable_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxis_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxis_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxisrect_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxisrect_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxisticker_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxisticker_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpdataselection_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpdataselection_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpgraph_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpgraph_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpgraphdata_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpgraphdata_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpgrid_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpgrid_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayer_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayer_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayerable_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayerable_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayout_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayout_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayoutelement_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayoutelement_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayoutgrid_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayoutgrid_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplegend_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplegend_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplineending_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplineending_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcppainter_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcppainter_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpplottableinterface1d_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpplottableinterface1d_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcprange_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcprange_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpscatterstyle_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpscatterstyle_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcustomplot_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcustomplot_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpselectiondecorator_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpselectiondecorator_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickerlog_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickerlog_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpdatarange_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpdatarange_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpgraphdatacontainer_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpgraphdatacontainer_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpcurvedatacontainer_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpcurvedatacontainer_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpvector2d_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpvector2d_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpabstractitem_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpabstractitem_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemtracer_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemtracer_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemposition_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemposition_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemanchor_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemanchor_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxispainterprivate_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxispainterprivate_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpselectionrect_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpselectionrect_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpcurve_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpcurve_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpcurvedata_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpcurvedata_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemtext_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemtext_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemstraightline_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpitemstraightline_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickerdatetime_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickerdatetime_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpabstractlegenditem_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpabstractlegenditem_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpplottablelegenditem_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpplottablelegenditem_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qsharedpointer_qcpaxisticker_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qsharedpointer_qcpaxisticker_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qsharedpointer_qcpgraphdatacontainer_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qsharedpointer_qcpgraphdatacontainer_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qsharedpointer_qcpcurvedatacontainer_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qsharedpointer_qcpcurvedatacontainer_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/keyvalues_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/keyvalues_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayoutinset_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcplayoutinset_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickertime_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickertime_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickertext_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickertext_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickerpi_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickerpi_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickerfixed_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpaxistickerfixed_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpabstractpaintbuffer_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpabstractpaintbuffer_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcppaintbufferpixmap_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcppaintbufferpixmap_wrapper.h"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpmargingroup_wrapper.cpp"
	"${SHIBOKEN_OUTPUT_FOLDER}/PySide6QCustomPlot/qcpmargingroup_wrapper.h"
)

add_custom_command(
    COMMAND shiboken6
        --generator-set=shiboken
        --output-directory=OUTPUTDIR
		-I${MINGW_PREFIX}/include/qt6 -I${MINGW_PREFIX}/include/qt6/QtCore
		--typesystem-paths=${MINGW_PREFIX}/share/PySide6/typesystems
        --enable-pyside-extensions
        --avoid-protected-hack
        ${CMAKE_CURRENT_SOURCE_DIR}/src/wrapper.hpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/bindings.xml
    DEPENDS src/bindings.xml src/wrapper.hpp
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    OUTPUT ${SHIBOKEN_GENERATED_SOURCES}
    COMMENT "Running shiboken6 generator for src/bindings.xml"
)

add_custom_target(run_shiboken
    DEPENDS ${SHIBOKEN_GENERATED_SOURCES}
)

add_library(
	PySide6QCustomPlot
    SHARED
    src/wrapper.hpp
    src/qcustomplot.cpp
    src/qcustomplot.h
    ${SHIBOKEN_GENERATED_SOURCES}
)

# A bit ugly:
# - include paths to python installation
# - include paths to pyside and shiboken6_generator in .venv
target_include_directories(
	PySide6QCustomPlot
    PRIVATE
    "src"
	"${MINGW_PREFIX}/include"
	${Python3_INCLUDE_DIRS}
	"${MINGW_PREFIX}/include/PySide6"
	"${MINGW_PREFIX}/include/PySide6/QtWidgets"
	"${MINGW_PREFIX}/include/PySide6/QtGui"
	"${MINGW_PREFIX}/include/PySide6/QtCore"
	"${MINGW_PREFIX}/include/PySide6/QtPrintSupport"
	"${MINGW_PREFIX}/include/shiboken6"
)

# Use the python stable ABI3:
# See also https://docs.python.org/3/c-api/stable.html
target_compile_definitions(
	PySide6QCustomPlot
    PRIVATE
)

set_target_properties(
	PySide6QCustomPlot
    PROPERTIES
    SUFFIX ".pyd"
	PREFIX ""
)

target_link_directories(
	PySide6QCustomPlot
    PRIVATE
	"${MINGW_PREFIX}/lib"
	${SITE_PACKAGES_PATH}/shiboken6
	${SITE_PACKAGES_PATH}/PySide6
)

target_link_libraries(
	PySide6QCustomPlot
    PRIVATE
    Qt6::Widgets
    Qt6::Core
    Qt6::PrintSupport
	Python3::Python
)

if(TARGET Shiboken6::libshiboken)
	target_link_libraries(PySide6QCustomPlot PRIVATE Shiboken6::libshiboken)
endif()

if(TARGET PySide6::pyside6)
	target_link_libraries(PySide6QCustomPlot PRIVATE PySide6::pyside6)
endif()

install(
	TARGETS PySide6QCustomPlot
    DESTINATION ${PY_BUILD_CMAKE_MODULE_NAME}
)
