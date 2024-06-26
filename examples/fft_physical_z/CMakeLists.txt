file(GLOB files_fft_c2c fft_c2c_z.f90)
file(GLOB files_fft_r2c fft_r2c_z.f90)

include_directories(${CMAKE_SOURCE_DIR}/src)

add_executable(fft_c2c_z ${files_fft_c2c})
add_executable(fft_r2c_z ${files_fft_r2c})

target_link_libraries(fft_c2c_z PRIVATE decomp2d)
target_link_libraries(fft_r2c_z PRIVATE decomp2d)

# Run the test(s)
set(run_dir "${test_dir}/fft_c2c_z")
message(STATUS "Example dir ${run_dir}")
file(MAKE_DIRECTORY ${run_dir})
if (BUILD_TARGET MATCHES "gpu")
  file(COPY bind.sh DESTINATION ${run_dir})
  add_test(NAME fft_c2c_z COMMAND ${MPIEXEC_EXECUTABLE} ${MPIEXEC_NUMPROC_FLAG} ${MPIEXEC_MAX_NUMPROCS} ./bind.sh $<TARGET_FILE:fft_c2c_z> ${TEST_ARGUMENTS} WORKING_DIRECTORY ${run_dir})
else ()  
  add_test(NAME fft_c2c_z COMMAND ${MPIEXEC_EXECUTABLE} ${MPIEXEC_NUMPROC_FLAG} ${MPIEXEC_MAX_NUMPROCS} $<TARGET_FILE:fft_c2c_z> ${TEST_ARGUMENTS} WORKING_DIRECTORY ${run_dir})
endif ()
# 
set(run_dir "${test_dir}/fft_r2c_z")
message(STATUS "Example dir ${run_dir}")
file(MAKE_DIRECTORY ${run_dir})
if (BUILD_TARGET MATCHES "gpu")
  file(COPY bind.sh DESTINATION ${run_dir})
  add_test(NAME fft_r2c_z COMMAND ${MPIEXEC_EXECUTABLE} ${MPIEXEC_NUMPROC_FLAG} ${MPIEXEC_MAX_NUMPROCS} ./bind.sh $<TARGET_FILE:fft_r2c_z> ${TEST_ARGUMENTS} WORKING_DIRECTORY ${run_dir})
else ()  
  add_test(NAME fft_r2c_z COMMAND ${MPIEXEC_EXECUTABLE} ${MPIEXEC_NUMPROC_FLAG} ${MPIEXEC_MAX_NUMPROCS} $<TARGET_FILE:fft_r2c_z> ${TEST_ARGUMENTS} WORKING_DIRECTORY ${run_dir})
endif ()
