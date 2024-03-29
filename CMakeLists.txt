##############################################################################
# CMake
##############################################################################

cmake_minimum_required(VERSION 2.8.0)
project(qtros)

##############################################################################
# Catkin
##############################################################################

# qt_build provides the qt cmake glue, roscpp the comms for a default talker
find_package(catkin REQUIRED COMPONENTS roscpp)
find_package(Qt5Widgets REQUIRED)

include_directories(
        include
        ${catkin_INCLUDE_DIRS})

set(CPP_SOURCES src/main.cpp src/main_window.cpp src/qnode.cpp)
set(CPP_HEADERS include/qtros/main_window.hpp include/qtros/qnode.hpp)
set(QRC_RESOURCES resources/images.qrc)

set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

#add_definitions(${Qt5Widgets_DEFINITIONS})
# Use this to define what the package will export (e.g. libs, headers).
# Since the default here is to produce only a binary, we don't worry about
# exporting anything. 
catkin_package()

include_directories(${Qt5Widgets_INCLUDE_DIRS})
add_definitions(${Qt5Widgets_DEFINITIONS})

qt5_wrap_ui(UIS_HEADERS ui/main_window.ui)

qt5_wrap_cpp(MOC_SOURCES ${CPP_SOURCES} ${CPP_HEADERS})

qt5_add_resources(RESOURCES_RCC ${QRC_RESOURCES})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${Qt5Widgets_EXECUTABLE_COMPILE_FLAGS} -std=c++11")

##############################################################################
# Qt Environment
##############################################################################

# this comes from qt_build's qt-ros.cmake which is automatically 
# included via the dependency call in package.xml

##############################################################################
# Sections
##############################################################################

#file(GLOB QT_FORMS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ui/*.ui)
#file(GLOB QT_RESOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} resources/*.qrc)
#file(GLOB_RECURSE QT_MOC RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} FOLLOW_SYMLINKS include/qtros/*.hpp)
#
##QT4_ADD_RESOURCES(QT_RESOURCES_CPP ${QT_RESOURCES})
##QT4_WRAP_UI(QT_FORMS_HPP ${QT_FORMS})
##QT4_WRAP_CPP(QT_MOC_HPP ${QT_MOC})
#
##qt5_wrap_cpp(QT_MOC_HPP include/qtros/qnode.hpp)
#qt5_wrap_cpp(QT_MOC_HPP ${QT_MOC})

##############################################################################
# Sources
##############################################################################

#file(GLOB_RECURSE QT_SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} FOLLOW_SYMLINKS src/*.cpp)

##############################################################################
# Binaries
##############################################################################


#add_executable(qtros ${QT_SOURCES} ${QT_RESOURCES_CPP} ${QT_FORMS_HPP} ${QT_MOC_HPP})
#qt5_use_modules(qtros Widgets)
#target_link_libraries(qtros ${QT_LIBRARIES} Qt5::Widgets ${catkin_LIBRARIES})
#install(TARGETS qtros RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

add_executable(qtros ${CPP_SOURCES} ${UIS_HEADERS} ${MOC_SOURCES} ${QRC_RESOURCES} ${RESOURCES_RCC})
target_link_libraries(qtros ${Qt5Widgets_LIBRARIES} ${catkin_LIBRARIES})

qt5_use_modules(qtros Widgets)
