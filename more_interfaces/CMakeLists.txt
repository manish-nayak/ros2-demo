cmake_minimum_required(VERSION 3.8)
project(more_interfaces)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(PYTHON_EXECUTABLE "/usr/bin/python3.8")
set(Python3_ROOT_DIR "/usr/bin/python3.8")
set(Python3_INCLUDE_DIR "/usr/include/python3.8")
set(Python3_LIBRARY "/usr/lib/x86_64-linux-gnu/libpython3.8.so")
message(STATUS "Python3 Interpreter: ${Python3_EXECUTABLE}")


# find_package(Python3 REQUIRED COMPONENTS Interpreter Development)
find_package(PythonInterp REQUIRED VERSION 3.8)  # Find Python version 3.8

message(STATUS "Python3 Interpreter: ${Python3_EXECUTABLE}")
find_package(Python3 3.8 EXACT REQUIRED COMPONENTS Interpreter)

find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(rclcpp REQUIRED)

add_executable(publish_address_book src/publish_address_book.cpp)
ament_target_dependencies(publish_address_book rclcpp)

set(msg_files
  "msg/AddressBook.msg"
)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
)

install(TARGETS 
  publish_address_book
  DESTINATION lib/${PROJECT_NAME}
)

rosidl_target_interfaces(publish_address_book
  ${PROJECT_NAME} "rosidl_typesupport_cpp")

ament_export_dependencies(rosidl_default_runtime)

ament_package()