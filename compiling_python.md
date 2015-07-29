**Pluses:**

First: mild, defeatable obfuscation.

Second: if compilation results in a significantly smaller file, you will get faster load times. Nice for the web.

Third: Python can skip the compilation step. Faster at intial load. Nice for the CPU and the web.

Fourth: the more you comment, the smaller the `.pyc` or `.pyo` file will be in comparison to the source `.py` file.

Fifth: an end user with only a `.pyc` or `.pyo` file in hand is much less likely to present you with a bug they caused by an un-reverted change they forgot to tell you about.

Sixth: if you're aiming at an embedded system, obtaining a smaller size
file to embed may represent a significant plus, and the architecture is stable so drawback one, detailed below, does not come into play.

**Top level compilation**

It is useful to know that you can compile a top level python source file into a `.pyc` file this way:

    python -m py_compile myscript.py

This removes comments. It leaves `docstrings` intact. If you'd like to get rid of the `docstrings` as well (you might want to seriously think about why you're doing that) then compile this way instead...

    python -OO -m py_compile myscript.py

...and you'll get a `.pyo` file instead of a `.pyc` file; equally distributable in terms of the code's essential functionality, but smaller by the size of the stripped-out `docstrings` (and less easily understood for subsequent employment if it had decent `docstrings` in the first place). But see drawback three, below.

Note that python uses the `.py` file's date, if it is present, to decide whether it should execute the `.py` file as opposed to the `.pyc` or `.pyo` file --- so edit your .py file, and the `.pyc` or `.pyo` is obsolete and whatever benefits you gained are lost. You need to recompile it in order to get the `.pyc` or `.pyo` benefits back again again, such as they may be.

**Drawbacks:**

First: There's a "magic cookie" in `.pyc` and `.pyo` files that indicates the system architecture that the python file was compiled in. If you distribute one of these files into an environment of a different type, it will break. If you distribute the `.pyc` or `.pyo` without the associated `.py` to recompile or `touch` so it supersedes the `.pyc` or `.pyo`, the end user can't fix it, either.

Second: If `docstrings` are skipped with the use of the `-OO` command line option as described above, no one will be able to get at that information, which can make use of the code more difficult (or impossible.)

Third: Python's `-OO` option also implements some optimizations as per the `-O` command line option; this may result in changes in operation. Known optimizations are:

* `sys.flags.optimize` = 1
* `assert` statements are skipped
* `__debug__` = False

Fourth: if you had intentionally made your python script executable with something on the order of `#!/usr/bin/python` on the first line, this is stripped out in `.pyc` and `.pyo` files and that functionality is lost.

Fifth: somewhat obvious, but if you compile your code, not only can its use be impacted, but the potential for others to learn from your work is reduced, often severely.