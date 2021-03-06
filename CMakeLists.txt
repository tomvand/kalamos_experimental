cmake_minimum_required(VERSION 2.8)
project( HelloWorld )

# Add packages
find_package(PkgConfig REQUIRED)
# OpenCV
find_package( OpenCV REQUIRED )
MESSAGE( STATUS "OPENCV_VERSION: " ${OpenCV_VERSION} )
# SLAMDunk
pkg_search_module(KALAMOS_CONTEXT REQUIRED kalamos-context)
MESSAGE( STATUS "KALAMOS_CONTEXT_INCLUDE_DIRS: " ${KALAMOS_CONTEXT_INCLUDE_DIRS} )
MESSAGE( STATUS "KALAMOS_CONTEXT_LIBRARIES: " ${KALAMOS_CONTEXT_LIBRARIES} )
MESSAGE( STATUS "KALAMOS_CONTEXT_LIBRARY_DIRS : " ${KALAMOS_CONTEXT_LIBRARY_DIRS} )
MESSAGE( STATUS "KALAMOS_CONTEXT_LDFLAGS : " ${KALAMOS_CONTEXT_LDFLAGS} )
MESSAGE( STATUS "KALAMOS_CONTEXT_CFLAGS : " ${KALAMOS_CONTEXT_CFLAGS} )
include_directories(${KALAMOS_CONTEXT_INCLUDE_DIRS})
link_directories(${KALAMOS_CONTEXT_LIBRARY_DIRS})

# Compiler flags
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

# Build program
# hello
add_executable(hello hello.cpp)
target_link_libraries(hello ${OpenCV_LIBS})
# frameL
add_executable(frameL frameL.cpp)
target_link_libraries(frameL ${OpenCV_LIBS} ${KALAMOS_CONTEXT_LIBRARIES})
# calibrate
#add_executable(calibrate calibrate.cpp)
#target_link_libraries(calibrate ${OpenCV_LIBS} ${KALAMOS_CONTEXT_LIBRARIES})
# kalamoscapture
add_library(kalamoscapture STATIC kalamoscapture.cpp kalamosrectify.cpp)
target_link_libraries(kalamoscapture ${OpenCV_LIBS} ${KALAMOS_CONTEXT_LIBRARIES})
# kalamoscapture_demo
add_executable(kalamoscapture_demo kalamoscapture_demo.cpp)
target_link_libraries(kalamoscapture_demo kalamoscapture ${OpenCV_LIBS} ${KALAMOS_CONTEXT_LIBRARIES})
# sgbm_demo
add_executable(sgbm_demo sgbm.cpp)
target_link_libraries(sgbm_demo kalamoscapture ${OpenCV_LIBS} ${KALAMOS_CONTEXT_LIBRARIES})
# flow_demo
add_executable(flow_demo flow.cpp)
target_link_libraries(flow_demo kalamoscapture ${OpenCV_LIBS} ${KALAMOS_CONTEXT_LIBRARIES})
# calibration_images
add_executable(calibration_images calibration_images.cpp)
target_link_libraries(calibration_images kalamoscapture ${OpenCV_LIBS} ${KALAMOS_CONTEXT_LIBRARIES})
# calibrate_fisheye_stereo
add_executable(calibrate_fisheye_stereo calibrate_fisheye_stereo.cpp)
target_link_libraries(calibrate_fisheye_stereo ${OpenCV_LIBS})
