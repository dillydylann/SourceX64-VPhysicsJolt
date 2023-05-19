cmake_minimum_required(VERSION 3.16)

set(SRCDIR "${CMAKE_CURRENT_SOURCE_DIR}/../..")
set(CMAKE_MODULE_PATH "${SRCDIR}/cmake")

function(main_project JOLT_TYPE)

if(JOLT_TYPE STREQUAL "AVX2")
	project(vphysics_jolt_avx2)
elseif(JOLT_TYPE STREQUAL "SSE42")
	project(vphysics_jolt_sse42)
elseif(JOLT_TYPE STREQUAL "SSE2")
	project(vphysics_jolt_sse2)
else()
	message(FATAL_ERROR "Unknown Jolt type!")
endif()

include(../joltphysics/joltphysics_settings.cmake)

include(source_dll_base)

target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_20)


## Source Files ##

target_sources(${PROJECT_NAME}
	PRIVATE
		"vjolt_collide.cpp"
		"vjolt_collide_trace.cpp"
		"vjolt_constraints.cpp"
		"vjolt_controller_fluid.cpp"
		"vjolt_controller_motion.cpp"
		"vjolt_controller_player.cpp"
		"vjolt_controller_vehicle.cpp"
		"vjolt_controller_shadow.cpp"
		"vjolt_debugrender.cpp"
		"vjolt_environment.cpp"
		"vjolt_friction.cpp"
		"vjolt_interface.cpp"
		"vjolt_keyvalues_schema.cpp"
		"vjolt_object.cpp"
		"vjolt_objectpairhash.cpp"
		"vjolt_parse.cpp"
		"vjolt_querymodel.cpp"
		"vjolt_surfaceprops.cpp"
	PRIVATE
		"${SRCDIR}/public/collisionutils.cpp")

## Header Files ##

file(GLOB SRC_HEADER_FILES "*.h")
target_sources(${PROJECT_NAME} PRIVATE ${SRC_HEADER_FILES})


## Public Header Files ##

file(GLOB SRC_PUBLIC_HEADER_FILES "${SRCDIR}/public/vphysics/*.h")
set(SRC_PUBLIC_HEADER_FILES ${SRC_PUBLIC_HEADER_FILES}
    "${SRCDIR}/public/vcollide.h"
    "${SRCDIR}/public/vcollide_parse.h"
    "${SRCDIR}/public/vphysics_interface.h")
source_group("Public Header Files" FILES ${SRC_PUBLIC_HEADER_FILES})
target_sources(${PROJECT_NAME} PRIVATE ${SRC_PUBLIC_HEADER_FILES})


## Link Libraries ##

if(JOLT_TYPE STREQUAL "AVX2")
	target_link_libraries(${PROJECT_NAME} joltphysics_avx2)
elseif(JOLT_TYPE STREQUAL "SSE42")
	target_link_libraries(${PROJECT_NAME} joltphysics_sse42)
elseif(JOLT_TYPE STREQUAL "SSE2")
	target_link_libraries(${PROJECT_NAME} joltphysics_sse2)
endif()

target_link_libraries(${PROJECT_NAME} mathlib tier2)

# System libs must be linked last!
target_link_libraries(${PROJECT_NAME} ${SYSTEM_LIBS})

endfunction()

main_project("AVX2")
main_project("SSE42")
main_project("SSE2")
