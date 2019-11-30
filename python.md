# Python

***

### Class use itself (for example: implements FluentInterface)
- Check fluentInterface (https://martinfowler.com/bliki/FluentInterface.html)
- Answer on stackoverflow: https://stackoverflow.com/questions/33533148/how-do-i-specify-that-the-return-type-of-a-method-is-the-same-as-the-class-itsel

```
from __future__ import annotations

class Foo:
  def set(self, data) -> Foo
    pass
```

***
