---
transition: fade-out
---

# Python 3.12's Solution: PEP 669

Introducing a Python tracing tool by @laike9m.

<div class="grid grid-cols-3 gap-10 pt-4 -mb-6">

<div height=600>

<v-clicks>

![image](https://user-images.githubusercontent.com/2592205/95418789-1820b480-08ed-11eb-9b3e-61c8cdbf187a.png)

</v-clicks>
</div>

<v-click>

<div>

This program enables code execution tracing. However, it faces a significant issue: its performance is severely lacking.

</div>

</v-click>

<v-click>

<div>

But why is this?

</div>

</v-click>

</div>

---
transition: fade-out
---

# Python 3.12's Solution: PEP 669

<div class="grid grid-cols-3 gap-10 pt-4 -mb-6">

<v-clicks>

<div height=600>

The program is built on the `sys.settrace` function.

What are the limitations of this API?

</div>
</v-clicks>

<v-click>

<div>

It supports only five traceable events:

1. call
2. line
3. return
4. exception
5. opcode

More critically, its performance is suboptimal. `sys.settrace` is invoked for every frame evaluated by the Python VM.

</div>
</v-click>

<v-click>
<div>

Therefore, a new API is essential to address these issues.

PEP 669 comes to the rescue.

</div>
</v-click>

</div>

---
transition: fade-out
monaco: true
---

# Python 3.12's Solution: PEP 669

PEP 669 introduces a new API in Python 3.12.

<div class="grid grid-cols-3 gap-10 pt-4 -mb-6">

<v-click>

<div>

Developers can now monitor processes using a broader range of events.

</div>

</v-click>

<v-click>

<div>

We now have access to 16 new events for process monitoring:

1. PY_START
2. PY_RESUME
3. PY_THROW
4. PY_RETURN
5. PY_YIELD
6. PY_UNWIND
7. CALL
8. C_RETURN
9. etc...

</div>

</v-click>

<div>

<v-click>

Developers can also choose the level of hook implementation:

1. Global Callback
2. Code Block Callback

This extends beyond just global hooks.

As a result, the new API is 10x-50x faster than `sys.settrace`!

</v-click>

</div>

</div>

---
transition: fade-out
---

# Python 3.12's Solution: GH-96143

<div class="grid grid-cols-4 gap-10 pt-4 -mb-6">

<div>

<v-click>

In many situations, we need to analyze processes during production issues, such as:

1. Performance issues
2. Memory leaks
3. And more...

</v-click>

</div>

<div>

<v-click>

In the Linux ecosystem, we have many tools for process analysis. A common tool is `perf`. 

Before Python 3.12, `perf` results would often appear like this:

</v-click>

</div>

<div>

<v-click>

```text
+   39.87%     0.00%  python   [unknown]             [.] 0x00007fa3c7f68ba0
+   31.59%    31.58%  python   libpython3.12.so.1.0  [.] _PyObject_Free
+   26.22%     0.00%  python   [unknown]             [.] 0x00007fa3c7f59ec0
+   26.22%     0.00%  python   [unknown]             [.] 0x00007ffc04e23270
+   20.54%    20.53%  python   libpython3.12.so.1.0  [.] _PyObject_Malloc
+   15.12%    15.12%  python   libpython3.12.so.1.0  [.] _PyEval_EvalFrameDefault
+    8.28%     8.28%  python   ld-linux-x86-64.so.2  [.] __tls_get_addr
+    5.79%     5.79%  python   libpython3.12.so.1.0  [.] _PyLong_Add
+    4.18%     4.18%  python   libpython3.12.so.1.0  [.] long_dealloc
+    3.38%     3.38%  python   libpython3.12.so.1.0  [.] PyLong_FromLong
+    3.31%     3.31%  python   libpython3.12.so.1.0  [.] __tls_get_addr@plt
+    1.70%     1.70%  python   libpython3.12.so.1.0  [.] _Py_NewReference
+    1.69%     1.69%  python   libpython3.12.so.1.0  [.] _Py_NewReference@plt
+    1.65%     1.65%  python   libpython3.12.so.1.0  [.] PyObject_Free
+    1.62%     1.62%  python   libpython3.12.so.1.0  [.] PyObject_Malloc@plt
+    1.61%     0.00%  python   [unknown]             [.] 0x00007fa3c75b3d30
+    0.98%     0.98%  python   libpython3.12.so.1.0  [.] PyLong_FromLong@plt
```

</v-click>

</div>

<div>

<v-click>

This format can be difficult to interpret, right? This is because Python is a dynamic language, which complicates the extraction of symbol information from binaries.

</v-click>

</div>

</div>

---
transition: fade-out
---

# Python 3.12's Solution: GH-96143

The landscape changes with Python 3.12.

<div class="grid grid-cols-3 gap-10 pt-4 -mb-6">

<div>

<v-click>

In Python 3.12, developers have introduced a trampoline for each code block running in CPython. This gives dynamic code a stable memory address, enabling more effective tracing and problem-solving.

</v-click>

</div>

<div>

<v-click>

```text
+   99.96%     0.00%  python   libc.so.6             [.] __libc_start_call_main
+   99.96%     0.00%  python   libpython3.12.so.1.0  [.] Py_BytesMain
+   99.93%    13.75%  python   libpython3.12.so.1.0  [.] _PyEval_EvalFrameDefault
+   99.92%     0.00%  python   libpython3.12.so.1.0  [.] PyEval_EvalCode
+   99.91%     0.00%  python   libpython3.12.so.1.0  [.] PyObject_Vectorcall
+   99.88%     0.00%  python   libpython3.12.so.1.0  [.] Py_RunMain
+   99.88%     0.00%  python   libpython3.12.so.1.0  [.] pymain_run_python.constprop.0
+   99.88%     0.00%  python   libpython3.12.so.1.0  [.] _PyRun_AnyFileObject
+   99.88%     0.00%  python   libpython3.12.so.1.0  [.] _PyRun_SimpleFileObject
+   99.87%     0.00%  python   libpython3.12.so.1.0  [.] run_mod
+   99.87%     0.00%  python   libpython3.12.so.1.0  [.] run_eval_code_obj
+   99.87%     0.00%  python   [JIT] tid 208391      [.] py::<module>:/root/opendal-demo/demo2.py
+   99.87%     0.00%  python   [JIT] tid 208391      [.] py::baz:/root/opendal-demo/demo2.py
+   99.87%     0.00%  python   [JIT] tid 208391      [.] py::bar:/root/opendal-demo/demo2.py
+   99.87%     0.00%  python   [JIT] tid 208391      [.] py::foo:/root/opendal-demo/demo2.py
+   29.15%    29.15%  python   libpython3.12.so.1.0  [.] _PyObject_Free
+   25.38%    25.38%  python   libpython3.12.so.1.0  [.] _PyObject_Malloc
+   20.82%     5.30%  python   libpython3.12.so.1.0  [.] _PyLong_Add
+   18.71%     3.83%  python   libpython3.12.so.1.0  [.] PyLong_FromLong
+    5.81%     5.81%  python   ld-linux-x86-64.so.2  [.] __tls_get_addr
```

</v-click>

</div>

<div>

<v-click>

This trampoline mechanism can also be utilized in tools like:

1. uprobes + eBPF
2. DTrace/SystemTap
3. And others...

</v-click>

</div>

</div>

---
transition: fade-out
---

# Python 3.12's Solution: PEP 697

Stable ABI is a key feature in Python 3.12.

<div class="grid grid-cols-3 gap-10 pt-4 -mb-6">

<div>

<v-click>

When writing a Python extension, there are several factors to consider:

1. The current Python version
2. The Python version you wish to support
3. And so on...

</v-click>

</div>

<div>

<v-click>

Post Python 3.12, core developers consistently strive to maintain a stable ABI.

- The Limited API has been in place since Python 3.2
- PEP 652

</v-click>

</div>

<div>

<v-click>

But is that enough?

No

</v-click>

</div>

</div>

---
transition: fade-out
---

# Python 3.12's Solution: PEP 697

Let's explore some examples.

<div class="grid grid-cols-3 gap-10 pt-4 -mb-6">

<div>

<v-click>

Suppose you want to extend basic Python data structures like `list`, `dict`, `set`, etc. You might write code like this:

</v-click>

</div>

<div>

<v-click>

\```c
typedef struct {
    PyListObject list;
    int state;
} SubListObject;
\```

</v-click>

</div>

<div>

<v-click>

But is this approach effective? No.

- `PyListObject` does not constitute a stable ABI
- `PyListObject` is an internal API
- The size of `PyListObject` may change in the future, leading to various issues such as page alignment.

</v-click>

</div>

</div>

---
transition: fade-out
---

# Python 3.12's Solution: PEP 697

With Python 3.12, everything changes.

<div class="grid grid-cols-2 gap-10 pt-4 -mb-6">

<v-clicks>

![Image](https://github.com/Zheaoli/zheaoli.github.io/assets/7054676/e1862b64-45d0-44a9-a5b6-53d0f0d02a3b)

Developers no longer need to worry about the intricacies of `PyListObject`.

</v-clicks>

</div>

---
transition: fade-out
---

# Python 3.12's Solution: PEP 684

The Global Interpreter Lock (GIL) is problematic, and its removal is a future goal. 

To achieve this, it's crucial to ensure Python code is thread-safe. 

The solution? Moving the GIL to the interpreter level.
