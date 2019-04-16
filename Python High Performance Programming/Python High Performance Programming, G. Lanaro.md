This book provides a practical approach to optimising python codes. 
The text and instructions are well organised and easy to pick up in a short time for programmers comfortable with python.
I encourage everyone to buy this book.

---
### Application Design

First make it run, then make it right, finally make it fast. *Premature optimisation is the root of all evil - Donald Knuth*

### Optimisation Step 1: Setting up tests

Set up tests to check if your program is working correctly. Any subsequent optimisation will run the same tests to ensure that all outputs remain valid.

### Optimisation Step 2: Benchmark performance

Performance benchmark will let us know where are the bottlenecks in the program, and also inform us whether our optimisation provided better performance.
1. Time the **program execution**. This can be done using Unix `time` command in the terminal or using Python `timeit` module, `timeit.timeit()` function in the program.
2. Time each **method execution**. This can be done using Python `cProfile` module in the terminal. The book recommends KCachegrind software to graphically analyse the profile output.
3. Time **each line of code**. This can be done using Python `line_profiler` module, `@profile` decorator in the program.
4. **Disassemble** the code to reveal the number of bytecode instructions. This can be done using Python `dis` module, `dis.dis()` function in the program.

5. Profile the **memory usage** for each line of code. This can be done using Python `memory_profiler` module, `@profile` decorator.

### Optimisation Step 3: Follow heuristics

- replace algorithm with a more efficient one
- reduce number of instructions
- reduce number of assignment operations
- use cache or Python `__slots__`
- fully utilise Python standard library; codes in the libraries are usually already optimised
- use data structure best suited for each task
- use list comprehensions and generators instead of explicit loops

### Optimisation Step 4: Speed up array operations

This step is suitable for programs that perform element wise operations.
- Use NumPy library, `numpy.array`. It is faster when performing large array operations.
- Use `numexpr` package to optimize and compile array expressions by using CPU cache and multiple processors. Squeeze as many operations as possible into a single expression, making sure to execute a reduction operation last.

### Optimisation Step 5: Program in C

Use Cython to make your code go even faster by compiling your code to C.
- Cython source files `.pyx` can be compiled into C using `cython` command.
- the C file `.c` can be compiled to a Python extension module `.so` using the gcc compiler.
- alternatively, using `distutils` module, `setup` function, together with `Cython`, `cythonize` helper function to compile Cython modules.
- using `cdef` declares static types for variables.
- using `cdef` declares functions that are translated to native C with less overhead. `cdef inline` replaces the function head with the function body inline (recommended only for small functions).
- using `cdef` declares classes as extension types, which have static typed attributes stored in C struct for fast access.
- Cython uses definition files `.pxd` to declare interfaces. Interfaces are implemented in `.pyx` files with the same base name. Implemented functions can be imported by other scripts using `cimport`.

The same syntax for C arrays and pointers applies:
```
/* define a variable */
cdef double var_doub

/* get memory address of variable */
&var_doub

/* define a pointer variable (asterisk is part of the declaration), and assign it with a memory address */
cdef double *var_doub_ptr
var_doub_ptr = &var_doub

/* dereference a pointer to obtain a value stored in the memory address, by using special operator (asterisk) */
cdef double value
value = *var_doub_ptr

/* C arrays are allocated in memory, and stores data contiguously. Multidimensional array writes data to memory in row-major */
cdef double arr[10]

/* C array variables are also pointers, storing the address of the first array element */
arr == &arr[0]
```

- use `cimport numpy` to use NumPy arrays that directly act on memory area, avoiding overhead from python interpreters.
