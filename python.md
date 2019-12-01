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
### PEP 526 -- Syntax for Variable Annotations
- https://www.python.org/dev/peps/pep-0526/
Examples:
~~~
# 'primes' is a list of integers
primes = []  # type: List[int]
stats = {}  # type: Dict[str, int]
stats: ClassVar[Dict[str, int]] = {}
~~~
