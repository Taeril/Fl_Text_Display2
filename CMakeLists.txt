cmake_minimum_required(VERSION 3.2.3)
project(Fl_Text_Display2)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${PROJECT_SOURCE_DIR}/cmake")
include(SetupCompiler)

find_package(FLTK REQUIRED NO_MODULE)

# If FLTK is not installed get path to sources.
# Otherwise -DFLTK_SOURCE_DIRECTORY=/path/to/fltk must be specified manually
if(NOT FLTK_SOURCE_DIRECTORY)
	foreach(_dir IN LISTS FLTK_INCLUDE_DIRS)
		if(EXISTS "${_dir}/README.CMake.txt" AND EXISTS "${_dir}/fltk.xpm")
			set(FLTK_SOURCE_DIRECTORY "${_dir}")
		endif()
	endforeach()
endif()
if(NOT FLTK_SOURCE_DIRECTORY)
	message(FATAL_ERROR "FLTK_SOURCE_DIRECTORY unset - need to specify path to FLTK sources")
endif()
#message(STATUS "FLTK: ${FLTK_SOURCE_DIRECTORY}")


add_library(Fl_Text_Display2
	src/Fl_Text_Display2.cxx
	include/FL/Fl_Text_Display2.H
	src/Fl_Text_Editor2.cxx
	include/FL/Fl_Text_Editor2.H
)

target_include_directories(Fl_Text_Display2 
	PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include" ${FLTK_INCLUDE_DIRS}
	PRIVATE "${FLTK_SOURCE_DIRECTORY}/src"
)
target_link_libraries(Fl_Text_Display2 PUBLIC fltk)

add_executable(editor2 src/editor2.cxx)

target_link_libraries(editor2 PRIVATE Fl_Text_Display2)

