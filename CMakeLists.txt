cmake_minimum_required(VERSION 2.8.12)
project(st_handeye)

add_compile_options(-std=c++17)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake")
set(VISP_DIR ${VISP_DIR} "/home/raghav/visp/visp-build")
set(Eigen3_DIR ${Eigen3_DIR} "/usr/include/eigen3")
#set(OpenCV_DIR ${OpenCV_DIR} "/usr/local/include/opencv4/opencv2")
#list(APPEND CMAKE_PREFIX_PATH "/usr/local")

find_package(G2O REQUIRED)
find_package(VISP REQUIRED)
find_package(Eigen3 REQUIRED)

find_package(OpenCV 4.7.0 EXACT REQUIRED)
set(OpenCV4_LIBS ${OpenCV_LIBS})

find_package(OpenCV 3.4.19 EXACT REQUIRED)
set(OpenCV3_LIBS ${OpenCV_LIBS})

find_package(Boost REQUIRED filesystem program_options)

include_directories(
	include
        ${G2O_INCLUDE_DIRS}
	${VISP_INCLUDE_DIRS}
        ${EIGEN3_INCLUDE_DIRS}
	${Boost_INCLUDE_DIRS}
	${OpenCV_INCLUDE_DIRS}
)

link_directories(${G2O_LIBRARY_DIRS})
message("OpenCV_LIBS: ${OpenCV_LIBS}")
message("OpenCV_INCLUDE_DIRS: ${OpenCV_INCLUDE_DIRS}")

###########
## Build ##
###########

add_library(st_handeye_graph
  src/st_handeye_graph.cpp
)
target_link_libraries(st_handeye_graph
  ${Boost_LIBRARIES}
  ${OpenCV_LIBRARIES}
  ${G2O_TYPES_DATA}
  ${G2O_CORE_LIBRARY}
  ${G2O_STUFF_LIBRARY}
  ${G2O_SOLVER_PCG}
  ${G2O_SOLVER_CSPARSE}
  ${G2O_SOLVER_CHOLMOD}
  ${G2O_TYPES_SLAM3D}
  ${G2O_TYPES_SLAM3D_ADDONS}
)

add_library(st_handeye_graph_xray
  src/st_handeye_graph_xray.cpp
)
target_link_libraries(st_handeye_graph_xray
  ${Boost_LIBRARIES}
  ${OpenCV_LIBRARIES}
  ${G2O_TYPES_DATA}
  ${G2O_CORE_LIBRARY}
  ${G2O_STUFF_LIBRARY}
  ${G2O_SOLVER_PCG}
  ${G2O_SOLVER_CSPARSE}
  ${G2O_SOLVER_CHOLMOD}
  ${G2O_TYPES_SLAM3D}
  ${G2O_TYPES_SLAM3D_ADDONS}
)

add_library(st_handeye_visp
  src/st_handeye_visp.cpp
)
target_link_libraries(st_handeye_visp
  ${Boost_LIBRARIES}
  ${OpenCV_LIBRARIES}
  ${VISP_LIBRARIES}
)

# calibrate
add_executable(calibrate
  src/calibrate.cpp
)
target_link_libraries(calibrate
    ${OpenCV4_LIBS}
    st_handeye_graph
    st_handeye_visp
)
add_dependencies(calibrate
  st_handeye_graph
  st_handeye_visp
)

# calibrate charuco
add_executable(calibrate_charuco
  src/calibrate_charuco.cpp
)
target_link_libraries(calibrate_charuco
    ${OpenCV4_LIBS}
    st_handeye_graph
    st_handeye_visp
)
add_dependencies(calibrate_charuco
  st_handeye_graph
  st_handeye_visp)
