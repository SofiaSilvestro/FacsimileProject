cmake_minimum_required(VERSION 3.9)

# Definisce il nome del progetto
project(DFN LANGUAGES CXX C VERSION 1.0.0)

# Concateno stringhe
set(${CMAKE_PROJECT_NAME}_A a b)
message(${${CMAKE_PROJECT_NAME}_A})

# Ottengo due stringhe in output: "CurrentSourceDir" e la directory del file Cmake.
message(CurrentSourceDir : ${CMAKE_CURRENT_SOURCE_DIR})
message(CurrentBinaryDir: ${CMAKE_CURRENT_BINARY_DIR})

# Set C++ Standard
################################################################################
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# IMPOSE WARNINGS ON DEBUG
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall -Wextra -pedantic-errors")

# IMPOSE CXX FLAGS FOR WINDOWS
if (WIN32)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wa,-mbig-obj")
endif (WIN32)

# Add dependencies
################################################################################
set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH};${CMAKE_CURRENT_SOURCE_DIR}/../externals/Main_Install/eigen3)
set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH};${CMAKE_CURRENT_SOURCE_DIR}/../externals/Main_Install/googletest)

# ProjectName è equivalente a ${CMAKE_PROJECT_NAME}

## Eigen3
find_package(Eigen3 CONFIG REQUIRED)
list(APPEND ${CMAKE_PROJECT_NAME}_LINKED_LIBRARIES PUBLIC Eigen3::Eigen)

## Threads
find_package(Threads REQUIRED)
list(APPEND ${CMAKE_PROJECT_NAME}_LINKED_LIBRARIES PRIVATE Threads::Threads)

## GTest
find_package(GTest REQUIRED)

list(APPEND ${CMAKE_PROJECT_NAME}_LINKED_LIBRARIES PRIVATE GTest::gtest)
list(APPEND ${CMAKE_PROJECT_NAME}_LINKED_LIBRARIES PRIVATE GTest::gtest_main)
list(APPEND ${CMAKE_PROJECT_NAME}_LINKED_LIBRARIES PRIVATE GTest::gmock)
list(APPEND ${CMAKE_PROJECT_NAME}_LINKED_LIBRARIES PRIVATE GTest::gmock_main)

# Copy file
###############################################################################

# Insert Sources
################################################################################


# Create executable
################################################################################
# Creo il file main.cpp nella cartella, e includo i files GeometryLibrary.cpp e GeometryLibrary.hpp già presenti nel progetto.
add_executable(${CMAKE_PROJECT_NAME} main.cpp
    "${CMAKE_CURRENT_SOURCE_DIR}/GeometryLibrary.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/GeometryLibrary.hpp")


#target_compile_options(${PROJECT_NAME} PUBLIC -fPIC)
