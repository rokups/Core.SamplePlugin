cmake_minimum_required(VERSION 3.21)
project (Plugin.Core.SamplePlugin)

# Top-level plugin build is not supported yet.
# If the plugin is only built as subdirectory, this snippet is not needed and may be discarded.
if (PROJECT_IS_TOP_LEVEL)
    set (CMAKE_CXX_STANDARD 17)
    find_package (Urho3D REQUIRED)
endif ()

file (GLOB SOURCE_FILES *.h *.cpp)
add_plugin (${PROJECT_NAME} "${SOURCE_FILES}")

# Optional C# bindings. They need SharpCpp.Rbfx (find_package(SharpCppRbfx) in the top-level project).
# Without it the plugin is a pure C++ plugin.
# The C++ namespace of the bound types is Urho3D, so this is also the C# namespace of the bindings.
# The generated C++ code is added to the plugin library itself (the plugin is the IMPLEMENTATION target), so there is
# no separate native library of the bindings. The C# code imports its entry points from the plugin library.
# The assembly name is the target name, ${PROJECT_NAME}.Bindings (the default).
# The top-level project passes the bindings target to rbfx_csharp_add_project(... BINDINGS_TARGETS).
if (COMMAND rbfx_csharp_add_bindings)
    rbfx_csharp_add_bindings (${PROJECT_NAME}.Bindings
        HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/SampleComponent.h
        INCLUDE_DIRECTORIES ${CMAKE_CURRENT_SOURCE_DIR}
        IMPLEMENTATION ${PROJECT_NAME}
        NAMESPACE Urho3D
        SYMBOL_PREFIX sampleplugin
        GENERATED_DIRECTORY ${PROJECT_NAME}
    )
endif ()
