# __getattr__ vs __getattribute__



A key difference between `__getattr__` and `__getattribute__` is that `__getattr__` is only invoked if the attribute wasn't found the usual ways. It's good for implementing a fallback for missing attributes, and is probably the one of two you want.

`__getattribute__` is invoked before looking at the actual attributes on the object, and so can be tricky to implement correctly. You can end up in infinite recursions very easily.

New-style classes derive from `object`, old-style classes are those in Python 2.x with no explicit base class. But the distinction between old-style and new-style classes is not the important one when choosing between `__getattr__` and `__getattribute__`.

You almost certainly want `__getattr__`.



下面的连接有很详细的讨论，已经够用了

https://stackoverflow.com/questions/3278077/difference-between-getattr-vs-getattribute

