cmake_minimum_required(VERSION 3.16)
project(rdkafka_complex_consumer_example)
# include(/usr/local/lib/cmake/RdKafka/FindLZ4.cmake)
list(APPEND CMAKE_MODULE_PATH "/usr/local/lib/cmake/RdKafka/")
find_package(RdKafka 2.6.0 REQUIRED)
include_directories(/usr/include/librdkafka/)
add_executable(rdkafka_complex_consumer_example rdkafka_complex_consumer_example.cpp)
# find_package(rdKafka REQUIRED)
# find_path(RDKAFKA_INCLUDE_DIR rdkafkacpp.h
#     HINTS /usr/include/librdkafka
# )
# include_directories(${RDKAFKA_INCLUDE_DIR})
# target_link_libraries(${PROJECT_NAME} PUBLIC rdkafka++)
target_link_libraries(${PROJECT_NAME} PUBLIC /usr/local/lib/librdkafka++.so)