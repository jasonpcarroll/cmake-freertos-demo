cmake_minimum_required(VERSION 3.15)

project(FreeRTOS-Kernel-Demo C ASM)

# ------------------------------------------------------------------------------
# Includes
# ------------------------------------------------------------------------------

include(FetchContent)

# ------------------------------------------------------------------------------
# Dependencies
# ------------------------------------------------------------------------------

# Target Support Repo - Gives needed CMake targets for the libraries used in
# this demo.

if(DEFINED TARGET_SUPPORT_REPO)

  # Pull requested hash if defined, otherwise pull main branch.

  if(NOT DEFINED TARGET_SUPPORT_REPO_TAG OR TARGET_SUPPORT_REPO_TAG STREQUAL "")
    set(TARGET_SUPPORT_REPO_TAG "main")
  endif()

  FetchContent_Declare(
    target_support_package
    GIT_REPOSITORY ${TARGET_SUPPORT_REPO}
    GIT_TAG ${TARGET_SUPPORT_REPO_TAG}
    GIT_PROGRESS TRUE)

  FetchContent_MakeAvailable(target_support_package)

endif()

# FreeRTOS-Kernel (gives freertos_kernel target) #

FetchContent_Declare(
  freertos_kernel
  GIT_REPOSITORY https://github.com/jasonpcarroll/FreeRTOS-Kernel.git
  GIT_TAG main
  GIT_PROGRESS TRUE)

FetchContent_MakeAvailable(freertos_kernel)

# ------------------------------------------------------------------------------
# Targets
# ------------------------------------------------------------------------------

# FreeRTOS based logging

target_link_libraries(logging_freertos PUBLIC freertos_kernel)

# FreeRTOS Demo (freertos_demo)

set(FREERTOS_DEMO_SOURCES
    # ${CMAKE_CURRENT_LIST_DIR}/source/AbortDelay.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/blocktim.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/BlockQ.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/comtest.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/comtest_strings.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/countsem.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/crflash.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/crhook.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/death.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/dynamic.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/EventGroupsDemo.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/flash.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/flash_timer.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/flop.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/GenQTest.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/integer.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/IntQueue.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/IntSemTest.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/MessageBufferAMP.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/MessageBufferDemo.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/PollQ.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/QPeek.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/QueueOverwrite.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/QueueSet.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/QueueSetPolling.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/recmutex.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/semtest.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/sp_flop.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/StaticAllocation.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/StreamBufferDemo.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/StreamBufferInterrupt.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/TaskNotify.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/TaskNotifyArray.c
    # ${CMAKE_CURRENT_LIST_DIR}/source/TimerDemo.c
    ${CMAKE_CURRENT_LIST_DIR}/main.c)

set(FREERTOS_DEMO_INCLUDE_DIRS ${CMAKE_CURRENT_LIST_DIR}/include)

add_executable(freertos_demo ${FREERTOS_DEMO_SOURCES})

target_link_libraries(freertos_demo PUBLIC freertos_kernel logging_freertos)

if(TARGET board_support_package)
  target_link_libraries(freertos_demo PUBLIC board_support_package)
endif()
