cmake_minimum_required(VERSION 3.15) # Проверка версии CMake.

# Проект
set(PROJECT_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(PROJECT_NAME Windows)
set(PROJECT_INCLUDE "${PROJECT_DIR}/Engine/Platform/Win/main.h")
set(PROJECT_SOURCE "${PROJECT_DIR}/Engine/Platform/Win/main.cpp")

#
set(CMAKE_CXX_STANDARD 17)

# Библиотеки

# Engine
set(ENGINE_NAME Engine)
set(ENGINE_DIR "${PROJECT_DIR}/${ENGINE_NAME}")
set(ENGINE_SOURCE "${ENGINE_DIR}/Source")

# Game
set(GAME_NAME Game)
set(GAME_DIR "${PROJECT_DIR}/${GAME_NAME}")
set(GAME_SOURCE "${GAME_DIR}/Source")

# jsoncpp
set(JSONCPP_NAME jsoncpp)
set(JSONCPP_DIR "${ENGINE_DIR}/ThirdParty/jsoncpp")
set(JSONCPP_INCLUDES "${JSONCPP_DIR}/include")
set(JSONCPP_SOURCE "${JSONCPP_DIR}/src")
set(JSONCPP_LIB "${PROJECT_DIR}/../build/Win/Engine/ThirdParty/jsoncpp/src/lib_json")

FILE(GLOB_RECURSE JSONCPP_ALL_INCLUDES ${JSONCPP_INCLUDES}/*.h)
FILE(GLOB_RECURSE JSONCPP_ALL_SOURCES ${JSONCPP_SOURCE}/*.cpp)

# Создание

project(${PROJECT_NAME})											# Соглашение
add_executable(${PROJECT_NAME} ${PROJECT_INCLUDE} ${PROJECT_SOURCE})# Создает исполняемый файл с именем main, из исходника main.cpp

# Библиотеки

# Engine
include_directories(${ENGINE_SOURCE})					# Заголовочные файлы библиотек
add_subdirectory(${ENGINE_DIR})							# Добавление подпроекта, указывается имя дирректории
target_link_libraries(${PROJECT_NAME} ${ENGINE_NAME})	# Линковка программы с библиотекой
link_directories(${ENGINE_DIR})

# Game
include_directories(${GAME_SOURCE})
add_subdirectory(${GAME_DIR})
target_link_libraries(${PROJECT_NAME} ${GAME_NAME})
link_directories(${GAME_DIR})

# jsoncpp
include_directories(${JSONCPP_INCLUDES})
link_directories(${JSONCPP_LIB})
add_library(${JSONCPP_NAME} STATIC ${JSONCPP_ALL_SOURCES})
add_subdirectory(${JSONCPP_SOURCE})
target_link_libraries(${PROJECT_NAME} ${JSONCPP_NAME})
