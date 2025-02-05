cmake_minimum_required(VERSION 3.16)
project(ThyroidClassification)

set(CMAKE_CXX_STANDARD 17)

# Define the paths for OpenCV and ONNX Runtime in the ROOT directory
set(OPENCV_DIR "C:/opencv/build")
set(ONNXRUNTIME_DIR "C:/onnxruntime")

# Include OpenCV headers
include_directories("${OPENCV_DIR}/include")
include_directories("${OPENCV_DIR}/include/opencv2")

# Include ONNX Runtime headers
include_directories("${ONNXRUNTIME_DIR}/include")

# Link directories for OpenCV and ONNX Runtime
link_directories("${OPENCV_DIR}/x64/vc16/lib")
link_directories("${ONNXRUNTIME_DIR}/lib")

# Add the executable
add_executable(ThyroidClassification main.cpp)

# Link against OpenCV and ONNX Runtime libraries
target_link_libraries(ThyroidClassification 
    opencv_world4100
    onnxruntime
)

# Ensure necessary DLLs are copied to the output directory
if (WIN32)
    add_custom_command(TARGET ThyroidClassification POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            "${OPENCV_DIR}/x64/vc16/bin/opencv_world4100.dll"
            "$<TARGET_FILE_DIR:ThyroidClassification>"

    )

    add_custom_command(TARGET ThyroidClassification POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            "${ONNXRUNTIME_DIR}/lib/onnxruntime.dll"
            "$<TARGET_FILE_DIR:ThyroidClassification>"

    )
endif()
