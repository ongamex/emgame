cmake_minimum_required(VERSION 3.14)

##############################
# emgame_all
##############################
project(emgame_all)

if(EMSCRIPTEN)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -s DISABLE_DEPRECATED_FIND_EVENT_TARGET_BEHAVIOR=1")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -s WASM=1")
endif()

##############################
# External global libs.
##############################
if(EMSCRIPTEN)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -s USE_SDL=2")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -s USE_SDL_IMAGE=2")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -s SDL2_IMAGE_FORMATS=\"['png']\"")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -s ALLOW_MEMORY_GROWTH=1") # By default Emscripten has a fixed pool size for dynamic allocations.
else()
	set(SDL_STATIC OFF CACHE BOOL "  " FORCE)
	set(SDL_SHARED ON CACHE BOOL "  " FORCE)
	add_subdirectory(./libs_ext/SDL2)
endif()

##############################
# game
##############################
add_subdirectory(./game)