###############################################################################
# Test Macro
###############################################################################

macro(ecl_threads_add_gtest test_name)
  ament_add_gtest(ecl_test_${test_name} ${test_name}.cpp)
  # If we don't have gtest installed, then the target doesn't get made - check!
  if(TARGET ecl_test_${test_name})
    target_link_libraries(
      ecl_test_${test_name}
      ${PROJECT_NAME}
      ecl_config::ecl_config
      ecl_exceptions::ecl_exceptions
      ecl_time::ecl_time
    )
  endif()
endmacro()

###############################################################################
# Special Case
###############################################################################

# problems running as the jenkins user
set(ECL_JENKINS $ENV{ECL_JENKINS})
if("${ECL_JENKINS}" STREQUAL "")
  ament_add_gtest(ecl_test_mutex mutex.cpp)
  if(TARGET ecl_test_mutex)
    target_link_libraries(
      ecl_test_mutex
      ${PROJECT_NAME}
      ecl_config::ecl_config
      ecl_exceptions::ecl_exceptions
      ecl_time::ecl_time
    )
  endif()
endif()

###############################################################################
# Google Tests
###############################################################################

ecl_threads_add_gtest(priorities)
ecl_threads_add_gtest(threadable)
ecl_threads_add_gtest(threads)
