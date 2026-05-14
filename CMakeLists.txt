cmake_minimum_required(VERSION 3.10)

# Prevent in-source builds
if("${CMAKE_SOURCE_DIR}" STREQUAL "${CMAKE_BINARY_DIR}")
	message(SEND_ERROR "In-source builds are not allowed. Please create a 'build' folder and run cmake from there.")
	message(FATAL_ERROR "mkdir -p build && cd build && cmake .. && cmake --build ." )
endif()

project(authSSHviaOKTA LANGUAGES C)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

file(GLOB SOURCES "src/*.c")

add_library(${PROJECT_NAME} SHARED
	${SOURCES})

target_link_libraries(${PROJECT_NAME} PRIVATE
	m
	qrencode
	curl
	ssl
	crypto )

set_target_properties(${PROJECT_NAME} PROPERTIES
	VERSION 1.0.0
	SOVERSION 1
	PREFIX "pam_"
)

install(TARGETS ${PROJECT_NAME} DESTINATION /usr/lib/security)
