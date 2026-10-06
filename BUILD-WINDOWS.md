# Windows build guide

This repo is the performance-remix project page and patch. The complete FalloutCraft source tree is currently taken from the upstream FalloutCraft repository and then modified with the performance change.

## 1. Install the Windows toolchain

Install:
- Git for Windows
- Visual Studio 2022 Community (or newer) with **Desktop development with C++**
- XMake 3.0+
- Java JDK 25 for the Minecraft half

The CommonLibF4 template requires XMake 3.0+ and a C++23 compiler (MSVC or Clang-CL).

## 2. Get the source

Open PowerShell and run:

    cd $HOME\source
    git clone --recurse-submodules https://github.com/zeyvu/FalloutCraft.git FalloutCraft
    cd FalloutCraft

Then download this remix's performance patch from this repository and apply the change to `FO4_ModFiles/fo_blocks.cpp`.

## 3. Build the Fallout 4 plugin

From the FalloutCraft directory:

    git clone --recurse-submodules https://github.com/libxse/commonlibf4-template

Copy FalloutCraft's plugin source into the template:

    Copy-Item FO4_ModFiles\*.cpp commonlibf4-template\src\
    Copy-Item FO4_ModFiles\*.h commonlibf4-template\src\
    Copy-Item FO4_ModFiles\xmake.lua commonlibf4-template\xmake.lua

Set the Fallout 4 installation path for the current terminal. Example:

    $env:XSE_FO4_GAME_PATH = 'C:\Program Files (x86)\Steam\steamapps\common\Fallout 4'

Open **x64 Native Tools Command Prompt for VS 2022** before running the build so MSVC is on PATH.

Then:

    cd commonlibf4-template
    xmake build -r

A successful build produces:

    commonlibf4-template\build\windows\x64\release\commonlibf4-template.dll

With `XSE_FO4_GAME_PATH` set, the template can also install the plugin into Fallout 4's `Data\F4SE\Plugins\` directory.

Without that environment variable, copy the DLL manually into:

    Fallout 4\Data\F4SE\Plugins\

## 4. Build the Minecraft half

Fabric:

    cd ..\fabric
    .\gradlew.bat build

The repository's Fabric wrapper uses Gradle 9.7.1 and its Java compiler target is Java 25.

The resulting JAR is in:

    fabric\build\libs\

NeoForge 1.21.1:

    cd ..\versions\1.21.1\neoforge
    .\gradlew.bat build

## 5. Test

Before changing the installed copy, keep a backup of the original FalloutCraft DLL.

Launch Fallout 4 through F4SE and load the same save/scene you use for the baseline.

Check:

    %USERPROFILE%\Documents\My Games\Fallout4\F4SE\commonlibf4-template.log

The remix adds a five-second renderer diagnostic reporting visible and stored Minecraft sections.

Compare:
- FPS in the same scene
- turning the camera through dense builds
- distant/off-screen builds
- returning to previously off-screen sections
- shadows/lighting when sections become visible

Do not claim an FPS gain until the modified build has been run and compared with the original.

## 6. Package for Melty

After the Windows build works, copy the built DLL/JAR and any required release files into the remix release layout. Melty's one-click validation should be run before publishing.

Original credit and licensing must stay with the FalloutCraft/SkyCraft sources.
