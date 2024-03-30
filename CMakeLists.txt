# CMakeList.txt : CMake project for RefleXen, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

# Enable Hot Reload for MSVC compilers if supported.
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project ("RefleXen")

include(FetchContent)
FetchContent_Declare(
    DiligentCore
    GIT_REPOSITORY https://github.com/DiligentGraphics/DiligentCore.git
    SOURCE_DIR _deps/DiligentCore
)
FetchContent_Declare(
    DiligentTools
    GIT_REPOSITORY https://github.com/DiligentGraphics/DiligentTools.git
    SOURCE_DIR _deps/DiligentTools
)
FetchContent_Declare(
    DiligentFX
    GIT_REPOSITORY https://github.com/DiligentGraphics/DiligentFX.git
    SOURCE_DIR _deps/DiligentFX
)
FetchContent_MakeAvailable(DiligentCore DiligentTools DiligentFX)

# Add source to this project's executable.
add_executable (RefleXen "src/RefleXen.cpp" "src/RefleXen.h")

target_include_directories(RefleXen
PRIVATE
    ${diligentcore_SOURCE_DIR}
    ${diligenttools_SOURCE_DIR}
    ${diligentfx_SOURCE_DIR}
)

target_compile_definitions(RefleXen PRIVATE UNICODE)

target_link_libraries(RefleXen
PRIVATE
    Diligent-BuildSettings
    Diligent-GraphicsEngineD3D11-shared
    Diligent-GraphicsEngineD3D12-shared
    Diligent-GraphicsEngineOpenGL-shared
    Diligent-GraphicsEngineVk-shared
    DiligentFX
)
copy_required_dlls(RefleXen)

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET RefleXen PROPERTY CXX_STANDARD 20)
endif()

# TODO: Add tests and install targets if needed.
