cmake_minimum_required(VERSION 2.8.3)
project(collvoid_wiggle_recovery)

find_package(catkin REQUIRED
        COMPONENTS
            cmake_modules
            roscpp
            tf
            costmap_2d
            nav_core
            pluginlib
            base_local_planner
        )

# Eigen3 FindScript Backward compatibility (ubuntu saucy)
# Since FindEigen.cmake is deprecated starting from jade.
if (EXISTS "FindEigen3.cmake")
  find_package(Eigen3 REQUIRED)
  set(Eigen_INCLUDE_DIRS ${Eigen3_INCLUDE_DIRS})
elseif (EXISTS "FindEigen.cmake")
  find_package(Eigen REQUIRED)
elseif (EXISTS "FindEigen.cmake")
  message(WARNING "No findEigen cmake script found. You must provde one of them,
  e.g. by adding it to ${PROJECT_SOURCE_DIR}/cmake_modules.")
endif (EXISTS "FindEigen3.cmake")

include_directories(
    include
    ${catkin_INCLUDE_DIRS}
    ${EIGEN_INCLUDE_DIRS}
)
add_definitions(${EIGEN_DEFINITIONS})

catkin_package(
    INCLUDE_DIRS include
    LIBRARIES collvoid_wiggle_recovery
    CATKIN_DEPENDS
        roscpp
        pluginlib
)

add_library(collvoid_wiggle_recovery src/collvoid_wiggle_recovery.cpp)
target_link_libraries(collvoid_wiggle_recovery ${catkin_LIBRARIES})

install(TARGETS collvoid_wiggle_recovery
       ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
       LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
       )

install(FILES rotate_plugin.xml
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
