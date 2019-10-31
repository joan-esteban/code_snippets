
# Warning in VS C++
- https://stackoverflow.com/questions/2685598/warning-in-visual-studio
```c++
#define STRINGIZE_HELPER(x) #x
#define STRINGIZE(x) STRINGIZE_HELPER(x)
#define WARNING(desc) message(__FILE__ "(" STRINGIZE(__LINE__) ") : Warning: " #desc)

// usage:
#pragma WARNING(FIXME: Code removed because...)
```

***


# Build from commandline
- Just work with command.com

```
"C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\Community\\MSBuild\\15.0\\Bin\\amd64\\msbuild.exe" /m /p:Configuration=Release;Platform=x64;DefineConstants="NO_ORACLE;GPU" /t:Clean,Build .\myproject\myproject.sln
```

***

# Pass 'defines' to C++/CLI to build using msbuild.exe
- Using [this](https://docs.microsoft.com/en-us/cpp/build/reference/d-preprocessor-definitions?view=vs-2019) you can define a envoriment variable as:
```
set CL=/DY_DEFINE#0
```
- Reuse same `DefineConstants` as C#
- Edit *.vcxproj
- Search all entries `PreprocessorDefinitions`
- Add at end ;$(DefineConstants)
Check:
- https://stackoverflow.com/questions/14342492/msbuild-c-command-line-can-pass-defines
- https://stackoverflow.com/questions/479979/msbuild-defining-conditional-compilation-symbols
- https://docs.microsoft.com/en-us/cpp/build/reference/d-preprocessor-definitions?view=vs-2019
- https://stackoverflow.com/questions/2016697/msbuild-exe-not-accepting-either-pdefineconstants-nor-ppreprocessordefinitio


