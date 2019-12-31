cmake_minimum_required(VERSION 3.5)
project(OpenRoadEd)

add_compile_options(-fPIC)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BUILD_TYPE Debug)
find_package(Qt5Core REQUIRED)
find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)
find_package(Qt5OpenGL REQUIRED)
find_package(OpenGL REQUIRED)
FIND_PACKAGE(OpenThreads)
FIND_PACKAGE(osg)
FIND_PACKAGE(osgDB)
FIND_PACKAGE(osgUtil)
FIND_PACKAGE(osgGA)
FIND_PACKAGE(osgViewer)

include_directories(.)

include_directories(${EIGEN3_INCLUDE_DIR})
include_directories(${PCL_INCLUDE_DIRS})


set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)


file(GLOB_RECURSE SRCS_QT ./src/Qt/*.cpp ./src/Qt/*.h)
file(GLOB_RECURSE SRCS_OSG ./src/Osg/*.cpp ./src/Osg/*.h)
file(GLOB_RECURSE SRCS_OPENDRIVE ./src/OpenDrive/*.cpp ./src/OpenDrive/*.h)
file(GLOB_RECURSE SRCS_CEPHES ./src/Cephes/*.cpp ./src/Cephes/*.h ./src/Cephes/*.c)
file(GLOB_RECURSE SRCS_XML ./src/TinyXML/*.cpp ./src/TinyXML/*.h)


add_library(xml
        ${SRCS_XML}
        )

add_library(qtgui SHARED
        ${SRCS_QT}
        ./res.qrc
        )
target_link_libraries(
        qtgui
        ${Qt5Gui_LIBRARIES}
        ${Qt5Widgets_LIBRARIES}
)
add_library(opendrive
        ${SRCS_OPENDRIVE}
        )


include_directories(${Qt5Core_INCLUDE_DIRS})
message("${Qt5Core_INCLUDE_DIRS}")
add_library(
        osglib
        ${SRCS_OSG}
        )

add_library(
        Cephes
        ${SRCS_CEPHES}
)

add_executable(
        app
        ./src/main.cpp
)
target_link_libraries(
        app
        qtgui
        opendrive
        osglib
        Cephes
        osg
        osgGA
        osgDB
        osgUtil
        osgViewer
        xml
        /usr/local/lib64/libosg.so.158
)

