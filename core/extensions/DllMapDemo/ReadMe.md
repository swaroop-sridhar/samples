# DllMap Demo

This samples illustrates the use of NativeLibrary APIs to implement library-name mappings similar to [Mono](https://www.mono-project.com/) [Dllmap](http://www.mono-project.com/docs/advanced/pinvoke/dllmap/) feature.

## NativeLibrary APIs

.Net Core 3 provides a rich set of APIs to manage native libraries:

- [NativeLibrary APIs](https://github.com/dotnet/corefx/blob/master/src/System.Runtime.InteropServices/ref/System.Runtime.InteropServices.cs#L728-L738): Perform operations on native libraries (such as `Load()`, `Free()`, get the address of an exported  symbol, etc.) in a platform-independent way from managed code.
- [DllImport Resolver callback](https://github.com/dotnet/corefx/blob/master/src/System.Runtime.InteropServices/ref/System.Runtime.InteropServices.cs#L734):  Get a call-back for first-chance native library resolution using custom logic. 
- [Native Library Resolve event](https://github.com/dotnet/corefx/blob/master/src/System.Runtime.Loader/ref/System.Runtime.Loader.cs#L39): Get an event for last-chance native library resolution using custom logic.   

## Library Mapping

The above APIs can be used to implement custom native library resolution logic, including DllMap, as illustrated in this example. The sample demonstrates:

- An [app](Demo.cs) that pInvokes a method in `OldLib`, but runs in an environment where only [`NewLib`](NewLib.cpp) is available.
- The [XML file](Demo.xml) that maps the library name from `OldLib` to `NewLib`. 
- The [Map](Map.cs) implementation, which parses the above mapping, and uses `NativeLibrary` APIs to load the correct library.

## Build and Run

1. Install .NET Core 3.0 Preview 3 or newer.

2. Use the .NET Core SDK to build the project via `dotnet build`.

3. Build the native component `NewLib.cpp` as a dynamic library, using the platform's native toolset. 

    Place the generated native library (`NewLib.dll` / `libNewLib.so` / `libNewLib.dylib`) in the `dotnet build` output directory.

4. Run the app with `dotnet run`
