cmake_minimum_required(VERSION 3.14)

project(emgame)
include_directories("./src")

FILE(GLOB EMGAME_SRC src/*)

add_executable(emgame WIN32 ${EMGAME_SRC})

if(EMSCRIPTEN)
	set_target_properties(${PROJECT_NAME} PROPERTIES SUFFIX ".html")
else()
	target_link_libraries(emgame SDL2main SDL2)
endif()

set_property(TARGET emgame PROPERTY VS_DEBUGGER_COMMAND "${CMAKE_INSTALL_PREFIX}/emgame_bin/emgame.exe")
set_property(TARGET emgame PROPERTY VS_DEBUGGER_WORKING_DIRECTORY "${CMAKE_INSTALL_PREFIX}/emgame_bin")

# Install
if(NOT EMSCRIPTEN)
	install(TARGETS SDL2 DESTINATION emgame_bin)
endif()
install(TARGETS emgame DESTINATION emgame_bin)

# Assets
if(EMSCRIPTEN)
	set(PRELOADFILE "${CMAKE_CURRENT_LIST_DIR}/assets@assets/")
	message(${PRELOADFILE})
	set_target_properties(emgame PROPERTIES LINK_FLAGS "--preload-file ${PRELOADFILE}")
else()
	FILE(GLOB EMGAME_ASSETS assets/*)
	message (${EMGAME_ASSETS})
	install(DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/assets/ DESTINATION emgame_bin/assets)
endif()