---
layout: post
title: "Python has dynamic scoping."
categories: tech rant
---

I recognize I should probably have learned this in my career by now. But chalk one more point up on my "Python is braindead" list.

```python
def step(message):
    print(message)

class Step(object):
    def run(self, context):
        print(self == step)
        print(self)
        print(step)
        step("1")

if __name__ == "__main__":
    context = {}
    procedure = [
        Step(),
    ]
    for step in procedure:
        step.run(context)
    print("Done.")
```

Running this with Python 3.10.6 produces the following output:

```
True
<__main__.Step object at 0x000001D65110BFD0>
<__main__.Step object at 0x000001D65110BFD0>
Traceback (most recent call last):
  File "E:\Source\marisalactation\python-does-not-have-lexical-scoping.py", line 17, in <module>
    step.run(context)
  File "E:\Source\marisalactation\python-does-not-have-lexical-scoping.py", line 9, in run
    step("1")
TypeError: 'Step' object is not callable
```

In other words, `step("1")` does **not** call the function defined a few lines above but rather at that point in code `step` references the for loop iteration variable `step` defined at the bottom of the file. Congratulations Python is, at least here, dynamically scoped.

I forget that languages do that.

The issue is, of course, not the presence of dynamic scoping (though I prefer visual signifiers to such) but the odd interaction between various scoping rules.

## EDIT:

I guess likely what is happening here is that `def step` binds that function implementation to the `step` global variable and then `for step` inside the `if` block is rebinding `step` with its iteration variable. Rather than actually having dynamic scoping.
