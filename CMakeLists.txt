cmake_minimum_required(VERSION 3.0.2) # 3.8.2

project(helloworld)

# Does not work linux with CMake 3.7

# Find includes in the build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Turn on automatic invocation of the MOC
set(CMAKE_AUTOMOC ON)

# There may be a way to tell up front if Qt5 is going to be found, but I haven't found
# a foolproof way to do it yet, so settle for the default error message for now.
#if(NOT CMAKE_PREFIX_PATH AND NOT Qt5Widgets_DIR)
#    message(WARNING "CMAKE_PREFIX_PATH is not defined, so find_package may not work. Set the CMAKE_PREFIX_PATH "
#            "environment variable to the install prefix of Qt 5, either on the command line as "
#            "-DCMAKE_PREFIX_PATH=\"path/to/Qt5/lib/cmake\" or with set(CMAKE_PREFIX_PATH path/to/Qt5/lib/cmake)")
#endif(NOT CMAKE_PREFIX_PATH AND NOT Qt5Widgets_DIR)

if (UNIX)
  set (CMAKE_PREFIX_PATH "/usr/lib/x86_64-linux-gnu/cmake/Qt5Widgets/")
endif()

# Add a compiler flag
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")

# Make this a GUI application on Windows
if(WIN32)
  set(CMAKE_WIN32_EXECUTABLE ON)
endif()

# Find the QtWidgets library
#find_package(Qt5Widgets REQUIRED)
find_package(Qt5 COMPONENTS Core Widgets REQUIRED)

# Generate code from ui files
qt5_wrap_ui(UI_HEADERS mainwindow.ui)

# Generate rules for building source files from the resources
qt5_add_resources(QRCS resources.qrc)

# Tell CMake to create the helloworld executable
add_executable(helloworld main.cpp mainwindow.cpp ${UI_HEADERS} ${QRCS})

if (UNIX)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
  include_directories(/usr/include/x86_64-linux-gnu/qt5/QtWidgets)
  include_directories(/usr/include/x86_64-linux-gnu/qt5)
endif()

#get_target_property(QtCore_location Qt5::Core LOCATION)
#message(${QtCore_location})
#get_target_property(QtWidgets_location Qt5::Widgets LOCATION)
#message(${QtWidgets_location})

# Add the Qt5 Widgets for linking
if (WIN32)
  target_link_libraries(helloworld Qt5::Widgets Qt5::WinMain)
else()
  target_link_libraries(helloworld Qt5Core Qt5Widgets Qt5Gui)
endif()
