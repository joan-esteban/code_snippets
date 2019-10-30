
# Warning in VS C++
- https://stackoverflow.com/questions/2685598/warning-in-visual-studio
```c++
#define STRINGIZE_HELPER(x) #x
#define STRINGIZE(x) STRINGIZE_HELPER(x)
#define WARNING(desc) message(__FILE__ "(" STRINGIZE(__LINE__) ") : Warning: " #desc)

// usage:
#pragma WARNING(FIXME: Code removed because...)
```
# Build from commandline
- Just work with command.com

```
"C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\Community\\MSBuild\\15.0\\Bin\\amd64\\msbuild.exe" /m /p:Configuration=Release;Platform=x64;DefineConstants="NO_ORACLE;GPU" /t:Clean,Build .\myproject\myproject.sln
```
