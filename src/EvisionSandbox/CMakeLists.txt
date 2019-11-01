#EvisionSandbox:主程序

#设置cmake最小版本号
cmake_minimum_required(VERSION 3.13.0)
 
#设置工程名称
#project(EvisionSandbox)
 
# Find OpenCV, you may need to set OpenCV_DIR variable
# to the absolute path to the directory containing OpenCVConfig.cmake file
# via the command line or GUI
find_package(OpenCV REQUIRED)
find_package(PCL REQUIRED)
# If the package has been found, several variables will
# be set, you can find the full list with descriptions
# in the OpenCVConfig.cmake file.
# Print some message showing some of them
message(STATUS "OpenCV library status:")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")


find_path(QT_INCLUDE_MultimediaWidgets QtMultimediaWidgets_Path)
find_path(QT_INCLUDE_Multimedia QtMultimedia_Path)
find_path(QT_INCLUDE_CORE_PRIVATE QtCore_private_path)
find_path(QT_INCLUDE_CORE_INNER QtCore_version_QtCore_path)
# Add OpenCV headers location to your include paths
include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(${PCL_INCLUDE_DIRS})
include_directories(${QT_INCLUDE_MultimediaWidgets})
include_directories(${QT_INCLUDE_Multimedia})
include_directories(${QT_INCLUDE_CORE_PRIVATE})
include_directories(${QT_INCLUDE_CORE_INNER})
#设置工程包含当前目录
set(CMAKE_INCLUDE_CURRENT_DIR ON) 
 
add_definitions(-D QT_CORE_LIB)
add_definitions(-D QT_GUI_LIB)
add_definitions(-D QT_WIDGETS_LIB)
add_definitions(-D QT_MULTIMEDIA_LIB)
add_definitions(-D QT_MULTIMEDIAWIDGETS_LIB)
add_definitions(-D QT_IMAGE_LIB)
add_definitions(-D QT_CORE_PRIVATE)
add_definitions(-D OPENCV)
add_definitions(-D WITH_PCL)
add_definitions(-D WITH_CUDA)
add_definitions(-D WITH_VTK)



#自动moc,uic,qrc,
#set(CMAKE_AUTOMOC ON) 
#set(CMAKE_AUTORCC ON)
#set(CMAKE_AUTOUIC ON)

#查找需要的Qt库文
find_package(Qt5 COMPONENTS Widgets REQUIRED)
find_package(Qt5 COMPONENTS Core REQUIRED)
find_package(Qt5 COMPONENTS Gui REQUIRED)
find_package(Qt5 COMPONENTS UiPlugin REQUIRED)
#查找当前文件夹中的所有源代码文件
FILE(GLOB SRC_FILES "./*.cpp") 
#查找当前文件夹中所有的头文件
FILE(GLOB HEAD_FILES "./*.h") 
#查找当前文件夹中所有的ui文件
FILE(GLOB UI_FILES "./*.ui") 
#查找当前文件夹中所有的ui文件
FILE(GLOB RES_FILES "./*.qrc") 
#编译ui文件
qt5_wrap_ui(UIC_FILES ${UI_FILES})
#moc
qt5_wrap_cpp(MOC_FILES ${HEAD_FILES})
#编译qrc资源文件
qt5_add_resources(QRC_FILES ${RES_FILES})
 
#文件整理
source_group("Generated Files" FILES ${UIC_FILES} ${MOC_FILES} ${QRC_FILES})
source_group("Form Files" FILES ${UI_FILES})
source_group("Resource Files" FILES ${RES_FILES})


set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/../install/Debug/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/../install/Release/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/../install/Debug/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/../install/Release/bin)
set(CMAKE_DEBUG_POSTFIX "_d")
set_target_properties(${TARGET_NAME} PROPERTIES DEBUG_POSTFIX "_d")

#创建可执行文件工程
add_executable(EvisionSandbox ${SRC_FILES} ${HEAD_FILES} ${MOC_FILES} ${RES_FILES} ${UIC_FILES} ${QRC_FILES})

#链接Qt5依赖项
target_link_libraries(EvisionSandbox Qt5::Widgets Qt5::Core Qt5::Gui)