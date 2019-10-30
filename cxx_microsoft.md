
# Warning in VS C++
- https://stackoverflow.com/questions/2685598/warning-in-visual-studio
```c++
#define STRINGIZE_HELPER(x) #x
#define STRINGIZE(x) STRINGIZE_HELPER(x)
#define WARNING(desc) message(__FILE__ "(" STRINGIZE(__LINE__) ") : Warning: " #desc)

// usage:
#pragma WARNING(FIXME: Code removed because...)
```
