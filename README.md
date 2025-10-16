# Core.SamplePlugin

(WIP) Example of engine plugin

The plugin exposes `Urho3D::SampleComponent` as a C++ engine plugin.

## C# bindings

The plugin can also be used from C# through SharpCpp.Rbfx.
`CMakeLists.txt` calls `rbfx_csharp_add_wrapper()` only when this function exists, that is when the top-level
project did `find_package(SharpCppRbfx CONFIG REQUIRED)` before `add_subdirectory()` of the plugin.
Without SharpCpp.Rbfx the plugin is built as a pure C++ plugin.

The call makes the target `Plugin.Core.SamplePlugin.Wrapper`. The C# namespace of the bindings is `Urho3D`,
the assembly is `Plugin.Core.SamplePlugin.Bindings`. Pass the target to the C# plugin of the application:

```cmake
add_subdirectory (Plugins/Core.SamplePlugin)
rbfx_csharp_add_plugin (MyApp.Managed REFERENCES Plugin.Core.SamplePlugin.Wrapper)
```

The C# code then creates the component:

```csharp
using var component = new Urho3D.SampleComponent(context);
component.SetRotationSpeed(90.0f);
```
