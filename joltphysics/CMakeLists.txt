set(PHYSICS_REPO_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/src")

set(DISABLE_CUSTOM_ALLOCATOR 1)
set(DEBUG_RENDERER_IN_DISTRIBUTION 1)

# X86 processor feature defaults
set(USE_SSE4_1 1)
set(USE_SSE4_2 1)
set(USE_AVX 1)
set(USE_AVX2 1)
set(USE_AVX512 0)
set(USE_LZCNT 1)
set(USE_TZCNT 1)
set(USE_F16C 1)
set(USE_FMADD 1)

include("${PHYSICS_REPO_ROOT}/Jolt/Jolt.cmake")

if(MSVC)
	set_target_properties(Jolt PROPERTIES MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()
