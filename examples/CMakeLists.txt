set(TARGET main)
add_executable(${TARGET} main.cpp)
install(TARGETS ${TARGET} RUNTIME)
target_link_libraries(${TARGET} PRIVATE bark ${CMAKE_THREAD_LIBS_INIT})
target_compile_features(${TARGET} PRIVATE cxx_std_11)
if(TARGET BUILD_INFO)
    add_dependencies(${TARGET} BUILD_INFO)
endif()

# TODO: make subdirs for them
set(TARGET quantize)
add_executable(${TARGET} ../quantize.cpp)
install(TARGETS ${TARGET} RUNTIME)
target_link_libraries(${TARGET} PRIVATE bark ${CMAKE_THREAD_LIBS_INIT})
target_compile_features(${TARGET} PRIVATE cxx_std_11)
if(TARGET BUILD_INFO)
    add_dependencies(${TARGET} BUILD_INFO)
endif()

