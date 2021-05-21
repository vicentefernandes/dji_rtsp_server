cmake_minimum_required(VERSION 2.8)
project(dji_RTSP)

find_package( OpenCV REQUIRED)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/common)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/hal)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/osal)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/libs)

find_package(PkgConfig REQUIRED)

pkg_check_modules(GLIB REQUIRED glib-2.0)
pkg_check_modules(GSTREAMER REQUIRED gstreamer-1.0)
pkg_check_modules(GSTREAMERRTSP REQUIRED gstreamer-rtsp-1.0)
pkg_check_modules(GSTREAMERRTSPSERVER REQUIRED gstreamer-rtsp-server-1.0)
pkg_check_modules(GSTREAMERAPP REQUIRED gstreamer-app-1.0)
pkg_check_modules(LIBUSB REQUIRED libusb-1.0)


set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DADVANCED_SENSING  -std=c++11 -fpermissive -fPIC -pthread -Ofast -g -O0")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DADVANCED_SENSING")



include_directories(${LIBUSB_INCLUDE_DIRS})
include_directories(
###  ${OpenCV_INCLUDE_DIRS} 
  ${GLIB_INCLUDE_DIRS} 
  ${GSTREAMER_INCLUDE_DIRS} 
  ${GSTREAMERRTSP_INCLUDE_DIRS} 
  ${GSTREAMERRTSPSERVER_INCLUDE_DIRS} 
  ${GSTREAMERAPP_INCLUDE_DIRS})


link_libraries(-lgstrtsp-1.0 -lgstsdp-1.0 -lgio-2.0 -lgstrtspserver-1.0 -lgstapp-1.0 -lgstbase-1.0 -lgstreamer-1.0 -lgobject-2.0 -lglib-2.0)
  

add_executable(${PROJECT_NAME}
  src/appsrc_rtsp_server.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/hal/osdkhal_linux.c
  ${CMAKE_CURRENT_SOURCE_DIR}/osal/osdkosal_linux.c
  ${CMAKE_CURRENT_SOURCE_DIR}/common/dji_linux_helpers.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/common/dji_linux_environment.cpp

)



target_link_libraries(${PROJECT_NAME} -ladvanced-sensing)
target_link_libraries(${PROJECT_NAME} -ldjiosdk-core -ldji-linker)
target_link_libraries(${PROJECT_NAME} -lgstrtsp-1.0 -lgstsdp-1.0 -lgio-2.0 -lgstrtspserver-1.0 -lgstapp-1.0 -lgstbase-1.0 -lgstreamer-1.0 -lgobject-2.0 -lglib-2.0 -lusb-1.0)
target_link_libraries(${PROJECT_NAME} -lavcodec -lavformat -lavutil -lswscale)


