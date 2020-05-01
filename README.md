ReShade (Mafia III fix)
=======

This version of ReShade is specific to fix Mafia III fullscreen refresh rate.
When going (or starting) fullscreen, the game is forcing a refresh rate of 60, unless 60 isn't supported by the monitor, despite the game being perfectly capable of running at higher frame rates and at higher refresh rates when windowed.
With this mod, it uses the current monitor default setting, you can even change it after the game has started and it refreshes correctly.
V-Sync still works correctly.

I couldn't be bothered to write my own custom injector and then proxy it to ReShade or viceversa, so, given that ReShade already had a hook on the function that needed overriding, and that ReShade is a must to fix the blurriness and orange filter this game has, I thought I'd just make a branch of it. The same fix was also possible with the SpecialK mod, but it caused crashed on startup and alt-tab.
This is based on ReShade master, but at this point ReShade seems very stable and Mafia III doesn't need any new features from it, so I don't even need to keep merging this with new versions.
=======

ReShade
=======

This is a generic post-processing injector for games and video software. It exposes an automated way to access both frame color and depth information and a custom shader language called ReShade FX to write effects like ambient occlusion, depth of field, color correction and more which work everywhere.

The ReShade FX shader compiler contained in this repository is standalone, so can be integrated into other projects as well. Simply add all `source/effect_*.*` files to your project and use it similar to the [fxc example](tools/fxc.cpp).

## Building

You'll need Visual Studio 2017 or higher to build ReShade and Python for the `gl3w` dependency. The [Vulkan SDK](https://vulkan.lunarg.com/sdk/home#windows) is required if building with Vulkan support.

1. Clone this repository including all Git submodules
2. Open the Visual Studio solution
3. Select either the "32-bit" or "64-bit" target platform and build the solution (this will build ReShade and all dependencies).

After the first build, a `version.h` file will show up in the [res](/res) directory. Change the `VERSION_FULL` definition inside to something matching the current release version and rebuild so that shaders from the official repository at https://github.com/crosire/reshade-shaders won't cause a version mismatch error during compilation.

A quick overview of what some of the source code files contain:

|File                                                      |Description                                                            |
|----------------------------------------------------------|-----------------------------------------------------------------------|
|[dll_log.cpp](source/dll_log.cpp)                         |Simple file logger implementation                                      |
|[dll_main.cpp](source/dll_main.cpp)                       |Main entry point and test application when building for debug          |
|[dll_resources.cpp](source/dll_resources.cpp)             |Access to DLL resource data (e.g. built-in shaders)                    |
|[effect_lexer.cpp](source/effect_lexer.cpp)               |Lexical analyzer for C-like languages                                  |
|[effect_parser.cpp](source/effect_parser.cpp)             |Parser for the ReShade FX shader language                              |
|[effect_preprocessor.cpp](source/effect_preprocessor.cpp) |C-style preprocessor implementation                                    |
|[hook.cpp](source/hook.cpp)                               |Wrapper around MinHook which tracks associated function pointers       |
|[hook_manager.cpp](source/hook_manager.cpp)               |Automatic hook installation based on DLL exports                       |
|[input.cpp](source/input.cpp)                             |Keyboard and mouse input management and window message queue hooks     |
|[runtime.cpp](source/runtime.cpp)                         |Core ReShade runtime including effect and preset management            |
|[runtime_gui.cpp](source/runtime_gui.cpp)                 |Overlay GUI and everything related to that                             |
|[d3d9/runtime_d3d9.cpp](source/d3d9/runtime_d3d9.cpp)     |Effect runtime implementation for D3D9                                 |
|[d3d10/runtime_d3d10.cpp](source/d3d10/runtime_d3d10.cpp) |Effect runtime implementation for D3D10                                |
|[d3d11/runtime_d3d11.cpp](source/d3d11/runtime_d3d11.cpp) |Effect runtime implementation for D3D11                                |
|[d3d12/runtime_d3d12.cpp](source/d3d12/runtime_d3d12.cpp) |Effect runtime implementation for D3D12                                |
|[opengl/runtime_gl.cpp](source/opengl/runtime_gl.cpp)     |Effect runtime implementation for OpenGL                               |
|[vulkan/runtime_vk.cpp](source/vulkan/runtime_vk.cpp)     |Effect runtime implementation for Vulkan                               |

## Contributing

Any contributions to the project are welcomed, it's recommended to use GitHub [pull requests](https://help.github.com/articles/using-pull-requests/).

## License

All source code in this repository is licensed under a [BSD 3-clause license](LICENSE.md).
