cmake_minimum_required(VERSION 3.10)
project(MyCppProject)

add_executable(main main.cpp)
target_link_options(main PRIVATE -static-libstdc++)