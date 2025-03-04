# VML CMake scripts

This repository contains all CMake scripts used for VitaMonoLoader module compilation and AOT code generation.

## Implemented files

### VMLBuild.cmake

This is the main module which contains the following features:

__Utility functions__

- `get_mono_files(REFLIST)`: fills a variable with a list of files to add to `vita_create_vpk`, with all mandatory assemblies to run VitaMonoLoader

| **CMake Parameter**        | **Description**
|:-------------------|:------------------------------------------
|`REFLIST`         | Variable to store the list of files

__Compiling functions__

- `compile_mono_single_assembly_aot(ASSEMBLY CODE_FILE)`: generates an assembly from a single code file.

| **CMake Parameter**        | **Description**
|:-------------------|:------------------------------------------
|`ASSEMBLY`         | The name of the assembly to compile (including .dll)
|`CODE_FILE` | Path to a .cs file to compile

- `compile_mono_assembly_aot(ASSEMBLY CONFIG SOURCES REFERENCES FLAGS RESOURCES DEFINES)`: compiles an assembly from multiple source files and other advanced configurations

| **CMake Parameter**        | **Description**
|:-------------------|:------------------------------------------
|`ASSEMBLY`         | The name of the assembly to compile (without .dll)
|`CONFIG` | Path to the .dll.config file if needed
|`SOURCES` | List of .cs files to compile for mcs
|`REFERENCES` | List of dependency assemblies names for mcs (-r)
|`FLAGS` | Aditional flags to pass to mcs (without initial "-")
|`RESOURCES` | List of file resources to pack into the module for mcs (-resource)
|`DEFINES` | List of additional mono compiler defines for mcs (-define)

- `compile_mono_dll_aot(DLL_FILE)`: Generate AOT assembly files for an specific mono assembly

| **CMake Parameter**        | **Description**
|:-------------------|:------------------------------------------
|`DLL_FILE`         | Name of the dll file to generate AOT file for

NOTE: `compile_mono_assembly_aot` already generates AOT files when executed. Use `compile_mono_dll_aot` only to generate files for __assemblies generated by Unity Support for Vita__ (e.g.: System.Text.dll, etc)

- `compile_mono_external_dll_aot(ASSEMBLY TARGET LIBPATH REFERENCES)`: shorcut to generate AOT files for libraries inside the __Unity Support for Vita managed folder__

| **CMake Parameter**        | **Description**
|:-------------------|:------------------------------------------
|`ASSEMBLY`         | The name of the assembly to compile (including .dll)
|`LIBPATH`         | Path to Mono library if using custom installation
|`REFERENCES`         | List of dependency assemblies names

- `compile_mono_external_import(ASSEMBLY TARGET LIBPATH)`: includes an external library to the dependency chain, provided it has already been compiled to AOT and has an importable static library

| **CMake Parameter**        | **Description**
|:-------------------|:------------------------------------------
|`ASSEMBLY`         | The name of the assembly to compile (including .dll)
|`TARGET`         | Name of the CMake target to bind this file (needed for CMake dependency management)
|`LIBPATH`         | Path to Mono library if using custom installation

NOTE: for all calls, .dll and .dll.s files are generated and copied into `${CMAKE_BINARY_BIN}`, in order to reference all of them with mcs (may generate duplicates so be careful with space usage)
